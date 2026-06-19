# DVWA Lab 1: SQL Injection (Low Difficulty)

## Vulnerability Type
SQL Injection (SQLi)

## Payload Used 
1' OR '1'='1

## What Happened
Submitted the payload in the "User ID" field. Instead of returning 1 user, the database returned all 5 users in the system:
- admin / admin
- Gordon / Brown
- Hack / Me
- Pablo / Picasso
- Bob / Smith

## Why It Works
The application concatenates user input directly into the SQL query without sanitization.

Original query logic:
```sql
SELECT * FROM users WHERE user_id = [USER_INPUT]
```

With our payload, it becomes:
```sql
SELECT * FROM users WHERE user_id = '1' OR '1'='1'
```

Since `'1'='1'` is always TRUE, the WHERE clause returns all rows instead of filtering by user_id.

## Security Impact
- **Severity:** CRITICAL
- Attacker gains unauthorized access to all user data
- Can modify/delete database records
- Can potentially execute operating system commands (depending on database permissions)

## Real-World Example
E-commerce site login: attacker enters `' OR '1'='1` → bypasses authentication → accesses all customer orders and payment info

## How to Fix
1. Use **Prepared Statements (Parameterized Queries)**
```php
$stmt = $pdo->prepare("SELECT * FROM users WHERE user_id = ?");
$stmt->execute([$user_id]);
```

2. Use **Input Validation**
- Whitelist allowed characters
- Reject special characters like `'`, `"`, `;`, `--`

3. Use **Web Application Firewalls (WAF)**
- Detect and block SQLi patterns

## OWASP Reference
SQL Injection is #3 in OWASP Top 10 (2021)

## Tools Used
- DVWA (Docker)
- Browser

## Date Completed
June 19, 2026

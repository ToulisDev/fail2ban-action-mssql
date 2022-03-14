# MS SQL Server Action for fail2ban


## Installation

### Step 1:
Create a database to your MS SQL Server named fail2ban

### Step 2:
Create a user fail2ban that has permission to fail2ban database and save credentials to /root/.my.cnf-fail2ban (yes this file can be read by MySQL too).

The file inside of ".my.cnf-fail2ban" should be like this:

```
[client]
host="127.0.0.1"
port="1433"
user="fail2ban"
password="r@nd0m.p@ssψorp"
```

### Step 3:

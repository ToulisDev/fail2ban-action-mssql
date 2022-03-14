# MS SQL Server Action for fail2ban
This is a script to add banned ips from fail2ban to your MS SQL server using ban-action from fail2ban. (Tested on Ubuntu Server)

## Requirements
You have to get mssql-tools and fail2ban to get this script running.
```
sudo apt-get update
sudo apt-get install mssql-tools fail2ban
```

## Installation

### Step 1:
Create a database to your MS SQL Server named fail2ban and create a user fail2ban that has write/update permission to database (You can give dbowner permission too).

### Step 2:
Download the ".my.cnf-fail2ban" file using sudo permision and edit it with your favourite text editor.
You should change the ```<change me>``` with your password of user fail2ban.
After changing the password move the file to /root/ folder.

```
sudo wget https://raw.githubusercontent.com/ToulisDev/fail2ban-action-mssql/main/.my.cnf-fail2ban
sudo nano ./.my.cnf-fail2ban
sudo mv ./.my.cnf-fail2ban /root/.my.cnf-fail2ban
```
### Step 3:
Download config file for fail2ban service and move it to "/etc/fail2ban/action.d/" folder.

```
wget https://raw.githubusercontent.com/ToulisDev/fail2ban-action-mssql/main/banned_db.conf
mv ./banned_db.conf /etc/fail2ban/action.d/banned_db.conf
```
### Step 4:
Download the "fail2ban_banned_db" bash script, give chmod 0550 permission and move the file to "/usr/local/bin/fail2ban_banned_db".

```
sudo wget https://raw.githubusercontent.com/ToulisDev/fail2ban-action-mssql/main/fail2ban_banned_db
sudo chmod 0550
sudo mv ./fail2ban_banned_db /usr/local/bin/fail2ban_banned_db
```
### Step 5:
Add action to your "/etc/fail2ban/jail.local" file. Example:

```
[mssqld]
enabled = true
logpath = /var/opt/mssql/log/errorlog
maxfailures = 3
findtime = 600
bantime = 672h
filter = mssqld-auth
port = 1433
action = iptables-allports
         banned_db[name=mssqld, port="All-ports", protocol=tcp]     <--- name,port, protocol are optional you can add the action just with "banned_db"
```

And finally, restart your fail2ban-client.
```sudo fail2ban-client restart```




## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.


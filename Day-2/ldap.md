### Setup LDAP server.

1.Change Host name 
```
nano /etc/hostname
Set,
server.ldap.com
```
2. Reboot system 
```
init 6
```

3.Install packages
```
apt-get install slapd ldap-utils
```
Enter administrator password:

5. Configuration file changes,
```
nano /etc/ldap/ldap.conf
```
Remove hash brefore BASE ans URI line ,and add foillowing
```
BASE   dc=ldap,dc=com
URI  ldap://localhost:389
```

5. Reconfigure `Slapd`
```
dpkg-reconfigure slapd
```
You will be asked a series of questions about how you'd like to configure the software.
```
    Omit OpenLDAP server configuration? No
    DNS domain name?
        ldap.com
    Organization name?
        ldap.com
    Administrator password?
        123
        123
    Database backend to use? HDB
    Remove the database when slapd is purged? No
    Move old database? Yes
    Allow LDAPv2 protocol? No
```

7. ldapsearch -x

8. apt-get install phpldapadmin

9. nano /etc/phpldapadmin/config.php
```
    line  no. 286   “My Ldap server”     you can add anything here
    Line no 293      Enter ip address of your machine e.g. 192.168.1.226
    Line no. 300             dc=ldap,dc=com
    Line no. 326            dc=ldap
    Line no. 161   true
```

10. Restart service
```
/etc/init.d/apache2 restart
```
12. Open browser
 http://192.168.1.226/phpldapadmin <br>
 -login with password which is given during ldap installation
```
Click on login -> type password -> login
```

Click on <br>
```
 dc=ldap,dc-com(1) -> create new entry here -> create organizational unit -> e.g sales -> commit
```
 Click on
```
 ou=sales -> create child entry -> Generic : Posix Group -> e.g. sales-group -> commit 
```
 Click on 
```
 ou=sales-group -> create child entry -> Generic: User Account ->
```
 Fill the following Text boxes, <br>
  Common name , GID number, Home Directory, login shell, last name, password <br>
 -> ok -> commit <br>

[ NOTE : We can change the path of users home Directory on Client system by just defining it into the Home Directory text Box.] <br>


## LDAP Commands

- list all the users under some  group 
```
ldapsearch -x -b $dn -s sub "objectclass=posixGroup" | sed -n '/cn: $grp_name/,/dn:/p' | grep "^memberUid:" | awk '{print$2}' 
```
- List all created Users in LDAP
```
ldapsearch -x -b dc=ldap,dc=com -s sub "objectclass=posixAccount" | grep -i "^uid: " | awk '{print$2}' 
```

Modify LDAP group name usig LDIF file : /tmp/.modify.ldif
```
dn: cn=php,ou=Groups,dc=ldap,dc=com
changetype: modrdn
newrdn: cn=purval
deleteoldrdn: 1
```
```
ldapmodify -x -D "cn=admin,dc=ldap,dc=com" -w 123 -f /tmp/.modify
```
- Search All Users Password history
```
ldapsearch -LLL -x -h localhost -Z -D  cn=admin,dc=ldap,dc=com "(&(objectclass=Person))"  pwdHistory  -w {password}

dn: cn=purval,ou=Users,dc=ldap,dc=com
pwdHistory: 20170131064459Z#1.3.6.1.4.1.1466.115.121.1.40#6#123456
pwdHistory: 20170131093602Z#1.3.6.1.4.1.1466.115.121.1.40#6#123456
pwdHistory: 20170201053907Z#1.3.6.1.4.1.1466.115.121.1.40#6#123456
pwdHistory: 20170204074307Z#1.3.6.1.4.1.1466.115.121.1.40#6#123456

dn: cn=bhavesh,ou=Users,dc=ldap,dc=com

dn: cn=mehul,ou=Users,dc=ldap,dc=com

dn: cn=lokesh,ou=Users,dc=ldap,dc=com

dn: cn=khushbu,ou=Users,dc=ldap,dc=com
```

- if Print All the password History of purval user then,
```
ldapsearch -LLL -x -h localhost -Z -D  cn=admin,dc=ldap,dc=com "(&(objectclass=Person))"  pwdHistory  -w 123 | sed -n '/cn=purval/,/dn:/p' 
```
- Print all Users Password change time
```
ldapsearch -LLL -x -h localhost -Z -D  cn=admin,dc=ldap,dc=com "(&(objectclass=Person))"  modifyTimestamp -w {password}  
```

### Convert LDAP timezone to UTC to IST

- default LDAP server timezone is Asia/Zulu
time is displayed in UTC standard format e.g. 20170206055543Z

- To covert this time format in normal time 
```
 echo "20170402125623"  | sed -re 's/^([0-9]{8})([0-9]{2})([0-9]{2})([0-9]{2})$/\1\\ \2:\3:\4/'    | xargs date -u -d   >/tmp/.date
output is:  Mon Feb  6 11:25:43 UTC 2017 

 dateis=`cat /tmp/.date`
(This date format is UTC format, convert it to IST) 

eval "date --date='TZ=\"Asia/Zulu\" $dateis'"
output :Mon Feb  6 11:25:43 IST 2017
```
 

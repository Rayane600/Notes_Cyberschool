# Task 1 
1. 
```bash
useradd alice
passwd alice

useradd bob
```
2. 
-  The default hashing algorithm of my linux distro is yescrypt :y:
- No because in the /etc/pam.d/su we have `auth required pam_unix.so` which requires a password to switch users.
3. the fields are all the same except  the hash and the salt
4. No we can't log in as alice anymore 
5. Yes we still can login as alice after modifying the passwd.
- changing the password with `sudo passwd alice` modifies both the passwd file and the shadow file, it puts the x back in the passwd file to indicate that the password is in the shadow file and it puts the new hash in the shadow file

# Task 2 
1. the ones that are not checked are palindromes and birthday.
2. 

# Warming up 
## Q1 - 
-  Script : 
```
#!/bin/bash  
CACHE=$(echo "$CACHE";echo "line 1")  
CACHE=$(echo "$CACHE";echo "line 2")  
CACHE=$(echo "$CACHE";echo "line 3")  
echo "$CACHE"
```
The final `echo` outputs `line 1 line 2 line 3` because the first echo cache is empty.
- reverse the `echo "$CACHE";echo "line x"`
## Q2 - 
| Command        | Explanation                                                       |
|----------------|-------------------------------------------------------------------|
| cd /           | Changes to the root directory (/).                                |
| cd .           | Stays in the current directory (.).                               |
| cd /home/sabt  | Changes to /home/sabt directory (absolute path).                  |
| cd ../../      | Moves two levels up in the directory structure (relative path).   |
| cd ~           | Changes to the home directory of the user (~).                    |
| cd home        | Changes to the 'home' directory within the current directory.     |
| cd ../data/..  | Moves up one level from data directory, effectively staying in the current directory. |
| cd ..          | Moves up one level from the current directory (..).               |
		
## Q3 - 
- `head -n 15`
- `tail -n 15`
- `tail -n +15`
- `.` is a command that mean "include" `.source.sh` but it isn't a bash keyword which means you don't need to escape it to use it.
## Q4 - 
- `echo cat $filename>>all.pdb` will not print anything but it will redirect the `cat $filename` output to the file all.pdb
- `echo "cat $filename>>all.pdb"` will print everything in the quotes as it is except for the `$filename` because we have weak quoting " " to ignore everything all together we need to use single quote ' '
## Q5 -
- `touch * ` will update the timestamp of every file in the current directory *it will be re-translated by bash to `touch f1 f2 f3 ...` *
- `touch '*'` will create the `*` file if it doesn't exist or it will update it's timestamp if it already exists
# Bash Basics
## Q6 - 
- `ls | wc -l`
## Q7 -
- `tar -czf td1.tar Td_1_q1`
## Q8 -
- `head -21 text.txt| tail -6`
## Q9 - 
- `echo example.html | cut -d '.' -f 1`
## Q10 - 
- `jkqp=q4d6` will create a shell variable named `jkqp` with the value `q4d6`
## Q11 -
Script :
```bash
#!/bin/bash 
wc -l $@ | sort -n
```
# Bash Basics Plus
## Q12 - 
- the `--` will specify that everything after will not be an option
- `touch -- --help
- `rm -- --help`
## Q13 - 
- `echo $PATH | tr ':' "\n" |wc -l`
## Q14 - 
- `<in cat >out` will write what's in `in` into out 
- `<in cat >in` we will always have an empty file 
	- 1- open("in",R)
	- 2- open("in",w) <- truncate the "in" file
	- 3- Execute cat afterwards
## Q15 - 
Script:
```bash
#!/bin/bash
password=$1
if [ ${#password} -lt 8 ]; then
	echo "Password must have at least 8 characters."
	return 1
fi
passwordUP=$(echo $password | grep -o '[A-Z]' | wc -l)
if [ passwordUP -lt 2 ]; then 
	echo "Password mush contain at least 2 uppcase characters."
	return 1
fi
passwordDG=$(echo $password | grep -o '[0-9]' | wc -l)
if [ passwordUP -lt 1 ]; then
	echo "Password must contain at least one digit."
	return 1
fi
echo "Password is safe."
return 0 
```

## Q16 - 

`ls -l | tail -n +2 | tr -s ' ' | cut -d ' ' -f3 |sort |uniq -c`

## Q17 - 
`ls -l | grep '^-' | grep ' 0 ' | tr -s ' ' | cut -d ' ' -f 9`
- `ls -l` lists all the files in long format.
- `grep '^-` choses only the file (files start with a `-`):
	- `-`: Regular file
	- `d`: Directory
	- `l`: Symbolic link
	- `b`: Block special device
	- `c`: Character special device
	- `p`: Named pipe (FIFO)
	- `s`: Socket 
Professors solution : 
`ls | xargs -I _ bash -c '[ ! -s "_" ] && echo _'`
# Arithmetic
## Q18 -
```bash
#!/bin/bash
A=" string"
echo $((A+0))
A="4"
echo $((A+0))
```

- The first echo outputs 0 because the (()) forces bash to interpret what's inside as numbers and if it finds non numeric characters it will be automatically set to 0 thus 0+0=0
- The second echo outputs 4 since `"4"` is interpreted as a number so we get 4+0=4
## Q19 -
Script:
```bash
#!/bin/bash

if [ "$#" -eq 0 ]; then
	echo "can't have the average of nothing."
	exit
fi
sum=0
for numbers in "$@"; do 
	sum=$((sum+numbers))
done
average=$(($sum / $#))
echo "the average is : $average"
```
**/!\\ this only gives us the integer average it doesn't compute floating point division**

## Q20 -
Script:
```bash
#!/bin/bash
if [ "$#" -eq 0 ]; then
	echo "can't have the max of nothing."
	exit
fi
max=$1
for number in "$@"; do 
	if [ $number -gt $max ]; then
		max=$number;
	fi
done 
echo "the maximum number is : $max"
exit
```


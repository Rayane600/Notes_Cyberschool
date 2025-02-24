
# Task 1 

## 1-a
`lsof -p <ProcessPID>`

## 1-b
`lsof -u <UserName>`

## 2-a
 `strace cat /dev/null`
- openat(AT_FDCWD, "/dev/null", O_RDONLY) = 3 // opens /dev/null in raed_only
- read(3, "", 262144)                     = 0 // Reads /dev/null
- close(3)                                = 0  //Closes /dev/null 
- close(1)                                = 0 // Closes the stdout
- close(2)                                = 0 //Closes the stderr

# Task 2 

## 2.1
```bash
pid=$1
if [ ! -d "/proc/$pid" ]; then
    echo "Process $pid not found."
    exit 1
fi
echo "Listing open files for PID: $pid"
for fd in /proc/$pid/fd/*; do
   file=$(readlink "$fd")
   type=$(file -b i "$file")
   
 echo "Command: $(ps -p $pid -o comm=),Type:$type, File Path: $file"
 done

```

## 2.2 
```bash
pid=$1
interval=$2
if [ ! -d "/proc/$pid" ]; then
    echo "Process $pid not found."
    exit 1
fi
while (true); do
clear
echo "Listing open files for PID: $pid"
for fd in /proc/$pid/fd/*; do
   file=$(readlink "$fd")
   if [ -f "$file" ] 
   then
   type=$(file -b -i "$file") 
   echo "$(ps -p $pid -o comm=) | $type | $file"
   fi
   done
 sleep "$interval"
done
```

## 2.3 
```bash
user=$1
echo "Listing open files for user: $user"
for pid in $(pgrep -u "$user"); do
    echo "PID: $pid, Command: $(ps -p $pid -o comm=)"
    for fd in /proc/$pid/fd/*; do
        file=$(readlink -f "$fd")
        if [ -f "$file" ] 
        then
        type=$(file -b -i "$file" 2>/dev/null)
        echo "  Type: $type, File Path: $file"
        fi
    done
done

```
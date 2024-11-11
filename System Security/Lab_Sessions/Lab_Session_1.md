

## Task 1 : Navigating Files
- Combining `ls -tr` will list the files sorted by modification time, with the oldest files listed first and the newest files listed last.
	- `-t`: This option sorts the files by modification time, with the most recently modified files appearing first by default.
	- `-r`: This option reverses the sorting order, so instead of showing the most recent files first, it shows the oldest files first.
- The `touch` command is used to create an empty file or update the timestamp of an existing file (access and modification time).
	- One might use `touch` to create an empty file when preparing files for a project or when you want to update the modification time without editing its content.
- Script
```bash
#!/bin/bash

smallest=$(ls -S "$1" | tail -n 1)
second_largest=$(ls -S "$1" | head -n 2 | tail -n 1)

echo "Smallest file: $smallest"
echo "Second largest file: $second_largest"

```
- Command : `tar -cjf alkanes.tar.bz2 alkanes/`
## Task 2 : Pipes and Filters
- Command: `sort -n numbers.txt`
	- sorts the lines of the file numerically (based on the numeric value of the contents, not lexicographically).
- Commands :
```bash
touch animals-subset.csv
head -n 3 animals.csv > animals-subset.csv
tail -n 2 animals.csv >> animals-subset.csv
  
```
- Command:
	`find . -type f -exec wc -l {} + | sort -n | head -n 3`
	- `find . -type f`: This finds all regular files (ignoring directories) in the current directory.
	- `-exec wc -l {} +`: For each file found, it runs `wc -l` to count the number of lines.
- Command :
- `cat animals.csv | head -n 5 | tail -n 3 | sort -r > final.txt`
    - `cat animals.csv`: Displays the contents of `animals.csv`.
    - `head -n 5`: Retrieves the first 5 lines of the file.
    - `tail -n 3`: Extracts the last 3 lines from those 5.
    - `sort -r`: Sorts those 3 lines in reverse order .
    - `> final.txt`: Writes the sorted output into `final.txt`.
- Command : 
	`tr '1' 'l' < numbers.txt > temp.txt && mv temp.txt numbers.txt`
- Script : 
```bash
#!/bin/bash  
  
declare -A animal_counts  
  
while IFS=',' read -r date animal count  
do  
 if [[ -n "${animal_counts[$animal]}" ]]; then  
   animal_counts[$animal]=$((animal_counts[$animal] + count))  
 else  
   animal_counts[$animal]=$count  
 fi  
done < animals.csv  
echo "Animal counts:"  
for animal in "${!animal_counts[@]}"  
do  
 echo "$animal: ${animal_counts[$animal]}"  
done
```
## Task 3: Shell Scripts
- Command : `bash script.sh '*.pdb' 1 1`
	- The command searches for files that match the `*.pdb` pattern and displays the first and last line of the matching files so we expect the output to have the first and last line of every matching file.
- Script : 
```bash
#!/bin/bash  
longest_file=$(find "$1" -type f -name "*.$2" -exec wc -l {} + | sort -nr | head -n 1)  
echo "The file with the most lines is : $longest_file"
```
- Script : 
```bash 
#!/bin/bash  
for i in {0..9}  
do    
       echo ${i}  
done
```
- Script:
```bash 
#!/bin/bash  
total=0  
while read -r number; do  
 total=$((total + number))  
done < numbers.txt  
echo "Total: $total"
```
## Task 4: Finding Things
- Command : `wc -l $(find . -name "*.dat") | sort -n`
	- This script will output the line counts of all `.dat` files in the current directory, sorted in ascending order by the number of lines.
 - Script : 
 ```bash
 #!/bin/bash

sisters=("Jo" "Meg" "Beth" "Amy")
max_count=0
max_sister=""
for sister in "${sisters[@]}"
do
        count=$(grep -wo "$sister" LittleWomen.txt | wc -w)
        echo "$sister was mentionned $count times"

        if [ "$count" -gt "$max_count" ]; then
                max_count=$count
                max_sister=$sister
        fi
done
echo "the most mentionned sister is $max_sister and she was mentionned $max_count times"


```
# Globbing

## Q1 -
`touch *` : 
- if the current directory is not empty it will execute `touch` on every file in the directory
- if the current directory **is** empty it will create a file named `*`

# Find

## Q2 - 
`find . -type f -name "system"`
## Q3 -
`find . -type f -name "*.c"`
## Q4 - 
`find . -type f ! -name "*.c"`
## Q5 -
`find . -path ./Lab -prune -o -type f -name "*.c" -print`
## Q6 -
`find . -type f -name '???'`
## Q7 - 
`find . -type d -name "*[0-9]*"`
## Q8 -
`find ~ -samefile system`
## Q9 -
`find . -type f -perm -400 ! -perm /222`
# Grep
## Q10 - 
- `grep -c "pattern" filename`
- `grep -o "pattern" filename | wc -l`
- `grep -l "pattern" *`
- `grep -w "system" filename`
- `grep -E 'word1|word2' filename`
## Q11 -
`egrep '\\' text`
## Q12 - 
- `..` matches `ab`,`67`,`7b7`,`67777`,`o7x7g7`,`77777`,`7777`
- `^..$` also matches `ab`,`67`.
- `[a-e]` matches `ab`,`7b7`
- `^.7*$` matches `67`,`67777`,`77777`,`7777`,`g`
- `^(.7)*` matches `67`,`o7x7g7`,`7777` and `""` 
## Q13 - 
- a- `grep -E '^(ed|ted|fed)' filename`
- b- `grep -E '^[g0-9]' filename`
- c- `grep -E '(sam.*sam)' filename`
## Q14 -
`find . -type f -name "*.c" -exec grep -l "help" {} +`
## Q15 -
`grep -vE '^//|^$' code.c`
## Q16 -
`sed -i 's/sabt/great professor/g' filename`
## Q17 -
`sed 's/^\([0-9]\+\)/(\1)/' filename`
## Q18
`sed -n '/hello/p' filename`
## Q19 -
`sed -n '1,10p' filename`
## Q20 -
`awk '/hello/' filename`
## Q21 -
`awk 'END {print NR}' filename`
## Q22 -
`ls -l | awk '{total += $5} END {print total}'`
## Q23 -
`ls -l | awk '{if ($3=="rayane") {count+=1 } }END {print count}'`

# Project : HITORI

# Characteristics of C 
- *Imperative* language : describes computation in terms of statements that change a program state.
- *Compiled* language : Generates machine code, directly executed by the processor, efficient.
- Strongly typed (With flexibility (Cast))
- Simple
- Portable(standardized)
## First C program
```C
#include <stdio.h>
#define N 5
int tab[N] = {1,2,3,4,5};
int main() {
	int sum = 0;
	// Compute sum of elements in tab
	for (int i=0;i<N;i++) sum = sum+tab[i];
	// Print the result on standard output
	printf("Sum equals %d\n",sum);
	return(0);
}
```
## Preprocessing 
- the code now includes the whole code of the included libraries
- the constants defined are all replaced *(not an intelligent replacement)*
- **ONLY TEXT REPLACEMENT**

## Task 1: 

```C
#include <stdio.h>
#define N 100

int array[N];

void display_array(int array[], int size) {
	for (int i = 0; i < size; i++) {
		printf("{%d}", array[i]);
	}
	printf("\n");
}

int main() {
	int n;
	printf("Give VLA size : ");
	scanf("%d", &n);
	int vla[n];
	for (int i; i < 3; i++) {
		array[i] = 3;
	}
	for (int i = 3; i < 53; i++) {
		array[i] = 4;
	}
	display_array(array, N);

	printf("\n");
	printf("\n");
	for (int i; i < 3; i++) {
		vla[i] = 3;
	}
	for (int i = 3; i < 53; i++) {
		vla[i] = 4;
	}
	display_array(vla, n);
}

```
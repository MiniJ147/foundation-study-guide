# WELCOME TO FOUNDATION PREP
## Topics Covered

Credit: https://www.cs.ucf.edu/wp-content/uploads/2019/08/FE-ExamOutline.pdf


## Dynamic Memory Allocation
### WARNING
during foundation watch out for 
<ol>
    <li>Dangling Pointers</li>
    <li>Incorrect Data Type or size</li>
    <li>Un-freed mallocs</li>
    <li>Double ptr array being uninlitized</li>
</ol>

 ### Example Code Tracing
```c
int n = 10;
int *p1, *p2, *p3, **p4;
char str1[100] = "test string";
char *str2;
strcpy(str2,str1);
p1 = (int*)malloc(sizeof(int) * n);
p2 = (int*)malloc(sizeof(int) * n);
for(int i=0; i<n; i++){
    p1[i] = rand() % 100;
}
p2 = p1;
*p3 = 50;
p4 = (int **)malloc(n * sizeof(int*));
for(int i=0; i<n; i++){
    p4[i] = -5;
}
free(p1);
free(p2);
free(p4);
```
#### how many memory errors are in this code?

```
Well there are 4.

Error 1:

The first happens at line 5 --> strcpy(str2,str1); 

Why is this an error? Well, because strcpy(char* dest, char* src) so since the dest* is empty because *str2 was never allocated. It is trying to write memory into an invlaid spot

Error 2:

The second error happens at line 10 --> p2 = p1;
This is a dangling ptr. So we lose access to p2 because now we have set that ptr to p1. 

[IMPORTANT]: In these types of questions make sure you remember that p2 is dangling because it can cause further memory issues down the line.

Error 3:

The third Error is at line 11 --> *p3 = 50;
This happens because you are derefrencing a ptr that is not allocated. Thus you are pointing to invalid memory

Error 4:

The fourth error is at line 14 --> p4[i] = -5;

This happens because p4 is a **int. While p4 is allocated to a **int in the line above it --> p4 = (int **)malloc(n * sizeof(int*)); 

In reailty while p4 points to valid data alls of its ptrs under it don't put to anything has they have not been allocated. 

Thus, p4[i] = -5; is writting to invalid data / dangling ptrs

Error 5:

The last error is at line 16 --> free(p2);

This is because we did free(p1); just above it and if you recall p2 = p1; and since we freed p1, p2 now points to invalid memory.
```

Foundation Source:<br>
Question: https://www.cs.ucf.edu/registration/exm/sum2021/FE-May21.pdf<br>
Answer: https://www.cs.ucf.edu/registration/exm/sum2022/FE-May22-Sol.pdf

### How to malloc memory

```c

//valid
int a; //since it is on the static it is allocated
int *p_a = &a;

//valid
int *p_a = (int*)malloc(sizeof(int)); //inlizizted memory
int *p_a = 5; //derefrences and assgined data

int SIZE = 5;
int *arr = (int*)malloc(sizeof(int) * SIZE); //equal to arr[5] = {};

//NOW FOR DOUBLE PTRS
int a; //0x04
int *p_a = &a; //0x8
int **p_p_a = &p_a; //0xF

//  a = value
//  p_a = 0x4
//  p_p_a = 0x8

int n; int m;
int **arr2D = (int)malloc(sizeof(int*) * n);
for(int i=0; i<n; i++){
    arr2D[i] = (int*)malloc(sizeof(int)*m);
}

//creates
 int arr2D[n][m];
```

### How to free malloc

```c
int a; //doesnt need to be free since its on stack
int *p_a; //doesnt need to be free because p_a is on stack
int **pp_a; //same case

int *p_a = malloc... //this needs to be freed;
free(p_a);

int *p_a = malloc();
int **pp_a = &p_a;
//only p_a needs to be freed since **pp_a is declared on stack

//freeing 2d
//allocating
int n; int m;
int **arr2D = (int)malloc(sizeof(int*) * n);
for(int i=0; i<n; i++){
    arr2D[i] = (int*)malloc(sizeof(int)*m);
}

//freeing
for(int i=0; i<n; i++){
    free(arr2D[i]);
}
free(arr2D);

/*ADVICE
When allocating work outside in.
When freeing work inside out.
*/
```

**GENERAL RULE OF THUMB**<br>
**EVERY TIME YOU SEE MALLOC YOU NEED TO FREE**
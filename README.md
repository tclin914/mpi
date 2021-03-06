# MPI Programming
The purpose of this assignment is to familiarize yourself with MPI programming.

## Statement of Problem 1
In this problem, you need to use MPI to parallelize the following serial program ``prime.c``, which takes a `long long int` argument as an input, finds the largest prime number that is smaller than the input, and counts the prime numbers that are smaller than the input.

```c
#include <stdio.h>
#include <stdlib.h>
#include <math.h>

int isprime(int n) {
  int i,squareroot;
  if (n>10) {
    squareroot = (int) sqrt(n);
    for (i=3; i<=squareroot; i=i+2)
      if ((n%i)==0)
        return 0;
    return 1;
  }
  else
    return 0;
}

int main(int argc, char *argv[])
{
  int pc,       /* prime counter */
      foundone; /* most recent prime found */
  long long int n, limit;

  sscanf(argv[1],"%llu",&limit);    
  printf("Starting. Numbers to be scanned= %lld\n",limit);

  pc=4;     /* Assume (2,3,5,7) are counted here */

  for (n=11; n<=limit; n=n+2) {
    if (isprime(n)) {
      pc++;
      foundone = n;
    }           
  }
  printf("Done. Largest prime is %d Total primes %d\n",foundone,pc);

  return 0;
} 

```

## Statement of Problem 2
In this problem, you need to use MPI to parallelize the following serial program ``integrate.c``, which integrates function sin(X) over the range from 0 to pi using N intervals, where N is an argument of the program.

```c
#include <stdio.h>
#include <math.h>

#define PI 3.1415926535

int main(int argc, char **argv) 
{
  long long i, num_intervals;
  double rect_width, area, sum, x_middle; 

  sscanf(argv[1],"%llu",&num_intervals);

  rect_width = PI / num_intervals;

  sum = 0;
  for(i = 1; i < num_intervals + 1; i++) {

    /* find the middle of the interval on the X-axis. */ 

    x_middle = (i - 0.5) * rect_width;
    area = sin(x_middle) * rect_width; 
    sum = sum + area;
  } 

  printf("The total area is: %f\n", (float)sum);

  return 0;
}   

```

## Evaluation

### Prime

|number of iterations |20000000                |200000000
|---------------------|------------------------|------------------------|
|sequential           |14.3597421646 (seconds) |376.177364111 (seconds) |
|mpi in single machine|9.53690385818 (seconds) |151.067785978 (seconds) |
|mpi in multi machine |5.39104700089 (seconds) |124.963972092 (seconds) |

### Integrate

|number of iterations |200000000               |2000000000 
|---------------------|------------------------|------------------------|
|sequential           |10.6179821491 (seconds) |98.610033989 (seconds)  |
|mpi in single machine|3.88602399826 (seconds) |31.2693638802 (seconds) |
|mpi in multi machine |3.93596291542 (seconds) |31.0279841423 (seconds) |







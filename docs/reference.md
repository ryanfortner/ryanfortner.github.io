# Reference

Here are some links to some of my most used applications and sites as well as some simple programs to help me get things done.

### C Programming
#### Hello, World!
The famous beginners' program, which just prints Hello World and exits.
```c
#include <stdio.h>

int main(void)
{
	printf("Hello, world!\n");
	return 0;
}
```
#### Escape Sequences
Printing C's different escape sequences.
```c
#include <stdio.h>

int main(void) {
    printf("Hello, world without a new line!");
    printf("Hello, world with a new line!\n");
    printf("A string with \"quoted text\" inside of it\n");
    printf("Tabbed\tColumn\tHeadings\n");
    printf("The\tquick\tbrown\n");
    printf("fox\tjumps\tover\n");
    printf("the\tlazy\tdog\n\n");
    printf("A line of text that \nspans three lines \nand completes the line \n\n");
    return 0;
}
```
#### Operators
Example program using different types of operators in C.
```c
#include <stdio.h>

int main(void) {
    int a = 2;
    int b;

    // arithmetic operators
    b = a++; // b is 2, a is 3
    b = ++a; // b is 4, a is 4
    printf("a is %d, b is %d\n", a, b);
    printf("a * b = %d\n", a * b);
    printf("a + b = %d\n", a + b);
    printf("a - b = %d\n", a - b);
    printf("a / b = %d\n", a / b);

    // comparison operators
    // initial condition, test, increment 
	for (a = 0, b = 5; a < b; a++)
	{
        printf("a is equal to %d, b is equal to %d, but a is less than b so adding 1 to a.\n", a, b);
	}
	printf("the value of a is %d\n", a);
    a = 0;
    do
	{
		printf(" a is equal to %d\n", a);
		a++;
	} while (a < 5);

    // miscellaneous operators

    // ternary operator
    // condition ? value_if_true : value_if_false
    a = 10, b = 20;
    int c;
    c = (a < b) ? a : b;
    printf("%d\n", c); // 10

    // sizeof operator
    // returns the size of the operand you pass. You can pass a variable, or even a type
    int age = 37;
    printf("%ld\n", sizeof(age));
    printf("%ld\n", sizeof(int));

    // operator precedence
    a = 2, b = 4;
    c = b + a * a / b - a;

    //
}
```
### Size of Types
Prints the size of each type.
```c
#include <stdio.h>

int main(void) {
    printf("char size: %lu bytes\n", sizeof(char));
    printf("int size: %lu bytes\n", sizeof(int));
    printf("short size: %lu bytes\n", sizeof(short));
    printf("long size: %lu bytes\n", sizeof(long));
    printf("float size: %lu bytes\n", sizeof(float));
    printf("double size: %lu bytes\n", sizeof(double));
    printf("long double size: %lu bytes\n", sizeof(long double));
}
```
#### Get Time
Gets the current time and date.
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
 
int main(void)
{
    // variables to store the date and time components
    int hours, minutes, seconds, day, month, year;
 
    // set time-type to now
    time_t now;
 
    // Obtain current time
    // `time()` returns the current time of the system as a `time_t` value
    time(&now);
 
    // Convert to local time format and print to stdout
    printf("Today is %s", ctime(&now));
 
    // localtime converts a `time_t` value to calendar time and
    // returns a pointer to a `tm` structure with its members
    // filled with the corresponding values
    struct tm *local = localtime(&now);
 
    hours = local->tm_hour;         // get hours since midnight (0-23)
    minutes = local->tm_min;        // get minutes passed after the hour (0-59)
    seconds = local->tm_sec;        // get seconds passed after a minute (0-59)
 
    day = local->tm_mday;            // get day of month (1 to 31)
    month = local->tm_mon + 1;      // get month of year (0 to 11)
    year = local->tm_year + 1900;   // get year since 1900
 
    // print local time
    if (hours < 12) {    // before midday
        printf("Time is %02d:%02d:%02d am\n", hours, minutes, seconds);
    }
    else {    // after midday
        printf("Time is %02d:%02d:%02d pm\n", hours - 12, minutes, seconds);
    }
 
    // print the current date
    printf("Date is: %02d/%02d/%d\n", month, day, year);
 
    return 0;
}
```
#### Ask Name
This program will ask for your name and say it back to you.
```c
#include <stdio.h>
#include <stdlib.h>

int main (void)
{
  char name[15];

  printf("Enter your name: ");
  scanf("%s", name);
  printf("Hello, %s!\n", name);

  return 0;
}
```
#### Fibonacci Sequence
This program asks how many terms in the Fibonacci sequence you want to print.
```c
#include <stdio.h>

int main(void) {
    int numOfTerms, termsPrinted, t1 = 0, t2 = 1, nextTerm = t1 + t2;

    printf("print x terms in the fibonacci sequance.\n");
    printf("enter how many terms you want: ");
    scanf("%d", &numOfTerms);
    printf("ok, you want %d terms.\n", numOfTerms);

    // printing the first 2 terms so they can be added
    printf("Sequence: %d, %d, ",t1, t2);

    for (termsPrinted = 3; termsPrinted <= numOfTerms; ++termsPrinted) {
        printf("%d, ", nextTerm);
        t1 = t2;
        t2 = nextTerm;
        nextTerm = t1 + t2;
    }
}
```
#### Quadratic Roots Calculator
This program will calculate the roots of a standard-form quadratic equation.
```c
#include <stdio.h>
#include <math.h>

int main(void) {
    float a, b, c, d;
    
    printf("standard-form quadratic solver\n");
    printf("enter a, b and c: ");
    scanf("%f%f%f",&a,&b,&c);
    printf("ok, I got 'a = %f', 'b = %f', 'c = %f'. continuing.\n", a, b, c);

    // complete the b^2-4ac of the quadratic formula first
    d = b * b - 4 * a * c;

    if (d < 0) {
        printf("two complex roots found.\n");
        printf("%.3f%+.3fi",-b/(2*a),sqrt(-d)/(2*a));
        printf(", %.3f%+.3fi",-b/(2*a),-sqrt(-d)/(2*a));
        return 0;
    } else if (d == 0) {
        printf("one repeated root found.\n");
        printf("root: %.3f\n", -b / (2 * a));
        return 0;
    } else {
        printf("two real roots found\n");
        printf("roots: %.3f , %.3f\n", (-b + sqrt(d)) / (2 * a), (-b - sqrt(d)) / (d * a));
    }
    return 0;
}
```
#### Factor a Number
Asks for a number and then prints its factors.
```c
#include <stdio.h>

int main(void) {
    int numToFactor, i;
    printf("Enter the number you want to factor: ");
    scanf("%d", &numToFactor);
    // for loop is iterated until i is false
    // in each iteration, numToFactor is checked if it is divisible by i.
    // the condition is for i to be a factor of numToFactor.
    // then, the value of i is incremented by 1 and the process repeats.
    for (i = 1; i <= numToFactor; ++i) { // initial condition, test, increment
        if ((numToFactor % i) == 0) {
            printf("%d ", i);
        }
    }
    return 0;
}
```
### Links
- To-Do List: [https://ryanfortner.github.io/todolist/](https://ryanfortner.github.io/todolist/)
- Invuedious: [https://ryanfortner.github.io/invuedious/](https://ryanfortner.github.io/invuedious/)
- Pi-Apps AARCH64 nodes:
    - Node1: [https://node1.pi-apps.io](https://node1.pi-apps.io/)
    - Node2: [https://node2.pi-apps.io](https://node2.pi-apps.io/)
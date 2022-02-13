# C Programming
This is my page for posting code snippets from the C language.
### Hello World
Every programmer's first program.
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
#### Size of Types
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
#### Write Text to File
Simple program which writes text to a specific file on the disk.
```c
#include <stdio.h>
#include <stdlib.h>

int main(void) {
    char sentence[1000];
    FILE *fptr;

    // use appropriate location if you're using macos, linux, windows, etc
    fptr = fopen("program.txt", "w");

    // exiting program 
    if (fptr == NULL) {
        printf("Error!");
        exit(1);
    }
    printf("Enter a sentence:\n");
    fgets(sentence, sizeof(sentence), stdin);
    fprintf(fptr, "%s", sentence);
    fclose(fptr);
    return 0;
}
```
#### Read Text from File
Simple program which reads text from a file on the disk.
```c
#include <stdio.h>
#include <stdlib.h>

int main(void) {
    char text[100];
    FILE *fptr;

    if ((fptr = fopen("program.txt", "r")) == NULL) {
        printf("Error opening file!\n");
        
        // Program exits if the file pointer returns null.
        exit(1);
    }

    fscanf(fptr, "%s", &text);

    printf("Value of n = %s", text);
    fclose(fptr);

    return 0;
}
```
#### Simple Calculator
This is a simple calculator written in C.
```c
#include <stdio.h>

int main (int argc, char argv[]) {
    int arg1, arg2;
    if (argc == 4)
    {
        sscanf (argv[1], "%d", &arg1);
        sscanf (argv[3], "%d", &arg2);
        if (*argv[2] == '+') printf ("%d\n", arg1 + arg2);
        if (*argv[2] == '-') printf ("%d\n", arg1 - arg2);
        if (*argv[2] == 'x') printf ("%d\n", arg1 * arg2);
        if (*argv[2] == '/') printf ("%d\n", arg1 / arg2);
    }
    return 0;
}
```
#### Convert Temperature
Converts Celsius to Fahrenheight and vice-versa.
```c
#include <stdio.h>

// function prototypes
double celsiusToFahrenheight(double degreesC);
double fahrenheightToCelsius(double degreesF);

int main(void) {
    int c = 0, f = 32;
    printf("%d Celsius is %d Fahrenheight\n", c, (int)celsiusToFahrenheight(c));
    printf("%d Fahrenheight is %d Celsius\n\n", f, (int)fahrenheightToCelsius(f));

    c = 100, f = 212;
    printf("%d Celsius is %d Fahrenheight\n", c, (int)celsiusToFahrenheight(c));
    printf("%d Fahrenheight is %d Celsius\n\n", f, (int)fahrenheightToCelsius(f));

    c = f = 50;
    printf("%d Celsius is %d Fahrenheight\n", c, (int)celsiusToFahrenheight(c));
    printf("%d Fahrenheight is %d Celsius\n\n", f, (int)fahrenheightToCelsius(f));

    return 0;
}

// convert celsius to fahrenheight
double celsiusToFahrenheight(double degreesC) {
    double degreesF = (degreesC * 9 / 5)+ 32;
    return degreesF;
}

// convert fahrenheight to celsius
double fahrenheightToCelsius(double degreesF) {
    double degreesC = (degreesF - 32) * 5 / 9;
    return degreesC;
}
```
#### Prime Numbers
Asks you for a number and checks if it's prime or not.
```c
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>

bool isPrime(int num);

int main(void) {
    int num;
    printf("Check if a number is prime.\nEnter a number: ");
    scanf("%d", &num);
    if ((isPrime(num))==true) {
        printf("%d is prime.\n", num);
    } else if ((isPrime(num))==false) {
        printf("%d is not prime.\n", num);
    } else {
        printf("An error occured.\n");
    }
    return 0;
}

bool isPrime(int num) {
    if(num<2) return false;
    if(num==2) return true;

    bool isPrime=true; //make initial assumption that the number is prime
    for(int i=2; i<num; i++) {
        if((num%i)==0) {
            //we found a divisor of num, so num is not prime.
            isPrime = false;
            break; //no need to keep checking, so we can exit the loop.
        }
    }
    return isPrime;
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
#### Leap Year
This program ask for a year, and will tell you if it's a leap year or not.
```c
#include <stdio.h>
#include <stdbool.h>

// function prototypes
bool isLeapYear(int);

int main(void) {
    int year;

    printf("Determine if a year is a leap year or not.\n\n");
    printf("Enter year: ");
    scanf("%d", &year);

    /* A simple way of printing the result.
    if (isLeapYear(year)) {
        printf("%d is a leap year.\n");
    } else {
        printf("%d is not a leap year.\n", year);
    }*/

    // A more C-like version of printing the result
    printf("%d is%sa leap year.\n", year, isLeapYear(year) ? " " : " not ");

    return 0;
}

bool isLeapYear(int year) {
    bool isLeap = false;

    // Leap years not part of the Gregorian calendar until after 1752.
    if(year < 1571) // is it before the years it was known?
        isLeap = false;
    else if((year % 4) != 0) // year is not a multiple of 4.
        isLeap = false;
    else {
        if((year % 400) == 0)
            isLeap = true;
        else if((year % 100) == 0)
            isLeap = false;
        else
            isLeap = true;
    }
    return isLeap;
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
# lesson-5


## Basic Functions
### Arguments & Return Values
- Functions splits up the main function to more readable chunks
- Generally, you don't want functions to be too large (<100 lines)

Functions are in the form:
```
return-type function-name(parameter declarations, if any)
{
  declarations
  statements
  return
}
```
- The function declaration, called a function prototype, has to match with the function definition. It is an error if the definition of a function or any uses of it doe not agree with its prototype.
- Parameter names do not have to match, and are in fact optional. The prototype for a power() function can be written as ```int power(int m, int n)```; Or ```int power(int, int)```. Later in the file the function will be defined as 

```
int power(int base, int n) {...}
```
- The function must be declared and or defined before it is used in main.

## Example of Using a Function
- The variables used in power are local to the function, the i in power() is not related to the i in main(). These local variables are also referred to as automatic variables. 

- Just as the power function returns a value to the main function, the main function returns a value to its caller, which in effect is the environment in which the program was executed. Typically a return value of zero implies normal termination; non-zero values signal unusual or erroneous termination conditions.

```c
#include <stdio.h>

// K&R pg. 24-25
// power function

// Function prototype at the beginning
int power(int m, int n);

int main()
{
    int i;
    for (i=0; i<10; ++i)
        printf("%d %d %d\n", i, power(2,i), power(-3,i));
    return 0;
}

// power: raise base to n-th power; n >= 0
int power(int base, int n)
{
    int i, p;
    p = 1;
    for (i=1; i<=n; ++i)
        p = p * base;
    return p;
}
```
It is good to use functions - to make the code more readable, re-usability of functions, …

## External Variables 
The alternative to local variables are external variables. These are variables that are external to all functions, that is variables that can be accessed by name by any function. They are globally accessible. 

The external variable must be defined, exactly once, outside of any function; this sets aside storage for it. The variable must also be declared in each function that wants to access it; this states the type of the variable.

The declaration may be an explicit extern statement or may be implicit from context. The extern declaration can be omitted if the definition of the external variable occurs in the source file before its use in a particular function. In fact, its common practice to place definitions of all external variables at the beginning of the source file, and then omit all extern declarations. The extern declarations below are redundant.

If the program is in several source files, and a variable is defined in file1 and used in file2 and file3, then extern declarations are needed in file2 and file3 to connect the occurrences of the variable. The usual practice is to collect extern declarations of variables and functions in a separate file, historically called a header, that is included by #include at the front of each source file. The suffix .h is convention for header files.

_Definition_ = place where the variable is created or assigned storage

_Declaration_ = place where the nature of the variable is stated but no storage is allocated

```c
#include <stdio.h>
#define MAXLINE 1000

// external variables are declared here with their type, so storage is allocated for them
int max;
char line[MAXLINE];
char longest[MAXLINE];

int getnewline(void);
void copy(void);

int main()
{
    int len;
    extern int max;
    extern char longest[];

    max = 0;
    while ((len = getnewline()) > 0)
    {
        printf("\tlen: %d max: %d\n", len, max);
        if (len > max)
        {
            max = len;
            copy();
        }
    }

    if (max > 0)
        printf("%s", longest);

    return 0;
}

int getnewline(void)
{
    int c, i;
    extern char line[];
    for (i = 0; i < MAXLINE - 1 && (c = getchar()) != EOF && c != '\n'; ++i)
    {
        line[i] = c;
    }

    if (c == '\n')
    {
        line[i] = c;
        ++i;
    }
    line[i] = '\0';
    printf("processing line: %s", line);

    return i;
}

void copy(void)
{
    int i;
    extern char line[], longest[];

    i = 0;
    while ((longest[i] = line[i]) != '\0')
        ++i;
    printf("\tcopied new longest: %s", longest);
}	
```

## GOTO
DO NOT USE, this can usually be replaced with other more readable code

Allows you to go to other lines in your program directly

The one situation where it might be helpful is if you have multiple nested loops, and you want to exit out of all the loops. Goto can do this with one statement, instead of using many breaks as breaks only exit out of the innermost loop.

For this reason we will skip an example on goto.


## Input & Output
### scanf
Don't worry about what the syntax means for now
```c
#include <stdio.h>

// K&R pg. 158
// rudimentary calculator

int main()
{
    double sum, v;
    sum = 0;
    while (scanf("%lf", &v) == 1)
        printf("\t%.2f\n", sum+=v);

    return 0;
}
```

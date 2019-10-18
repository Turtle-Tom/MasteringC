# Notes on the C Language
---
## Data Types

### Primary Data Types:
integer - `int`\
floating point - `float`\
character - `char`\
`void`\
enumeration - `enum`

### Derived Data Types:
`array`\
`structure`\
`union`\
`pointer`

### Integer Type
`int` (signed int) - 16 bits\
`unsigned int`\
`short int` or `short`\
`unsigned short int` or `unsigned short`\
`long int` or `long` - 32 bits\
`unsigned long int` or `unsigned long`\
`long long`\
`unsigned long long`

### Floating Point Type
`float`\
`double`\
`long double`

### Character Type
`char` (signed char)\
`unsigned char`

> Note that there are 8 bits in a byte.

## Symbolic Consttants 

It is typically bad practice to have constants within random places of your code. Instead, 
give them names as follows:\
#define <NAME> <value_to_replace_with>\
`#define FREEZING 32`\
Note that there is no semicolon after the definition of symbolic constants. The name given 
to them will be replaced with the text following it.

## Character Input and Output

Text input and output, regardless of origin or destination, is treated as a stream of characters.  

> A _text stream_ is a sequence of characters divided into lines with each line consisting of zero 
> or more characters followed by a newline character. (Ritchie & Kernigham, 1988)

The simpliest functions for character I/O provided by the standard library are `getchar()` and `putchar()`.\
Each time it is called, `getchar()` read the next input character from a text stream and returns it. The function 
`putchar()` will print the character passed to it. When assigning `getchar()` to a variable, the variable must be 
of size int or of a data type that is greater than or equal to the same number of bytes as int. This is because 
`getchar()` can return `EOF` which is an symbolic constant integer defined in stdio.h that means "End of File".

## Functions

> Note: The rules for declaring and using functions mentioned here are in realation to ANSI C.

Functions offer encapsulation to C programs. A function definition has the following form:\
`[return_type] [function_name]([parameter_declarations]) {`\
&nbsp;&nbsp;&nbsp;`declarations`\
&nbsp;&nbsp;&nbsp;`statements`\
`}`

Older versions of C require that functions that have no parameters be declared with `void` in the parameter list instead
of leaving it empty. Keep this in mind if compatibility with older C programs is necessary.

**Function prototypes** are definitions of the functions used within a file. They are placed at the beginnning, typically 
between the lines declaring the included libraries and the main function or first functions defined. They are helpful for 
allowing the compiler to check for errors.\
A **parameter** is a variable in the function definition, and an **argument** is a variable passed during a function call. 
All function arguements are _pass-by-value_ meaning that functions are given the values of the arguements in temporary varaibles. 
This makes it so a called function cannot directly alter the variable. However, _pointers_ allow the actual variable to be altered 
by passing the value of its address. This is how arrays must be passed in C.

## Variables and Scope

Variales that are local to a function exist only when the function is called and disappear when it returns. These local variables are 
called _automatic_ vaiables.\
The values of _external_ variables are permanent, and can be used within functions. To use an external variable within a function it
must be declared using the `extern` keyword.

`int max; // External variable`\
\
`void foo() {`\
&nbsp;&nbsp;&nbsp;`extern int max`\
&nbsp;&nbsp;&nbsp;`max = 10;`
&nbsp;&nbsp;&nbsp;`...`\
`}`

The keyword `extern` makes it known that the variable being declared refers to the external variable and not a local variable.
However, the use of `extern` is unnecessary if the external variables are defined in the file before the function. To avoid
the use of extern altogether, define external variables all at the top of the source file. It is common to put external 
variables in header files, in which case `extern` is needed, and it is always needed for access across files.

> Note: "definition" refers to the place where variables are created, and "declaration" refers to the place where the nature
> of the variable is stated, but no memory is allocated.

### Declarations

All variables must be declared before use. Declarations specify a type followed by a list of one or more variables that all share
the declared type. Declaring variables of the same type on different lines is useful for adding inline comments. Variables may also
be initialized within their declarations. External and static variabls are initialized to 0 by default. Automatic variables with no
explicit initialization will take on random/garbage values that can cause memory access errors. \
The qualifier `const` can be added to the beginning of a declaration to signify that its value(s) cannot be changed. `const` can also
be added to array arguments for functions to show the function will not alter the array. E.g. \

`int strlen(constchar[]);`

## Escape Sequences and Bit Patterns

### Bit Patterns

Since characters are just integers corresponding to ASCII values, they can be represented with octals and hexadecimals. 
Specific byte-sized bit patterns can be used to define different ASCII values as constants. \ 
E.g. \ 
`#define SPACE '\040'` (octal) \ 
`#define PLUS '\x2B'` (hex) \ 

Octal digits take the form of '\ooo' where _ooo_ is one to three octal digits (0...7). Hexadecimal digits take the form of
'\xhh' where _hh_ is one or more hexadecimal digits (0...9, a...f, A...F).

### Escape Sequences

Escape sequences are simply special types of character constants. \
- \\a alert (bell) character
- \\b backspace
- \\f formfeed
- \\n newline
- \\r carriage return
- \\t horizontal tab
- \\v vertical tab
- \\ backslash
- \\? question mark
- \\' single quote
- \\" double quote
- \\ooo octal number
- \\xhh hexadecimal number
- \\0 null character (value of zero)

> Note: String constants are actaully arrays of characters ending in '\0' to signify the end of the string. Therefore,
> strings take up the amount of bytes equal to their length + 1. Strings have no limit on size, and the standard library 
> function strlen() does not include the appended null character. Because of this, 'x' != "x".

## Enumerations

An enumeration is a list of constant integer values. Default value assignment starts at 0 and increases as more constants are defined.
If desired, enums may be given constant values, acting as an alternative to `#define` statement. \

`enum boolean { NO, YES };` \
`enum months = { JAN = 1, FEB, MAR, APR, MAY, JUN, JUL, AUG, SEPT, OCT, NOV, DEC }; // FEB = 2, MAR = 3, ...` \

Names in enumerations must be distinct, but their values need not be.

## Arithmetic Operators

`+` Addition \
`-` Subtraction \
`*` Multiplication \
`/` Division \
`%` Modulus \

The % operator produces the remainder of x in x % y. The % operator cannot be applied to `float` or `double`. \
The direction of truncation when using / and the sign of the result when using % are machine-dependent for negative operands, 
as is the action taken on overflow or underflow. \
Normal mathematical operator precedence applies. \

## Relational and Logical Operators

`<` Less than \
`>` Greater than \
`<=` Less than or equal to \
`>=` Greater than or equal to \
`==` Equal to \
`!=` Not equal to \
`&&` Logical AND \
`||` Logical OR \
`!` Logical NOT \

Precedence of operators is from highest to lowest as follows: \
&nbsp;&nbsp;&nbsp;(<, <=, >, >=), (==, !=), &&, || \

## Useful Libraries

`<limits.h>`/`<float.h>` - Contain symbolic constants for the sizes of different types, as well as various machine properties. \
`<string.h>` - Contains many functions for working with strings such as strlen(). \


## Miscellaneous

An isolated semicolon is called the _null statement_.

---

## References

Ritchie, M. D, & Kernigham, W. B. (1988). The C Programming Language (2nd Ed.). Englewood Cliffs, New Jersey: Prentice Hall.

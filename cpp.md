# C++ Refernence

## Primitive Built-In Types
Most important ones include

```cpp
bool		boolean			NA	
char		character		8 bits
short 		short integer		16 bits
int		integer			16 bits
long 		long integer		32 bits
float		single-precision 	floating-point
double		double-precision 	floating-point
```

Additionally, we can use *unsigned* (represents only values greater than or equal to zero) or *signed* types. Int, short, long and long long use *signed* as default.


#### Tips for deciding which type to use
* Use **unsigned** type when you are certain values can't go below 0
* Use **int**. Short usually to small and long often has the same size as int.
* Use **double**. Float often isn't precise enough and the cost of double vs float computation is negligible.

### Character and Character String Literals
`'a'` is a character literal

whereas `"Hello, World!"` is a string literal (array of constant chars, as in C, string literals are represented by the string + `\0` literal)

Good to know: `true` and `false` are literals of type bool.

#### Escape Sequences
```cpp
\n		// newline
\t		// tab
\v		// vertical tab
\a		// alert
\b		// backspace
\"		// double quote
\'		// single quote
\\		// backslash
...		
```

### Type Conversions
```cpp
bool b = 46;			// true, anything != 0 is true
int i = b;			// i has value 1
i = 3.14;			// i has value 3: it is truncated
double pi = i;			// pi has value 3.0
unsigned char c1 = -1;		// Assuming 8-bit chars, char has value 255
signed char c2 = 256;		// Value of c2 is undefined. AVOID! Compiler doesn't catch
```

#### Leftovers from C
Remember: there's no bool type in c, only int

```cpp
int i = 42;
if(i)			// Will only evaluate to true if i != 0
	i = 0;
```

**Caution: Don't mix signed and unsigend types:**
Signed values are automaically converted to unsigned. "*For example, in an expression like a * b, if a is -1 and b is 1, then if both a and b are ints, the value is, as expected -1. However, if a is int and b is an unsigned, then the value of this expression depends on how many bits an int has on the particular machine. On our machine, this expression yields 4294967295.*"

## IO
`<iostream>` library: *istream* for *input stream* and *ostream* for *output stream*

* `cin`	: standard input
* `cout`	: standard output
* `cerr`	: standard error
* `clog`	: standard log
* `endl`	: clears buffer and starts new line

Istream becomes invalid if we hit an *end-of-file* or encounter an invalid input type, say string instead of int. 
*End of file* is system dependent: Ctrl-D on Unix or Ctrl-Z on Windows

### Examples
```cpp
std::cout << "This is an output << std::endl;

// continous reading of stream
int val;
while (std::cin >> val) 
	sum += value;

```

## Comments
* `//`: single line comments
* `/**/` : multi line comments, use asterix between comment pairs to visualize inner lines. *Warning*: Comment pairs do not nest, meaning comments in comments cause compiler errors

## Operators
```cpp
int a = 0, b = 0;

++a;	--a;
a++;	a--;

a /= b; 	a *= b;
bool m = a < b;		bool n = a > b;
```

## Flow of control
Normally: sequential execution. Modify with:

### while loop
```cpp
while(val <= 10) {
	sum += 10;
}
```

### for loop
```cpp
for (int i = 0; i < 10; i++) {
	std::cout << i << std::endl;
	// More code
}

// Shorter
for (int i = 0; i < 10; i++) 
	val += 10;
	
// The shortest
for (int i = 0; i < 10; i++) val += 10;
```

### if statements
```cpp
int x = 0;
// Some program

if (x < 0) {
	x *= -1;
}

if (x < 0) 
	x *= -1;

if (x < 0) x *= -1;
```

## Classes
Classes define custom data structures in C++. A class defines a type along with a collection of operations that are related to that type. These types can be used like built-in types when implemented correctly and smartly. 

To use them, we need to know the name, where it is defined (typically the header) and what operations it supports.

## Sources
Mostly based on "C++ Primer 5th Ed." by Stanley Lippman a.o."


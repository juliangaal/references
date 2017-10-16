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


> **Caution: Don't mix signed and unsigend types:**
Signed values are automaically converted to unsigned and unsigned values can have unintended consequences, e.g.

```cpp
// This is an infinite loop! after 0, unsigned int becomes 45454...
for (unsigned int = 0; i >= 0; --i)
	std::cout << i << std::endl;
```

### Variables, Scope and Initialization
>“Initialization is not assignment. Initialization happens when a variable is given a value when it is created. Assignment obliterates an object’s current value and replaces that value with a new one.” 

For example, we can initialize the variable u in 4 different ways

```cpp
int u = 0;
int u = {0}; 		// list initialization!
int u{0};			// list initialization!
int u(0);
``` 
Curly brackets and list initialization becomes important when used with built-in types: The compiler won't let us initialize if/because it could lead to loss of information

```cpp
double pi = 3.1434345682...	
int a{pi}, b = {pi};			// error: narrowing conversion
int c(pi), b = pi;				// ok, but value will be truncated
```

#### Default Initialization
When we define a built-in type variable without an ititializer, it is assigned a *default initializer*, depending on the type and location of initialization: outside funtion body, they are inititialized to zero, inside a function body, they stay *uninitialized*. Objects of class type that we don't explicitely initialize have a value that is defined by the class.

>**Tip
“We recommend initializing every object of built-in type. It is not always necessary, but it is easier and safer to provide an initializer until you can be certain it is safe to omit the initializer.” This will avoid compiling headaches!**

#### Scope
Scope is mostly determined by curly brackets

A variable with *global scope* is accessible to the entire file. e.g. main function. 
A variable with *block scope* is accessbile to the block (function, {}, for-/while-loop) in question. 

>**Advice: 
>Define variables where you first use them: 1.ly Better readability and 2.ly initial value will (probably) be more adequate.** 

``` cpp

double x = 5; 	// accessible in entire file: avoid!

int main() {
	int i = 1;	// not accessible outside main!
	
	{
		int x = 0;		// x and b only accessible between these two brackets
		int b = (x + 6);
	}
	
	return 0;
}
```

#### Naming Conventions
Variables should 

* convey meaning to make code more readable
* normally be lowercase
* be visually distinct, e.g. student_loan or studentLoan instead of studentloan
* Class names usually are uppercase

***Use consistenly!***

### Character and Character String Literals
`'a'` is a character literal
`"Hello, World!"` is a string literal (array of constant chars, as in C, string literals are represented by the string + `\0` literal)

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

## Compound Types: References and Pointers

### &Reference
A **Reference** defines an alternate name for an object. A reference type "refers to" another type by using the memory address of the referenced object in question.

```cpp
int ival = 1024;
int &refVal = ival;		// refVal refers to another ival
int &refVal2 = 2;		// error: initializer must be object
double pi = 
int &refVal2 = pi;		// error: wrong type
```
Instead of copying the initializers value to refVal, we *bind* the reference to it's initializer. Additionally, 
>“There is *no* way to rebind a reference to refer to a different object. Because there is no way to rebind a reference, references **must** be initialized.”

### *Pointer
A **Pointer** is a compound type that "points to" another type. Unlike references, a pointer is an object in it's own right. This means
 
 * no need to initialize
 * pointer can be assigned and copied
 * can point to *several different objects* over the span of it's lifetime

A pointer holds the address of another object, which enables us to get the *content* of this pointer by dereferencing it with `*`.

```cpp
int ival = 42;
int *pval = &ival;		
int *rval = val;

std::cout << pval << std::endl;	// will print address 0x...
std::cout << *pval << std::endl;	// will print content (!) of address: 42
std::cout << &ival << std::endl;	// standard reference, will print address 0x...
```
#### Nullpointers
... don't point to any objects

```cpp
int *p2 = nullptr;
int *p1 = 0;			// equivalent
```

>**Advice: Initialize all pointers
> Unitialized pointers are a common source of runtime errors and are a nightmare to debug. Define pointers after object to which it should point is definedd. For all other pointers, initialize them to nullptr**

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
* `/**/` : multi line comments, use asterix between comment pairs to visualize inner lines. 

>*Warning*: Comment pairs do not nest, meaning comments in comments cause compiler errors

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
int x = 1;
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

## Separate Compilation
... allows our program to be split into multiple logical parts. To support it, C++ distinguishes between declarations and definitions. </br> A *declaration* makes a variable known to the program and defines type and name. </br>A *definition* creates the associated entity by declaring it and allocating space and/or an initial value.

```cpp
extern int i;						// declares but does not define
int j = 0;						// declares and defines
extern double pi = 3.1416;		// An extern that has an initializer is 									// a definition -> overrides extern 
```

>**Note**: **Variables must be defined exactly once but can be declared many times.** 

## Key Concepts
**Statically typed**
C++ is a statically typed lanuage, meaning that types are checked at compile time.
## Sources
Mostly based on "C++ Primer 5th Ed." by Stanley Lippman a.o."


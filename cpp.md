# C++ Reference

## TOC
1. [Built-In Types](#built_in)
2. [Abstract Data Types: Strings, Vectors, Arrays](#sva) </br>
    a. [Strings](#strings) </br>
    b. [Vectors](#vecs) </br>
    c. [Arrays](#arrays) </br>
3. [Flow of Control](#floc)
4. [Expressions](#exps) </br>
    a. [Operators](#ops) </br>
5. [Compound Types](#comps)
6. [Classes](#classes)
7. [IO](#io)

</br>
<a name="built_in"></a>

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


> **Caution: Don't mix signed and unsigned types:**
Signed values are automaically converted to unsigned and unsigned values can have unintended consequences, e.g.

```cpp
// This is an infinite loop! after 0, unsigned int becomes 45454...
for (unsigned int = 0; i >= 0; --i)
	std::cout << i << std::endl;
```

</br>

### Variables, Scope and Initialization

>“Initialization is not assignment. Initialization happens when a variable is given a value when it is created. Assignment obliterates an object’s current value and replaces that value with a new one.” 

For example, we can initialize the variable u in 4 different ways

```cpp
int u = 0;
int u = {0}; 		// list initialization!
int u{0};		// list initialization!
int u(0);
``` 
Curly brackets and list initialization becomes important when used with built-in types: The compiler won't let us initialize if/because it could lead to loss of information

```cpp
double pi = 3.1434345682...	
int a{pi}, b = {pi};			// error: narrowing conversion
int c(pi), b = pi;			// ok, but value will be truncated
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

#### Qualifiers

##### Const
 prohibits any changes to variable after intialization. By default a `const` variable is local to the file it is initialized in.

```cpp
const int buf_size = 512;
buf_size = 512;             // error: attempt to write to const
const int cus_size;         // error: const requires initialization
```
If you still want to use the same `const` variable across multiple files, without creating a new instance in every file, we can use keyword `extern` in both declaration and initialization,

```cpp
//file1.cpp
extern const int buf_size = 512;

//file1.h
extern const int buf_size;
```
where `extern` signals to the compiler, that the variable is initialized in another file.

> Remember: we can bind a reference to an object of a const type, but we can't change the object to which the reference is bound. For more info, 2.4.1/2 p. 217 in "C++ Primer"

**Constexpr and Constant Expressions** 
A *constant expressions* is an expressions whose value cannot be changed and that can evaluated at compile time. A `const` object that is initialized from a *constant expression* is also a constant expression.

```cpp
const int val = 5;              // constant expression
int vals = 6;                   // not a constant expression
const int valss = get_val()     // not a constant expression! Value not clear at compile time
``` 
In C++1x, we can use the keyword `constexpr` explicitely, to tell the compiler that the variable is implicitly const

```cpp
constexpr int val = 5;          // ok
constexpr int vals = val + 1;   // ok
constexpr int valss = get_val() // ok, if get_val() is a constexpr function
```
>Remember: “variables defined inside a function ordinarily are not stored at a fixed address. Hence, we cannot use a constexpr pointer to point to such variables.”

With pointers, `constexpr` behaves a little different

```cpp
const int *p = nullptr;         // p is a pointer to a const int
constexpr int *q = nullptr;     // q is a const pointer to int 
```
“ The difference is a consequence of the fact that constexpr imposes a top-level const [...] on the objects it defines.” More examples

```cpp
constexpr int i = 42;           // defines const int i
constexpr const int *p = &i;    // defines constant pointer to const int
```

#### Naming Conventions
Variables should 

* convey meaning to make code more readable
* normally be lowercase
* be visually distinct, e.g. student_loan or studentLoan instead of studentloan
* Class names usually are uppercase

***Use consistenly!***

</br>

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

</br>

### Types

#### Defining types

A `type alias` is a name synonymous with another type, e.g.

```cpp
typdef double wages;        // defines wages to be synonymous to double
wages mark = 5000;
```
Since C++1x you can use an `alias declaration`, e.g.

```cpp
using SI = Sales_item;      // SI is synonymous for Sales_item
```
or the `auto` type specifier, which "auto"-matically deduces the type from the initializer, e.g.

```cpp
int i = 5, j = 6;
auto k = i + j;     // auto will produce type int
```
#### Types Conversions

```cpp
bool b = 46;			// true, anything != 0 is true
int i = b;			// i has value 1
i = 3.14;			// i has value 3: it is truncated
double pi = i;			// pi has value 3.0
unsigned char c1 = -1;		// Assuming 8-bit chars, char has value 255
signed char c2 = 256;		// Value of c2 is undefined. AVOID! Compiler doesn't catch
```
It's a little different for pointers! There's no automatic type conversion there. The pointer must be of the same type as the one it points to.

```cpp
int i = 42;
double *dp = &i;		// Error!
```

#### Leftovers from C

Remember: there's no bool type in c, only int

```cpp
int i = 42;
if(i)			// Will only evaluate to true if i != 0
    i = 0;
```

</br>
<a name="sva"></a>

## Abstract Data Types: Strings, Vectors, Arrays

### Namespace *using* Declarations

Using a *using declarations* allows us to drop the prefix of the library we're using. It has the form `using namespace::name` e.g.

```cpp
std::string one = "one";

using namespace std::string;
string one = "one";
```
defines namespace and let's us access `name` directly.
>**Warning**: Don't include namespaces in header files. Because these are imported, it may lead to naming conflicts accross libraries and are a headache to debug.

Avoid too "general" namespaces like `using namespace std`, as it is very well possible that a library you imported may conflict with the resulting direct acces to `name`. If expressivness is important to you, explicitly stating the library namespace is a good practice however.

<a name="string"></a>

### String type

A string is a variable-length sequence of characters and is part of the standard library. It must be included in the header with `#include <string>`.

```cpp
#include <string>
using std::string;

string s1;                         // initialized to empty string
string s2 ("This is a string");    // direct init, not including the null
string s3 = "This is a string";    // copy init
string s4 (n, 'c');                // n number of 'c'-literals
```
The only difference between *direct initialization* and *copy initialization* is that we **must** use direct initialization when we have > 1 initializers (see `s4`).

#### Common string operations

Usage   | Description
------- | ----------- 
`os << s`|  Writes s onto output stream os, returns os
`is >> s`|  Reads whitespace seperated from is into s, return is
`getline(is, s)`| Reads a line of input from is into s, return is
`s.empty()`| Return true, if empty string
`s.size()`| Returns number of charachters in string ('\0', too??), `string::size_type`
`s[n]`| "Subscript": Returns reference to char of s at index n. *Caution: n unchecked - must be >= 0 && < size()*.
`s1 + s2` or `s1.append(s2)`| String concatenation
`s1 = s2`| Replaces contents of s1 with contents of s2
`s1 ==/(!=) s2` or `s1.equals(s2)`| Returns true if equal
`< <= >= >`| Comparisons 

For individual character treatment, look at `cctype`-header. (p. 293, C++ Primer)
Also, you should be using the *range for* statements introduced in c++1x. See [loops](#loops)

>**Advice for using c libraries in c++**
>C headers have the form *name.h*. The C++ versions remove the *.h* and prepend a `c`, to indicate it is part of the C library ("cname"). Use the version optimized for C++ if possible: you *could* use `ctype.h`, but you *should* `cctype`, as the names defined in `cctype` are defined in the std namespace.

#### Reading and Writing

```cpp
std::cin >> s;                  // Read whitespace seperated (!) string
std::cout << s << std::endl;    // e.g. in: Hello, World! out: Hello,

std::cin >> s1 >> s2;           // in: Hello, World! out: Hello,World!
std::cout << s1 << s2 << std::endl;

while (std::cin << s)           // Unkown number of s strings
    std::cout << s << std::endl;
```
Sometimes we don't want to ignore the whitespaces, for that we can use `getline`.

```cpp
while (getline(std::cin, s))
    std::cout << s << std::endl;
```
>Note: `newline` is ignored

</br>
<a name="vecs"></a>

### Vector Type

A vector holds a variable-length sequence of objects of a given type. Sometimes vectors are referred to as being a *container*, because it *contains* (duh) other types. To us it, we must include the header `#include <vector>`.

```cpp
#include <vector>
using std::vector;

// vector<TYPE> NAME;
vector <T> v1;              // empty vector
vector <T> v2 (v1);         // v2 is exact copy of v1
vector <T> v2 = v1;         // equivalent
vector <T> v3 (n, val);     // n elements with value val
vector <T> v4 (n);          // vector with n empty elements
vector <int> v5 {1, 2, 4};  // direct list initialization
vector <int> v6 = {1, 2, 3};// copy list initialization, equivalent 
vector <int> v7 = {};       // empty list initialization

```
Again, the only difference between *direct initialization* and *copy initialization* is that we **must** use direct initialization when we have > 1 initializers. Additionally, we **can't** to use regular brackets `()` with list initialization.

</br>

#### Vector operations

Adding elements is a little special case:
```cpp
vector<int> vec;
for (int i = 0; i < 100; i++)
    vec.push_back(i);
```
Intuitively (if you're used to C or Java), setting the size of `vec` to 100 may make sense, but **vectors grow efficiently**. That means it's usually more efficient to initialize the vector empty. The only exception to this is if all values are the same, e.g. `vector<int> vec (10, 1);`.

More operations include 

Operator|Description
---|---
`v.empty()`| True if empty
`v.size()`| Returns number of elements `size_type`
`v.push_back(val)`| "Appends" an element
`v[n]`| Returns a reference to element on spot n in v
`v1 = v2`| Replaces v1 with v2
`v1 ==(/!=) v2`| True if equal
`< <= >= >`| Comparison Operators

See vectors in [loops](#loops)

<br>

#### Getting vector values

As usual, you can get them using the `subscript []`-operator. This is unchecked and can cause buffer overflows, as it's not checked by most compilers, however. If possible use the `range for` functionality in loops or make use of `v.size()`!

A more general way is using **iterators**. They give us direct access to objects in a container, e.g. vectors or strings. Unlike pointers, iterators have type members: `begin` and `end`. “In general, we do not know (or care about) the precise type that an iterator has”.

```cpp
vector<int> vec = {1, 2, 3};
auto b = vec.begin(), e = vec.end();
```
The type returned by the `begin`/`end`-operator depends on the whether they operate on is `const` or not.

```cpp
vector<int> v;
const vector<int> cv;
auto it1 = v.begin();       // type vector<int>::iterator
auto it2 = cv.begin();      // type vector<int>::const_iterator
```
It is good practice to use type `vector<int>::const_iterator` when we want to read, but *not* write. C++1x introduced `cbegin` and `cend` for this:

```cpp
auto it1 = v.cbegin();      // both return type vector<int>::const_iterator!
auto it2 = v.cend();
```

</br>
<a name="iterop"></a>

**Operations**


Operation|Description
---|---
`*iter`|Returns a reference to the element denoted by iter
`iter->mem` or `(*iter).mem`| Dereferences and exposed member function `mem`
`++iter` or `--iter`| Move to next/previous adress
`iter1 ==(!=) iter2`| True, if they point to same memory address

and furthermore, these *iterator arithmetic* are supported

Operation|Description
---|---
`iter +/- n` or `iter +=/-= n`| Returns iterator n-steps ahead/back in container. Must be < that `end()`
`iter1 - iter2`| Returns distance between the two in `distance_type`
`< <= >= >`| iter1 is smaller than iter2 if iter1 appears first in container

Example usage

```cpp
auto mid = v.begin() + v.size() / 2;
// in some loop: process only first half
if (it < mid)
    do_something();
```

You can use iterator pointers like this

```cpp
if (v.begin() != v.end()) {
    auto it = v.begin();
    *it = some_value;    
}
```
>**Note: if a vector is declared, but not initialized begin() and end() iterators are the same**

</br>
<a name="arrays"></a>

### Arrays

Like vectors, arrays are containers that contain n number of elements of a specific time, **their size is fixed** however. An array declarator consists of a name "name" and number of elements "n": `name[n]`.

```cpp
unsigned size1 = 42;
constexpr size2 = 42;

int array1[10];
int array2[size1];          // Error: size1 is not constexpr
int array3[size2];
int array4[get_size()];     // ok, if get_size() constexpr
```
to initialize

```cpp
int array1[3] = {0, 1, 2};
int array2[] = {0, 1, 2};   // equivalent
int array3[5] = {0, 1, 2};  // {0, 1, 2, 0, 0}, fills up with empty type

// Multidim
int arr4[4][4] = {
    {1, 2, 3, 4},
    {1, 2, 3, 4},
    {1, 2, 3, 4},
    {1, 2, 3, 4}
};

// avoid this way less clear way
// Note: “To use a multidimensional array in a range for, the loop control variable for all but the innermost array must be references.”
int arr5[4][4] = {1, 2, 3, 4, 1, 2, 3, 4, 1, 2, 3, 4, 1, 2, 3, 4};
```

>**Note: no copy or assignment as e.g. in vectors, we can't assign one array to another!**

Character arrays end all "strings" with the `\0`, as in C.

>**“Although C++ supports C-style strings, they should not be used by C++ programs. C-style strings are a surprisingly rich source of bugs and are the root cause of many security problems. They’re also harder to use!” Also, for most C++ programs, it is more efficient to use the C++ provided `std::string`**


</br>

#### Arrays: References and Pointers

TODO

</br>

#### Accessing Array Elements

We can use `range for` operators or use the `subscript` operators. The index starts at 0. When choosing to use the latter, the variable should be of the type `size_t`.

>**`size_t` is a machine specific unsigned type that is guaranteed to be big enough to hold the size of any object in memory**

see [loops](#loops) to see the `range for` approach.

</br>

#### Arrays and pointers

Suppose we're looking at this

```cpp
string arr[5] = {"hi", "there", "hi"};
string *first = &arr[0];
```
the `first` pointer points to the first element in `arr`. This is symonymous to 

```cpp
string *first = arr;
```
because "the compiler automatically substitutes a pointer to first element". You can use this to iterate through arrays, as usual with pointers:

```cpp
“int last = *(arr + 4)”     // equivalent to arr[4]
```

It is recommended to use the `begin` and `end`-operators, as they are safer to use than manually getting the element behind the array, e.g. `int *e = &arr[10]` with length 10! Since C++1x, you can use

```cpp
int arr[] = {1, 2, 3, 4, 5};
int *b = begin(arr); 
int *e = end(arr);
```
This enables this kind of usage:

```cpp
while (b != e && *b >= 0)
    ++b; 
```

</br>

#### Array arithmetic

You can use the arithmetic mentioned [here](#iterop), however, subtracting two pointers from each other returns a signed type called `ptrdiff`.

</br>

#### Use vector if you can

Pointers and arrays are very error prone. For more read: 3.5.5 Interfacig with older code - C++ Primer

To convert, you could use something like this:
```cpp
int arr[] = {1, 2, 3};
vector<int> vec (begin(arr), end(arr));
```

</br>
<a name="floc"></a>

## Flow of control 

Normally: sequential execution. Modify with:

### while loops

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
<a name="loops"></a>
Since C++1x, you can use *range for* statements, e.g.

```cpp
std::string s = "Hello, World!";
for (auto c: s)     // For every character c in s, do..
    std::cout << c << std::endl;
```
We have to use references, if we want to change the value of the character, though! 
 
```cpp
std::string s = "Hello, World!";
for (auto &c: s)
    c = change_to_something();
```
Same goes for vectors
<a name="iterloop"></a>

```cpp
vector<int> vec = {1, 2, 3, 4, 5}
for (auto &v : vec)
    v *= v;

// No value change, no reference
for (auto v : vec)
    cout << v << endl;
    
// With iterator
for (auto it = v.begin(); it != v.end() && !isspace(*it); ++it) {
    *it = do_something_here;
}
```

And arrays

```cpp
int arr[10] = {0, 1, 2};
for (range val : arr)
    cout << val <<;

int *e = &arr[10];              // element just past array length!    
for (int *b = arr; b != e; ++b)
    cout << *b << endl; 
    
// Safer way: begin and end
int *b = begin(arr); 
int *e = end(arr);
while (b != e && *b >= 0)
    ++b; 
```
Using `subscript` is definitely unsafer for loops!

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

</br>
<a name="exps"></a>

## Expressions

An **expression** is formed from one or more *operands* and yields a result 

**Unary Operators**
... like `&` or `*` (dereference) act on one operand.

**Binary Operators**
... like `==` and `*` act on two operands.

**Compound Expressions**
... are expressions with 2 or more operators. To evaluate those, operators have to be grouped to operands according to *precedence* and *associativity*.

* “Because of *precedence*, the expression 3+4*5 is 23, not 35.”, it groups the operands. "Punkt vor Strich"
* “Because of *associativity*, the expression 20-15-3 is 2, not 8.”

In C++, e.g. `5 + 3 * 1 / 2 + 2` is internally represented by `((5 + ((3*1)/2)) + 2)`, where the parenthesis override precedence and associativity. We can manually override these properties with parenthesis, if we'd like. However, knowing the *order of evaluation* can help us reduce the use of parenthesis. For example, in `g() + f() * h() - x()`, *precedence* guarantees that `f() * g()` are grouped together, and *associativity* guarantess that `g()` is added to the prodct of `f() * h()` and that in turn subtracted from `h()`.

>**Advice for Compound Expressions: 
> When in doubt, manually set parenthesis
> if you change the value of the operand, try not to use it in the same expression**

TODO: “There are no guarantees as to the order in which these functions are called.” Y? 

</br>
<a name="ops"></a>

### Arithmetic Operators 

#### General

```cpp
int a = 0, b = 0;

++a;	--a;
a++;	a--;

int c = a % b;

a /= b; 	a *= b;
bool m = a < b;		bool n = a > b;

// Try to avoid     // better: logical NOT operator
bool t = -m;        bool s = !m;
```

>**Warning: Beware of overflow! e.g. on a 16 bit system, the max `short` value is 32767. Adding 1 to that has unpredictable results and often times is not caught by compiler. Also some limitations arise due to math, e.g. the division with 0**

</br>

##### Operators by Precedence

Op|Example
---|---
`+`/`-` **unary**|++i;
`*`|i * 2;
`/`|i / 2;
`%`|i % 2;
`+`/`-` **binary**|i + j;

>**Tip: Use postfix `i++` only when necessary! It avoids unnecessary work, the postfix must store the value before returning it + 1. Also, for ints and pointers, the compiler can do some optimization.

</br>

#### Logical `AND` and `OR`
Are represented by `&&` and `||`. Expressions containing one of those are evaluated from left to right and with **short-circuit evaluation**: the right operand is only evaluated, if and only if the left operator does not entail the result. e.g.

Expression|Type
---|---
`F && T` or `F && f`| short-circuit: only `F` must be evaluated for program to know the result of the entire expression
`T && F` or `T && F`| **not** short-circuit
`T || F`|short-circuit
`F || T`|**not** short circuit

##### Operators by precedence

Ops|Desc
---|---
`!`|!expr
`<`/`<=`|less than/less or equal than
`>`/`>=`|greater than, greater of equal than
`==`/`!=`|(in)equality
`&&`|logical `AND`
`||`|logical `OR`

</br>

#### Equality Tests
`i` evalues to `true`, if it's not 0, and `false` if it's zero:

```cpp
if (i)
    do_something();
```
We can also ask for equality with the `==`-operator, which returns a `bool`

```cpp
if (i == true)
    do_something();
```
The problem here is, that when `i` is not a `bool`, unexpected things happen: `bool` is converted to the type of val, e.g. `i` being `int`, before the `==` operator. In this case:

```cpp
if (i == 1)
    do_something();
```
<a name="todo"></a>
“If we really cared whether val was the specific value 1, we should write the condition to test that case directly.” TODO HOW 

>**Warning: these literals should only be used to compare objects**

</br>

### Conditional Operator
the `?` allows us to house simple if-else conditions in one line. e.g.

```cpp
string final_grade = (grade < 5.0) ? "pass" : "fail";
```
This will evalue to "pass" if the `grade` is smaller than 5.0, and "fail" otherwise. We can nest expressions as well, like this example from C++ Primer:

```cpp
string final_grade = (grade > 90) ? "high pass"
                          : (grade < 60) ? "fail" : "pass";
```
>These nested conditionals become quite unreadable. Stick to a max of 2-3 layers.

</br>

### Bitwise Operators

TODO

</br>
<a name="comps"></a>

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
 * can point to *several different objects* over the a of it's lifetime

A pointer holds the address of another object, which enables us to get the *content* of this pointer by dereferencing it with `*`.

```cpp
int ival = 42;
int *pval = &ival;		
int *rval = val;

std::cout << pval << std::endl;		// will print address 0x...
std::cout << *pval << std::endl;	// will print content (!) of address: 42
std::cout << &ival << std::endl;	// standard reference, will print address 0x...

int *fval;
int x = (*fval).some_member_function(); // equivalent with below
int y = fval->some_member_function();
int *uval = &ival;
fval = uval;				// fval, uval refer to same address
std::cout << *fval << std::endl;	// 42
```
We can also compare two valid pointers of the same type: `==` and `!=` to type *bool*. They are equal if they hold the same address. Two objects hold the same address if 

* they are both null
* they address the same object
* they are both pointers one past the same object

Incrementing pointers show an important difference:

```cpp
auto beg = v.begin();
while(beg != v.end() && *beg > 0)
    cout << *beg++ << endl;
```
because the precedence of `++` is higher than the dereferencing operator `*`, this is equivalent to `*(beg++)`.

</br>

#### Nullpointers

... don't point to any objects

```cpp
int *p2 = nullptr;
int *p1 = 0;			// equivalent

std::cout << *p2 << std::endl;	// error: no content
std::cout << p2 << std::endl;	// addr: 0x0

if (p2) {}			// evaluates to false! value is 0
if (!p2){}			// Would be used to check for nullptr
```

>**Advice: Initialize all pointers
> Unitialized pointers are a common source of runtime errors and are a nightmare to debug. There's no way for the compiler to distinguish a valid memory address from an invalid one. 
> Define pointers after object to which it should point is defined. For all other pointers, initialize them to nullptr**

</br>

#### Void* Pointers

Void* pointers can hold the address of any object, but the type is unknown.

What can we do with it?

* We can compare it to other pointers
* return from/ pass to function
* We **can't** use void to work on the object it points to

```cpp
double obj = 3.1416, *pd = &obj;
void *pv = &obj;
pv = pd;
*pv = 8.3434;			// Error, void pointer doesn't allow value change of underlying obj!
```
Generally used to work on memory as memory.

**Careful with compound type declarations**
`*` refers *not* (like the appearance would suggest) to the base type `int*`, but rather to `p1`! `p2` is not affected by the pointer. It is from type `int`.

```cpp
int* p1, p2; 			// p1 is a pointer to int, p2 is an int
```
To make this more clear (in my opinion), use this syntax

```cpp
int *p1, p2 = 5; 		// p1 is a pointer to int, p2 is and int
int *p2, *p3;			// p2 and p3 are pointers to int
```

Again, there's no "offical way to do it", so pick one way and use it consistently!

### Member access

`ptr->mem` is synonymous to `(*ptr).mem`. Because dereferencing has lower precedence than `*`, `*ptr.mem` would throw and error.

### Pointers to Pointers

TODO
### References to Pointers

TODO

<a name="io"></a> 

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

<a name="classes"></a>

## Defining Our Own Data Structures: Classes 

Classes define custom data structures in C++. A class defines a type along with a collection of operations that are related to that type. These types can be used like built-in types when implemented correctly and smartly. 

To use them, we need to know the name, where it is defined (typically the header) and what operations it supports.

### Creating header files

To ensure that our class definition is the same across multiple files, we tend to define classes in *header files*, usually abbreviated with `.h`, `.hpp`, `.H` or `.hxx`. 
The most common method to make it safe to use the same header file in multiple locations is the *preprocessor*, a program that runs before the compiler and changes the source text of your program. Then the pp sees `#include` it replaces the statement with the contents of the specified header file. 
C++ programs also use the preprocessor to define *header guards* that shield from reimportation/re-replacement of `include`-statements, e.g.

```cpp
#ifndef SALES_DATA_H
#define SALES_DATA_H
#include <string>
struct Sales_data {
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};
#endif
```
Let's say in main, by accident (or willful destructive urges), you `#include Sales_data.h` more than once, it is only imported/included once.

> Header Tips
> - use header guards
> - use unique name for header guards (usually class/file name)
> - use UPPER CASE for header guard name

## Separate Compilation

... allows our program to be split into multiple logical parts. To support it, C++ distinguishes between declarations and definitions. </br> A *declaration* makes a variable known to the program and defines type and name. </br>A *definition* creates the associated entity by declaring it and allocating space and/or an initial value.

```cpp
extern int i;				// declares but does not define
int j = 0;				// declares and defines
extern double pi = 3.1416;		// An extern that has an initializer is a definition -> overrides extern 
```

>**Note**: **Variables must be defined exactly once but can be declared many times.** 

## Comments

* `//`: single line comments
* `/**/` : multi line comments, use asterix between comment pairs to visualize inner lines. 

>*Warning*: Comment pairs do not nest, meaning comments in comments cause compiler errors

## Key Concepts

**Statically typed**
C++ is a statically typed lanuage, meaning that types are checked at compile time.

**Preprocessor** 
A program that runs before the compiler and changes the source text of your program.

**Vectors Grow Efficiently**
It is usually faster to declare a vector without an initializer, except when all elements in the vector have the same value. 

**Generic Programming**
“By routinely using iterators and !=, we don’t have to worry about the precise type of container we’re processing.” For example, in loop example [#3](#iterloop), you can see that we use `!=` rather than `<`. By using that and range/iterator based loops, we guarantee, that there can't be any buffer overflows.

Excerpt From: Lippman, Stanley B. “C++ Primer, 5/e.” iBooks.  

## Sources

Mostly based on "C++ Primer 5th Ed." by Stanley Lippman a.o."

## TODO

* `decltype`: p. 246 C++ Primer
* iterator vs iterator types: p. 342 C++ Primer
* Arrays: References and Pointers, 3.5.1 C++ Primer, Multidim Pointers 3.6
* "Lvalues and Rvalues"
* “If we really cared whether val was the specific value 1, we should write the condition to test that case directly.” [HERE](#todo)
* Why does prefix behave differently in loops?
* Bitwise Operators 4.8


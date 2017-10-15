# C++ Primer

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
```
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


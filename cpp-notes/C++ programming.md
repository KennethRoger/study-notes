# C/C++ PROGRAMMING


# POINTERS IN C

## Introduction to pointers in C##

### What is reference?

**Memory**: an array of bytes within RAM

**Memory block**: a single unit (byte) within memory, used to hold some value

**Memroy address**: The address of where a memory block is located

### A list of memory size used by different data types

![C data types memory storage image](./images/C-data-types-size.png)


Example in C:
```cpp
#include <stdio.h>

int main() {
    char a = 'X'; 
    short b = 'Y'; // can use characters as it will covert to ASCII
    int c = 'Z';    

    printf("Size of char: %zu bytes\n", sizeof(a));   // Use %zu for size_t
    printf("Size of short: %zu bytes\n", sizeof(b));  // Use %zu for size_t
    printf("Size of int: %zu bytes\n", sizeof(c));    // Use %zu for size_t

    printf("Address of a: %p\n", (void*)&a);  // Use %p for address
    printf("Address of b: %p\n", (void*)&b);  // Cast to void* to match %p
    printf("Address of c: %p\n", (void*)&c);  // Cast to void* to match %p

    return 0;
}
```

output:
```
1 byte
2 byte
4 byte
000000000061FE1F // memory address for a in hexadecimal form
000000000061FE1D // memory address for b in hexadecimal form
000000000061FE1 // memory address for c in hexadecimal form
```

Reference is when a data is stored in memory it has a memory address. A variable points to this memory address. This is called its reference.

# REFERENCES

How to use pointers in C: https://www.freecodecamp.org/news/pointers-in-c-programming/

Data types in C: https://www.geeksforgeeks.org/data-types-in-c/

Primitive & non primitive in JS: https://blog.stackademic.com/understanding-the-difference-between-primitive-and-non-primitive-data-types-in-javascript-c5251c0293db

# Random Access Memory

The main memory in a computer is called Random Access Memory (often called RAM for short). When we run a program, the operating system loads the program into RAM. Any data that is hardcoded into the program itself (e.g. text such as “Hello, world!”) is loaded at this point.

The operating system also reserves some additional RAM for the program to use while it is running. Common uses for this memory are to store values entered by the user, to store data read in from a file or network, or to store values calculated while the program is running (e.g. the sum of two values) so they can be used again later.

You can think of RAM as a series of numbered boxes that can be used to store data while the program is running.

In some older programming languages (like Applesoft BASIC), you could directly access these boxes (e.g. you could write a statement to “go get the value stored in membox number 7532”).

# Objects and variables

In C++, direct memory access is discouraged. Instead, we access memory indirectly through an object. An object represents a region of storage (typically RAM or a CPU register) that can hold a value. Objects also have associated properties (that we’ll cover in future lessons).

The key point here is that rather than say “go get the value stored in mailbox number 7532”, we can say, “go get the value stored by this object” and let the compiler figure out where and how to retrieve the value. This means we can focus on using objects to store and retrieve values, and not have to worry about where in memory those objects are actually being placed.

Although objects in C++ can be unnamed (anonymous), more often we name our objects using an identifier. An object with a name is called a variable.

**NOTE: In general programming, the term object typically refers to an unnamed object in memory, a variable, or a function. In C++, the term object has a narrower definition that excludes functions.**

## Variable definition

In order to use a variable in our program, we need to tell the compiler that we want one. The most common way to do this is by use of a special kind of declaration statement called a definition.
A definition statement can be used to tell the compiler that we want to use a variable in our program.
```cpp
int x; // define a variable named x (of type int)
```
At **compile-time** (when the program is being compiled), when encountering this statement, the compiler makes a note to itself that we want a variable with the name `x`, and that the variable has the data type `int`.  From that point forward (with some limitations), whenever we use the identifier x in our code, the compiler will know that we are referring to this variable.

## Variable creation

At **runtime** (when the program is loaded into memory and run), each object is given an actual storage location (such as RAM, or a CPU register) that it can use to store values. The process of reserving storage for an object’s use is called **allocation**. Once allocation has occurred, the object has been created and can be used.

For the sake of example, let’s say that variable `x` is instantiated at memory location 140. Whenever the program uses variable `x`, it will access the value in memory location 140.

**NOTE: An object is “created” once actual storage has been reserved for the object’s use.**

## Data types

Objects are regions of storage that can store a data value. A **data type** (more commonly just called a **type**) determines what kind of value (e.g. a number, a letter, text, etc…) the object will store.

In C++, the type of an object must be known at compile-time, and that type can not be changed without recompiling the program. This means an integer variable can only hold integer values. If you want to store some other kind of value, you’ll need to use a different type.

**NOTE: The data type of an object must be known at compile-time (so the compiler knows how much memory that object requires).**
C++ also allows you to create your own custom types and it’s part of what makes C++ powerful.

## Summary

In C++, we use objects to access memory. A named object is called a variable. Each variable has an identifier, a type, and a value (and some other attributes that aren’t relevant here). A variable’s type is used to determine how the value in memory should be interpreted.

Variables are actually created at runtime, when memory is allocated for their use.

* What is a value?

A value is a letter (e.g. a), number (e.g. 5), text (e.g. Hello), or instance of some other useful concept that can be represented as data.

* What is an object?

An object is a region of storage (usually memory) that can store a value.

* What is a variable?

A variable is an object that has a name.


# Variable initialization

One downside of assignment is that assigning a value to a just-defined object requires two statements: one to define the variable, and another to assign the value.

These two steps can be combined. When an object is defined, you can optionally provide an initial value for the object. The process of specifying an initial value for an object is called initialization, and the syntax used to initialize an object is called an initializer. Informally, the initial value is often called an “initializer” as well.

For example, the following statement both defines a variable named width (of type int) and initializes it with the value 5:

```cpp
#include <iostream>

int main()
{
    int width { 5 };    // define variable width and initialize with initial value 5
    std::cout << width; // prints 5

    return 0;
}

In the above initialization of variable width, { 5 } is the initializer, and 5 is the initial value.
```


## Different forms of initialization
Unlike assignment (which is generally straightforward), initialization in C++ is surprisingly complex. So we’ll present a simplified view here to get started.

There are 5 common forms of initialization in C++:

```cpp
int a;         // default-initialization (no initializer)

// Traditional initialization forms:
int b = 5;     // copy-initialization (initial value after equals sign)
int c ( 6 );   // direct-initialization (initial value in parenthesis)

// Modern initialization forms (preferred):
int d { 7 };   // direct-list-initialization (initial value in braces)
int e {};      // value-initialization (empty braces)
```

Other forms of initialization include:

* Aggregate initialization
* Copy-list-initialization
* Reference initialization
* Static-initialization, constant-initialization, and dynamic-initialization
* Zero-initialization

### Default-initialization

When no initializer is provided (such as for variable a above), this is called default-initialization. In many cases, default-initialization performs no initialization, and leaves the variable with an indeterminate value (a value that is not predictable).

### Copy-initialization

When an initial value is provided after an equals sign, this is called copy-initialization. This form of initialization was inherited from the C language.

```c
int width = 5; // copy-initialization of value 5 into variable width
```

Much like copy-assignment, this copies the value on the right-hand side of the equals into the variable being created on the left-hand side. In the above snippet, variable width will be initialized with value 5.

Copy-initialization had fallen out of favor in modern C++ due to being less efficient than other forms of initialization for some complex types. However, C++17 remedied the bulk of these issues, and copy-initialization is now finding new advocates.

**NOTE: Copy-initialization is also used whenever values are implicitly copied, such as when passing arguments to a function by value, returning from a function by value, or catching exceptions by value.**

### Direct-initialization

When an initial value is provided inside parenthesis, this is called direct-initialization.

```c
int width ( 5 ); // direct initialization of value 5 into variable width
```

Direct-initialization was initially introduced to allow for more efficient initialization of complex objects (those with class types). Just like copy-initialization, direct-initialization had fallen out of favor in modern C++, largely due to being superseded by direct-list-initialization. However, direct-list-initialization has a few quirks of its own, and so direct-initialization is once again finding use in certain cases.

Direct-initialization is also used when values are explicitly cast to another type (e.g. via `static_cast`).

One of the reasons direct-initialization had fallen out of favor is because it makes it hard to differentiate variables from functions. For example:

```c
int x();  // forward declaration of function x
int x(0); // definition of variable x with initializer 0
T(5);     // function call if T is a function, direct-initialization of temporary object if T is a type
```

### Direct-list-initialization and copy-list-initialization

The modern way to initialize objects in C++ is to use a form of initialization that makes use of curly braces. This is called **list-initialization** (or **uniform initialization** or **brace initialization**).

List-initialization comes in two forms:

```cpp
int width { 5 };    // direct-list-initialization of initial value 5 into variable width (preferred)
int height = { 6 }; // copy-list-initialization of initial value 6 into variable height (rarely used)
```

When we see curly braces, we know we’re creating and initializing an object.

Additionally, list initialization provides a way to initialize objects with a list of values rather than a single value (which is why it is called “list initialization”).

### List initialization disallows narrowing conversions

The primary benefit of list-initialization is that “narrowing conversions” are disallowed. This means that if you try to list-initialize a variable using a value that the variable can not safely hold, the compiler is required to produce a diagnostic (compilation error or warning) to notify you. For example:


```cpp
int main()
{
    // An integer can only hold non-fractional values
    int w1 { 4.5 }; // compile error: list init does not allow narrowing conversion of 4.5 to 4

    int w2 = 4.5;   // compiles: copy-init initializes width with 4
    int w3 (4.5);    // compiles: direct-init initializes width with 4

    return 0;
}
```

**NOTE: List-initialization is generally preferred over the other initialization forms because it works in most cases (and is therefore most consistent), it disallows narrowing conversions (which we normally don’t want), and it supports initialization with a list of values (something we’ll cover in a future lesson).**

### Value-initialization and zero-initialization

When a variable is initialized using empty braces, value initialization takes place. In most cases, value initialization will initialize the variable to zero (or empty, if that’s more appropriate for a given type). In such cases where zeroing occurs, this is called zero-initialization.

```cpp
int width {}; // value-initialization / zero-initialization to value 0
```

### Q: When should I initialize with { 0 } vs {}?

Use direct-list-initialization when you’re actually using the initial value:

```cpp
int x { 0 };    // direct-list-initialization with initial value 0
std::cout << x; // we're using that 0 value here
```

Use value-initialization when the object’s value is temporary and will be replaced:

```cpp
int x {};      // value initialization
std::cin >> x; // we're immediately replacing that value so an explicit 0 would be meaningless
```

## Instantiation

The term instantiation is a fancy word that means a variable has been created (allocated) and initialized (this includes default initialization). An instantiated object is sometimes called an instance. Most often, this term is applied to class type objects, but it is occasionally applied to objects of other types as well.

multiple variables defined on the same line:

```cpp
int a = 5, b = 6;          // copy-initialization
int c ( 7 ), d ( 8 );      // direct-initialization
int e { 9 }, f { 10 };     // direct-list-initialization
int i {}, j {};            // value-initialization
```

## The `[[maybe_unused]]` attribute C++17

The `[[maybe_unused]]` attribute allows us to tell the compiler that we’re okay with a variable being unused. The compiler will not generate unused variable warnings for such variables.

The following program should generate no warnings/errors:

```cpp
#include <iostream>

int main()
{
    [[maybe_unused]] double pi { 3.14159 };  // Don't complain if pi is unused
    [[maybe_unused]] double gravity { 9.8 }; // Don't complain if gravity is unused
    [[maybe_unused]] double phi { 1.61803 }; // Don't complain if phi is unused

    std::cout << pi << '\n';
    std::cout << phi << '\n';

    // The compiler will no longer warn about gravity not being used

    return 0;
}
```
Additionally, the compiler will likely optimize these variables out of the program, so they have no performance impact.

The `[[maybe_unused]]` attribute should only be applied selectively to variables that have a specific and legitimate reason for being unused (e.g. because you need a list of named values, but which specific values are actually used in a given program may vary). Otherwise, unused variables should be removed from the program.

### Q. What are default-initialization and value-initialization? What is the behavior of each? Which should you prefer?


Default-initialization is when a variable initialization has no initializer (e.g. int x;). In most cases, the variable is left with an indeterminate value.
Value-initialization is when a variable initialization has an empty brace initializer (e.g. int x{};). In most cases this will perform zero-initialization.
You should prefer value-initialization, as it initializes the variable to a consistent value.










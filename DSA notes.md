# What is an algorithm?

An algorithm is a set of well-defined instructions to solve a particular problem

input --> Algortihm --> Output

Eg:-

### To add two numbers

input - Two numbers 'a' and 'b'
Algorithm - 1. Add numbers using '+' 2. Return the value
Output - Sum of 'a' and 'b'

## Characteristics of an Algorithm

- Well defined inputs and outputs
- Each step be clear and unambiguous
- Language independent

# Algorithm analysis

The absolute running time of an algorithm cannot be predicted, since it depends on a number of factors like programming language, your computer, other programs running at the same time, quality of the OS etc.

Therefore we evaluate the performance of an algorithm in terms of its input size.

1. **Time complexity** - Amount of time taken by an algorithm to run, as a function of input size
2. **Space complexity** - Amount of memory taken by an algorithm to run, as a function of input size

By evaluating against the input size, the analysis is not only machine independent but the comparison is also more appropriate

## How to represent complexity

### Asymptotic notations

Mathematical tools to represent time and space complexity

1. Big-O Notation (O-notation) - Worst case complexity
2. Omega Notation (&ohm;-notation) - Best case complexity
3. Theta Notation (&theta;-notation) - Average case complexity

# Big-O Notation

Big-O notation describes the complexity of an algorithm using algebraic terms.
It has two important characteristics

- It is expressed in terms of the input
- It focuses on the bigger picture without getting caught up in the minute details

## Time complexities

- O(1) - Constant
- O(n) - Linear
- O(n^2) - Quadratic
- O(n^3) - Cubic
- If input size reduces to half every iteration, it is logarithmic - O(logn) - Logarithmic

## Space complexity

O(1) - when there is no need of additional space except the input value.
O(n) - When the space grows as input size.
O(logn) - When the sppace grows but not at the same rate as input size.

![Big O complexity chart](../../Pictures/BigO_complexity_chart.png)

## Objects - Big-O

An object is a collection of key value pairs

Insert - O(1)
Remove - O(1)
Access - O(1)
Search - O(n)
Object.keys() - O(n)
Object.values() - O(n)
Object.entries() - O(n)

## Array - Big-O

Insert / remove at the end of the array - O(1)
Insert / remove at the beginning - O(n)
Access - O(1)
Search - O(n)
Push / pop - O(1)
Shift / unshift / concat / slice / splice - O(n)
forEach / map / filter / reduce - O(n)

## Recursion

## Algorithm design techniques

1. Brute Force

Simple and exhaustive technique that evaluates every possible outcome to find the best solution. 
Eg: Linear Search

2. Greedy

Choose the best option at the current time, without any consideration for the future. 
Eg: Dijkstra's algorithm, Prim's algorithm and Kruskal's algorithm

3. Divide and Conquer

Divide the problem into smaller sub-problems. Each sub-problem is then solved and the partial solutions are recombined to determine the overall solution. 
Eg; Binary Search, Quick Sort, Merge Sort and Tower of Hanoi

4. Dynamic Programming

Divide the problem into smaller sub-problems. Break it down into smaller but overlapping sub problems. Store ther result and reuse it for the same sub-problems. This is called memoization and is an optimization technique that improves the time complexity of your algoritms. 
Eg: Fibonacci numbers and climbing staircase

5. Backtracking

Generate all possible solutions. check if the solution satisfies all the given constrains and only then you proceed with generating subsequent solutions. If the constraints are not satisfied, backtrack and go on a differnt path to find the solution.
Eg: N-Queens problems

## More problem suggestions

* Find the GCD using Euclidian algorithm
* Finding permutations and combinations of a list of numbers
* Finding the longest common substring in a given String
* Knapsack problem

Master theorem for calcuating time complexity for recusrsions

# Data Structure

A data structure is a way to store and organize data so that it can be used efficiently

A data structure is a collections of data values, the relationships among them, and the functions or operations that can be applied to that data

some example scenarios data structures is used:
DOM - Tree data structure
Browser back and forward - Stack data structure
OS job scheduling - Queue data structure

# Built in Data structures

## 1. Array

Big-O time Complexity

Insert/remove form end - O(1)
Insert/remove form beginning - O(n)
Access - O(1)
Search - O(n)
Push/Pop  - O(1)
shift/ unshift/ concat/ slice/ splice/ forEach/ map/ filter/ reduce - O(n)

## 2. Object

Big-O time Complexity

Insert - O(1)
Remove - O(1)
Access - O(1)
Search - O(n)
Object.keys() - O(n)
Object.values() - O(n)
Object.entries() - O(n)

## 3. Set

* A Set is a data structure that can hold a collection of values. The values however must be unique. 
* Sets can contain a mix of different data types.
* Sets are dynamically sized.
* Sets do not maintain an insertion order.
* Sets are iterables. Can be used with for of loop

**Set vs Array**

* Arrays can contain duplicate values whereas Sets cannot.
* Insertion orders is maintained in  arrays but it is not in the case with Sets.
* Searching and deleting an element in the set is faster compared to Arrays.

## 4. Map

* A map is an unordered collection of key-value pairs. Both keys and values can be of any data type
* To retrieve a value, you can use the corresponding key.
* Maps are iterables. They can be used with a for of loop.

**Objects vs Maps**

* Objects are unordered whereas Maps are ordered.
* Keys in objects can only be string or symbol types whereas in Maps, they can be of any type
* An object has a prototype and may contain a few default keys which may collide with your own keys if you're not careful. A Map on the other hand does not contain any keys by default.
* Objects are not iterables where as Maps are iterables.
* The number of items in an object must be determined manually whereas it is readily available with the size property in a Map.
* Apart from storing data, you can attach functionality to an object whereas Maps are restricted to just storing data.
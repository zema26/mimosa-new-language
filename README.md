# mimosa language specifications and design

Welcome to the specifications of the **mimosa programming language**. These specs will walk you through mimosa's unique syntax, program structure, and features, using enclosed examples

**mimosa** is designed with a primary goal: to make **AI coding** easier, much like natural writing. This is achieved through a few core syntactical changes that streamline the AI development process

\---

## 1\. The Basics: Syntax \& Variables

First, let's cover the fundamental building blocks of the mimosa language.

### Key Concepts

**It's time to change syntax of programming languages**

* **First: New Assignment Operator:** The most significant change is the assignment operator. Instead of ` variable = value `, mimosa uses ` value : variable `. This allows for a natural left-to-right flow mimicking handwriting. For example: ` a + b : a `. Notice: assignment operator and variable declaration look the same: ` i : int `

* **Second: Renamed Keywords:** 
  * if - when
  * else - other
  * while - repeat
  * for - iterate
  * function - fun 
  * return - back

* **Third: Semitagged Keywords:** Control flow and structural keywords are started and ended with symbol "|", much like a markup language. Examples include `repeat|`...`|repeat` , `fun|`...`|fun` , and `when|`...`|when` This makes it easy to find mistakes. So, blocks are explicit and self-closing with pipes.

* **Comments:** Single-line comments begin with `//`.

### Data Types & Operators

mimosa supports standard data types:

* `int`: integer
* `float`: float decimal
* `char`: character
* `string`: vector of "char"
* `bool`: boolean "true" or "false"

It also supports standard arithmetic operators: `+`, `-`, `*`, `/`, and `%` (mod) .

### Variable Declaration

You can declare a variable with or without an initial value

* **Declaration only:** ` i : int `
* **Declaration with initialization:** ` i(0) : int ` (i becomes 0)  , ` x(1.5) : float ` , ` c("a") : char `  , ` l(true) : bool `  , ` s("abcde") : string ` .

\---

## 2\. Program Structure \& Functions

mimosa programs are organized into components and functions.

### Components

A program is defined within a `comp| ... |comp` block.

```mimosa
comp| Euclidean 		// comp itself
    ...
|comp
```



### Function Declaration

Function declarations are also reversed to fit the left-to-right flow .

* **Syntax:** `fun| <function_name>(<input_parameters >) : <return_type>`
* **Example:** `fun| Euclid(a : int, b : int) : int`   (This declares a function named `Euclid` that takes two `int` parameters and returns an `int` ).
* **Calling a function:** `Euclid(a, b)`

### The `main` Function

The entry point for your program is the `main` function.

* **Declaration:** `fun| main(args|| : string) : int`

### Example 1: `Euclidean` Program

This full example demonstrates a simple comp, function declaration, and mimosa's powerful I/O syntax.

```mimosa
comp| Euclidean 		// comp itself

    fun| Euclid(a : int, b : int) : int  // function with type int
							
	repeat| b!= 0
		when| a > b
			    a - b : a
		    other|
			    b - a : b
		    |other
    	|when
	|repeat

	back| a |back     //this return of value by function, not a vector

    |fun

  
  fun|	main(args|| : string) : int  	// main function

	int -> a, b

    Euclid(a, b) -> out

  |fun

|comp
```


\---

## 3\. Control Flow & Vectors

mimosa provides standard control flow mechanisms and vector support.

### Conditional: `when|` / `other|`

The `Euclidean` example shows a simple `when|` / `other|` block.

```mimosa
when| a > b
        a - b : a
    other|
        b - a : b
    |other
|when
```



### Loop: `repeat|`

The `Euclidean` example also uses a `repeat|` loop.

```mimosa
repeat| b!= 0
    ...
|repeat
```



### Vectors

* We use vectors instead of arrays because they are more efficient
* Notice | | brackets instead of [ ]

  * **Declaration:** ` a|n| : int ` (declares an integer vector with ` n ` elements).
  * **Initialization:** ` a|4|(|1, 2, 3, 4|) : int `.
  * **Access:** `a|i|` (0-based indexing).

### Loop: `iterate|`

mimosa has a specific syntax for `iterate| ..  |iterate` loops .

* **Syntax:** `iterate| i(0)++ < n`
* **Explanation:**

  * ` i(0) `: The iterator ` i ` is declared and initialized to 0 .
  * ` ++ `: The iteration step is by 1 .
  * ` < n `: The loop continues as long as ` i ` is less than ` n ` .

Here is an example of a `iterate| .. |iterate` loop used to populate an vector:

```mimosa
k : int
iterate| k(0)++ < n       // fill with true
    true : primes|k|
|iterate
```



\---

## 4\. Advanced Example: Sieve of Eratosthenes

The following program, `primes`, uses vectors and loops to implement the Sieve of Eratosthenes algorithm.

```mimosa
comp| primes 

        fun| Eratosthenes(n : int) : int       // function declaration
             
            primes |n| : bool       // variable declaration
            l(0), i(0), index_square(3) : int        

            first, last, factor : int
            k : int
            iterate| k(0)++ < n       // fill with true
                true : primes|k|
            |iterate   
                
            repeat| index_square < n         
                when| primes|i|             

                    0 + index_square : first
                    0 + n : last  
                    i + i + 3 : factor
                    false : primes|first|  

                    repeat| last - first > factor 
                        first + factor : first
                        false : primes|first|   
                    |repeat
                    
                |when  
                i + 1 : i 
                2 * i * (i + 3) + 3 : index_square   

            |repeat          

            ' 2' ->  out  // print out

            iterate| i(0)++ < n         // print out
                when| primes|i| 
                    when| 2 * i + 3 : n 
                         break
                    |when

                    ' ' 2 * i + 3  ->  out
                    l + 1 : l
 
                    when| l % 10 = 0 
                        '\n'  ->  out
                    |when   
          
                |when        // when
            |iterate         // print out

            '\n number: ',  l  ->  out

        |fun            // erato fun
     
        fun|  main(args|| : string) : int 		// main function
 
                   Eratosthenes(1000)       // function call

        |fun

|comp
```



This example demonstrates vector manipulation (e.g., `false : primes|first|`), nested loops (`repeat|` inside `repeat|`  ), and sending formatted output to the console (e.g., ` '\\n'-> out ` ).

\---

## 5\. Object-Oriented Programming: Classes

mimosa also supports classes, allowing for object-oriented design.

* **Declaration:** `class| ClassName ... |class`.
* **Members:** You can define data members (e.g., `a : int, b : int`)   and member functions (e.g., `fun| Euclid...`)  inside a class.
* **Object Creation:** ` N : GCD ` creates an instance (object) `N` of the class `GCD`.
* **Access:** Members are accessed using the dot operator (e.g., `N.a`, `N.Euclid`).

### Example 3: `GCD` Class

Here is the `Euclidean` algorithm refactored into a class.

```mimosa
comp| Euclidean

    class| GCD    //class declaration

        a : int , b : int     //data members

        fun| Euclid(a : int, b : int) : int    //member function

            repeat| b!=0
               when| a > b
                        a - b : a
                    other|
                        b - a : b
                    |other
                |when
            |repeat

            back| a |back

        |fun
    |class


    fun| main(args|| : string) : int   //main function

        N : GCD    //object N of class GCD

         in -> N.a, N.b  
         
         N.Euclid > out    
                  
    |fun

|comp
```



Once again, the `main` function showcases mimosa's natural flow.  The first line `in -> N.a, N.b` reads input directly into the object's data members `N.a`, `N.b`, second line calls the object's member function `N.Euclid -> out`, and prints the returned result.

\---

## 6.Intermodular

mimosa components are connected through Intercomps

### Intercomp

Every component can be connected to other components through intercomps. Intermcomp goes before component itself

### Example: Intercomp and Component

This example demonstrates flexibility and interconnection of component using mentioned before `Euclidian` algorithm

```mimosa

intercomp|

   share| mimo.io |share    // using standard input and output
   share| Algo.algorithm |share     // connecting to another component

   share|  fun| Euclid(a : int, b : int) : int |fun |share      // function that can be used by another components

|intercomp

comp| Euclidean

    fun| Euclid(a : int, b : int) : int

    ...

    |fun

    ...

|comp

```

In the beginning intermodular gives component access to standard I/O and then access to functions of another component. Then there is declaration of function that can be used by another component. Notice that source code extension of mimosa components is ` .mimo `



\---

## 7.File System

### File Declaration

`f(name, type, options) : file` - file declaration and creating

* name includes path
* type can be: `bin`, `txt`, or `hex`
* options: `r` - read, `w` - write

### Example: writing and reading text file

```mimosa

f("Readme.md", txt, wr+) : file         //creating text file
s : strring     //srtring for reading and writing

f.open
repeat| in -> s    //string for writing exists like here is reading from standard input
    s -> f  //writing string to file
|repeat
f.close

f.open
repeat| ! f.eof  //reading
    f -> s  //reading string from file
|repeat
f.close

```

\---

## 8.Generics

This language utilizes C++ like generics

```mimosa

comp| generics

template| Iter : type, Val : type, Op : type |template

fun| connect(first : Iter, last : Iter, s : Val, operator : Op) : Val       

    repeat| first != last

        operator(s, *first) : s    // *first - element of type Val pointed by iterator first

        ++first

    |repeat

    back| s |back       // this is function return not a vector

|fun


fun|  main(args|| : string) : int

    vec|4| (|1,2,3,4|) : float      // vector with 4 elements

    connect(vec, vec + 4, 0.0, +) : s1 : float   // finding sum of floats, sum initialization 0.0

    connect(vec, vec + 4, 0, + ) : s2 :  float     // finding sum of integers, sum initialization 0

    connect(vec, vec + 4, 1.0, * ) : s3 :  float  // finding product of floats, product  initialization 1.0

    connect(vec, vec + 4, 1, * ) : s4 :  float  // finding product of integers, product initialization 1

|fun

|comp

```

Here you can see usage of generics for **different types**: ` float ` and ` integer ` and for **different operators**: **` + `** and **` * `**

\---

## 9.Concurrency

**mimosa** achieves concurrency through **Multiplex**:

```mimosa

tasks||: Multiplex       //vector of tasks for running together

function1(a, b) : tasks|0|     //list of fuctions for each task
function2 (i, n) : tasks|1|
...

i(0) : int
repeat| !eot        //end of time of execution

tasks|i++|.concur       //running tasks concurrently

|repeat

```

\---


## Origins

* **mimosa** language has its roots in **C++** and **Pascal** programming languages
* It has variable declaration `  i : int ` and function declaration ` f : int ` from **Pascal** and generics from **C++**



\---



[Valdemar](mailto:zema58@proton.me)



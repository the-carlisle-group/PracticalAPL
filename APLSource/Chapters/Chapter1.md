# Chapter 1 - Introduction to Arrays and Functions

## The Basics

Let's add two numbers in APL using the `plus` function: 

~~~
      2+3
5
~~~

A single number like 2, 3 or 5 above, is a called a `scalar`.
In APL we can also add lists of numbers:

~~~
      1 2 3+3 4 6   
4 6 9
~~~

A list of numbers is called a `vector`. 
If we try to add two vectors of different lengths together,
we get an error:

~~~
      1 2+3 4 5
LENGTH ERROR
~~~

However, we may add a scalar to a vector:

~~~
      10+1 2 3
11 12 13
~~~

This feature is called **scalar extension**. APl *extends* the scalar
to match the length of the vector, and effectively does:

~~~
      10 10 10+1 2 3
11 12 13
~~~

The `times` function is used for multiplication:

~~~
      2×3
6
~~~

Just like the the plus function, it too works on vectors:

~~~
      2×3 4 5
6 8 10
~~~

Similarly for `minus` and for division:

~~~
      2 4 6-1
1 3 5
      2 4 6÷2
1 2 3
~~~

Note that scalar extension works on either side of the function. 

These four functions, `+ - × ÷`, are classified as **scalar functions**.
Scalar functions are applied between the corresponding numbers on each
side of the function, and support scalar extention.
APL has many more scalar functions, each represented by a single symbol (or "glyph").
Two additional scalar functions are maximum (upstile symbol: `⌈`)
and minimum (downstile symbol `⌊`):

~~~
     11 0 10 7⌈9
11 9 10 9   
    0 5 1 6⌊1 2 3 4 
0 2 1 4
~~~

In all of our examples so far, a function has data on its left and its right.
This is the **dyadic** form of the function. The data on the right of a function 
is called the **right argument** and the data on the left of the function is called
the **left argument**. Almost all symbols have two forms,
a dyadic form and a **monadic** form, which takes data only on the right. So most
symbols are really two functions. The monadic and dyadic forms are usually closely
related. The monadic form of downstile is called floor and rounds down to nearest integer
while the monadic form of upstile is called ceiling and rounds up to the nearest integer:

~~~
      ⌊1.6 3.14 2.718
1 3 2
      ⌈1.6 3.14 2.718
2 4 3
~~~

## Order of Operations

In APL, all functions are created equal, none has higher precedence than another. 
There is one simple rule, evaluation is from right to left: 
      
      4×2+1
12    

In this expression, the plus funcion is evaluated first, producing 3, which is then 
multiplied by 4 to produce 12. Another way of reading the expression is 4 
times everything to right, which is 2 plus everything to the right.
 
Parenthese may be used to override the normal order of evaluation:
       
      (4×2)+1

9

In APL, we try to avoid the use of parenthesis if possible. This is often
possible by simply rearranging the order of the functions in an expression. 


## Negative Numbers

Negative numbers are represented by a **high minus** sign:

~~~
      6-10
¯4
~~~

The high minus may be thought of as phyically attached to the number,
and is separate and distinct from the minus function.
The minus sign's monadic form is **negate**, and it flips the sign of the number:

~~~
      -4
¯4
      -4-10
6
      4-¯2
6
      ¯4-10
¯14
~~~

In the first case the monadic function negate is applied to the number 4, yielding negative 4.
In the second case negate is applied to the result of 4 minus 10. In the third case, negative
2 is subtracted from 4, and in the forth case, 10 is subtracted from negative 4.

 
## Structural Functions

APL has other functions that are not scalar, and are referred to as structural.
The Greek letter iota (`⍳`), in its monadic form, is the index generator function, and the Greek letter
rho (`⍴`) is the **shape** function:  

~~~
     ⍳5
1 2 3 4 5
     ⍴8 9 10
3
~~~

The dyadic form of iota is `index of`, which looks up the items of the right argument in the left argument
and returns their location:

~~~
      56 43 54 22 8⍳54 56 9
3 1 6
~~~

The dyadic form of rho is `reshape`, which reshapes the right argument according to the left argument:

~~~
      5⍴1 
1 1 1 1 1
      5⍴1 2 3
1 2 3 1 2
      5⍴10 9 8 7 6 5 4 3 2 1
10 9 8 7 6
~~~

By inspection we can see that reshape will recycle elements if the right argument is too short,
and truncate elements if the right argument is too long.

The left argument to reshape may be a two-item vector (and even longer, as we shall see later):

~~~
      3 4⍴⍳12
1  2  3  4
5  6  7  8
9 10 11 12
~~~

The result is neither a scalar nor a vector, but a **matrix** of 3 rows and 4 columns. 
Scalar, vector, and matrix are the three most commonly used APL array shapes, but
APL functions can create and manipulate arrays of many more dimensions.


## Character Data

APL functions manipulate character data just like numeric data.
Character data is enclosed in single quotes, and just like numeric
data we may use shape and reshape on it:

~~~
      'A'
A
      ⍴'Hello world!'
12
      2 6⍴'Hello world!'
Hello 
world!
      'Hello world!'⍳'door'
11 5 5 9
~~~

We may choose to view a character vector as a word or a sentance
but APL knows only about arrays of characters.

## Operators

**Operators** are functions that take functions as arguments,
and return new functions. Consider adding up all the numbers
from 1 to 5. We could write:

~~~
      1+2+3+4+5
15
~~~

The reduce operator (the slash symbol /) takes a function
and inserts it between each element which is exactly what we
did manually above:
  
~~~
      +/1 2 3 4 5
  
15
~~~

In this case, the reduce operator takes the plus function as
its **left operand** and returns a new function (+/) that we
might call "sum". Thus to sum up the numbers from 1 to 1000:

~~~
     +/⍳100
5050
~~~

The reduce operator is useful with many other functions. Consider
finding the maximum or minimum value in a vector:


~~~
      ⌈/4 2 12 8 3
12
      ⌊/4 2 12 8 3
2
~~~

An operator looks to its left for a function, 
and then takes that function and applies it in some special way
to the data on the right.

## Boolean Values and Functions

In APL Boolean values, true and false, are represented by the numbers
1 and 0.  The scalar relational functions < ≤ = ≥ > all return Boolean values:

~~~
      5>3
1
      5 2<3 4
0 1
      5≥⍳10
1 1 1 1 1 0 0 0 0 0
      'hello world'='l'
0 0 1 1 0 0 0 0 0 1 0
~~~

Because Booleans are just the values 1 and 0, we can apply arithmetic
functions to them. Consider the question of how many values in a vector
are greater than 100:

~~~
     +/98 97 101 99 110 112>100
3
~~~

The scalar boolean functions all take boolean values as arguments and return
boolean values as results. 'Logical and'  and 'logical or' are the two
primary boolean functions:

~~~
      1∧0 
0
      1∨0
1
~~~

These functions are also supremely useful with the reduce operator.
And reduction answers the question are all items true, while or reduction
answers the questions are any items true:

~~~
      ∧/1 0 1 0
0
      ∨/1 0 1 0
1
      ∧/1 1 1 1
1
~~~

## Variables and Assignment

Arrays may be named by using the assignment arrow:

~~~
Greeting←'Hello world!'
         Greeting
Hello world!
         Ages←16 15 18 11 15 16 17 19 
         Ages
16 15 18 11 15 16 17 19 
~~~

These names are referred to as variables. The system command )vars
displays the variables in the session:

~~~
     )vars
Ages     Greeting   
~~~

Variables may be used as arguments to functions, and to save the result
of an expression for later use:

~~~
      AverageAge←(+/Ages)÷⍴Ages
      AverageAge
15.875
~~~

## User Defined Functions

So far all of the functions introduced have been single symbols. These
are built in to the APL interpreter. They are known as built-in functions or
primitive functions. We can also write our own functions, which will generally
call various primitive functions or other user defined functions. Here is 
a function that adds it's left argument to twice its right argument: 

~~~
      1 2 3{⍺+2×⍵}10
21 22 23
~~~

Functions are delimited by braces. The right argument of function is Greek letter omega,
and the left argument it the Greek letter alpha. A function may refer to alpha and omega in any
order, as many times as neccessary, even 0 times:

      {⍵ ⍵ ⍵}5
5 5 5
      2{⍵ ⍵ ⍵ ⍺ ⍺}5
5 5 5 2 2
      2{9}0
9

By definition, if a function does not include a reference to alpha, it is a mondadic function.
A function without alpha will not complain if you give it a left argument, it will just not use
it for anything. However, you must pass a right argument to a function whether it refers to omega
or not. 

The AverageAge expression above may be converted to a function:

~~~
      {(+/⍵)÷⍴⍵}Ages
15.875
~~~

These user defined functions are anonymous. They have no name. However we can give a name to a function
just like we can give a name to an array:

~~~
      Average←{(+/⍵)÷⍴⍵}
      Average 1 2 3 4 5
3
      Average Ages
15.875
~~~

The system command )fns displays the named functions in the session:

~~~
      )fns
Average 
~~~

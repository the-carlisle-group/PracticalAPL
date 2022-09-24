# Chapter 3 - Variables and User Defined Functions

> In which the glyphs `: ← { } ⍺ ⍵ ∇` are introduced
> and the terms variable, assignment, user-defined function, local variable, global variable,
> guard, error guard, recursion, workspace, system command are defined.  Multiline functions and
> the system commands )VARS and )FNS are explained.

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

These names are referred to as variables. The system command `)vars`
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
**primitive** functions. We can also write our own functions, which will generally
call various primitive functions or other user defined functions. Here is 
a function that adds it's left argument to twice its right argument:

~~~
      1 2 3{⍺+2×⍵}10
21 22 23
~~~

Functions are delimited by braces. The right argument of function is the Greek letter **omega**,
and the left argument it the Greek letter **alpha**. A function may refer to alpha and omega in any
order, as many times as neccessary, or not at all:

~~~
      {⍵ ⍵ ⍵}5
5 5 5
      2{⍵ ⍵ ⍵ ⍺ ⍺}5
5 5 5 2 2
      2{9}0
9

~~~

By definition, if a function does not include a reference to alpha, it is a monadic function.
A function without alpha will not complain if you give it a left argument, it will just not use
it for anything, and the same goes for omega. However, you must pass a right argument to a function whether it refers to omega
or not. 

The `AverageAge` expression above may be converted to a function by simply surround it with braces
and replacing the referencs to Ages with omega:

~~~
      {(+/⍵)÷⍴⍵}Ages
15.875
~~~

These user defined functions are anonymous; they have no name. However we can give a name to a function
just like we can give a name to an array:

~~~
      Average←{(+/⍵)÷⍴⍵}
      Average 1 2 3 4 5
3
      Average Ages
15.875
~~~

The system command `)fns` displays the named functions in the session:

~~~
      )fns
Average 
~~~


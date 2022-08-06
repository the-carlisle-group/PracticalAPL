# Chapter 2 - Character and Boolean Data

> In which the glyphs `= ≠ ≤ < > ≥ ∧ ∨ ~ ⌽ ↑ ↓ | *` are introduced
> and the terms character data, numeric data, mixed data
> Boolean value, Boolean function, true, false,
> and type are informally defined. 


## Character Data

APL functions can manipulate **character** data just like **numeric** data.
Character data is enclosed in single quotes, and just like numeric
data we may use `shape` and `reshape` on it:

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

We may choose to view a character vector as a word, phase, or sentence
but APL knows only about arrays of characters. 


MORE EXAMPLES OF CHAR DATA HERE
MIXED DATA


## Relational Functions and Boolean Values

The scalar dyadic functions represented by the glyphs =, ≠, , <  
are equals, not equals, .... repectivley. Each one of these functions returns a boolean value,
true or false, represented by a numeric 1 or 0:

...
   
  
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
 
Because Booleans are just the numeric values 1 and 0, we can apply arithmetic
functions to them. Consider the question of how many values in a vector
are greater than 100:

~~~
     +/98 97 101 99 110 112>100
3
~~~  

## Boolean Functions
 
and or not ∧ ∨  ~  , reductions

Boolean functions all take boolean values as arguments and return
boolean values as results. ***Logical and***  and ***logical or*** are the two
primary boolean functions:

~~~
      1∧0 
0
      1∨0
1
~~~

These functions are also supremely useful with the reduce operator.
***And reduction*** answers the question are ***all*** items true, while ***or reduction***
answers the questions are ***any*** items true:

~~~
      ∧/1 0 1 0
0
      ∨/1 0 1 0
1
      ∧/1 1 1 1
1
~~~



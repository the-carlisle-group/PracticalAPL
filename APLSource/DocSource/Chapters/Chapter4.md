# Chapter 4 - Arrays in Depth, Part 1

> In which the glyphs `≡ ≢ ¨` are introduced, and monadic use of `↑ ↓` demonstrated, 
> and the terms simple array, nested array, shape, tally, rank, and depth are introduced.
> User commands ]box and ]display are explained.

## Simple and Nested Arrays

In all of the examples in the first chapter, the arrays have all been **simple**.
This means that each item of the array is a simple scalar, like the character 'a',
or the number 5 or 2.7182.
Arrays, however, may be nested, which means that any item of the array may itself be a general array
and not specifically a scalar.
Nested numeric arrays may be composed by juxtapostion with parentheses:

~~~
      (1 2 3) (4 5) (6 7 8 9)
 1 2 3  4 5  6 7 8 9
~~~

This is a three item vector, where the first item is itself a three item vector,
the second item is a two element vector, and the third item is a four-element vector.  
Nested character arrays may be composed by juxtaposition with no need for parentheses:

~~~
      'January' 'February' 'March'
 January  February  March 
~~~

It is difficult to see the structure of the result in the session, as
by befault the APL interpeter just adds an extra blank between the items.
The user command `]box` sets the display of arrays to a more readable form:  

~~~
      ]box on
Was OFF
      (1 2 3) (4 5) (6 7 8 9)
┌─────┬───┬───────┐
│1 2 3│4 5│6 7 8 9│
└─────┴───┴───────┘
      'January' 'February' 'March'
┌───────┬────────┬─────┐
│January│February│March│
└───────┴────────┴─────┘
~~~

Now we can clearly see the structure of the array. Nesting can go arbitrarily deep:

~~~
     (1 2 3)  ((4 5)((6 7)(8 9 10)))
┌─────┬──────────────────┐
│1 2 3│┌───┬────────────┐│
│     ││4 5│┌───┬──────┐││
│     ││   ││6 7│8 9 10│││
│     ││   │└───┴──────┘││
│     │└───┴────────────┘│
└─────┴──────────────────┘

     ('Edgar' 'Codd') ('Chris' 'Date') ('Adam' 'Smith')
┌────────────┬────────────┬────────────┐
│┌─────┬────┐│┌─────┬────┐│┌────┬─────┐│
││Edgar│Codd│││Chris│Date│││Adam│Smith││
│└─────┴────┘│└─────┴────┘│└────┴─────┘│
└────────────┴────────────┴────────────┘
~~~

Items of any array can be nested, not just vectors:

~~~
      3 3 ⍴(1 2) (3 4 5) (6 7)
┌───┬─────┬───┐
│1 2│3 4 5│6 7│
├───┼─────┼───┤
│1 2│3 4 5│6 7│
├───┼─────┼───┤
│1 2│3 4 5│6 7│
└───┴─────┴───┘
~~~

Thus we may have vectors of vectors, vectors of matrices, matrices of
vectors, and  so on.

Scalar functions, in addition to supporting scalar extention as
we saw in chapter 1, are also **pervasive**. This means that a scalar
function will go down as deep as necessary:

~~~
      3×(1 2 3) (4 5) ((6 7 8) (9 10))
┌─────┬─────┬────────────────┐
│3 6 9│12 15│┌────────┬─────┐│
│     │     ││18 21 24│27 30││
│     │     │└────────┴─────┘│
└─────┴─────┴────────────────┘
      1 2 3×(1 2 3) (4 5) ((6 7 8) (9 10))
┌─────┬────┬────────────────┐
│1 2 3│8 10│┌────────┬─────┐│
│     │    ││18 21 24│27 30││
│     │    │└────────┴─────┘│
└─────┴────┴────────────────┘
~~~

## Shape, Tally and Rank 

The shape of an array is a vector describing the 
length of each dimension. Scalars have no dimensions, 
thus the shape of a scalar is an empty vector:

~~~
     ⍴5

~~~

> Empty arrays will be discussed in detail later on

The shape of a vector is a 1-item vector,
describing the number of items in the vector:  

~~~
      ⍴1 2 3
3
      ⍴'Hello World'
11
      ⍴'Hello' 'World'
2
      ⍴('Edgar' 'Codd') ('Chris' 'Date') ('Adam' 'Smith')
3
~~~

The shape of a matrix is a two-item vector describing the number
of rows and colums:

~~~
      m←3 4⍴⍳12
      m
1  2  3  4
5  6  7  8
9 10 11 12
      ⍴m
3 4
~~~

A scalar has 0 dimensions, a vector has one dimension, and  matrix has 2 dimensions.
The precise term for the number of dimensions is **rank**. 
Therefore a scalar has rank 0, a vector has rank 1,
and a matrix has rank 2 (and a cube has rank 3). 

The rank of an array may be thought of as how many pieces of information
are needed to locate a top-level item in the array. A scalar requires no
information, as there is only one item. A vector requires one piece of infomation,
(the item number), and a matrix requires two pieces of information (a row number
and a column number).

The first item of the shape of an array has a special name: tally, and its own function to
return it:

~~~
   ≢m
3
~~~

Because a vector has only one dimension, the shape and the tally of vector look 
suspiciously the same:

~~~
     ⍴ 1 2 3 4 5
5
     ≢ 1 2 3 4 5
5
~~~

However, the shape is always a vector, while the tally is always a scalar.

The rank an an array may thus be determined by applying tally to the shape:

~~~
      ≢⍴m
2
~~~

This directly asks the array 'how many' (`≢`) 'dimensions' (`⍴`) 'do you have?'
We may also apply shape to the shape to compute the rank: 


~~~
      ⍴⍴m  
2                         
~~~

The `]display` user command shows more structure than ]box on,
and may be used to display a specific array in detail: 

~~~
      ]display ≢⍴m
 
2
 
      ]display ⍴⍴m
┌→┐
│2│
└~┘
~~~

From this it is easy to see that the tally of the shape is a scalar
while the shape of the shape is a vector. Both of these expressions compute 
the rank, but the former is the preferred method.


## Depth

The shape and rank of a vector is independent of how nested it is.
The depth of an array is a measure of how nested it is:

~~~
     ≡5
0
     ≡1 2 3
1
     ≡'Hello World'
1
     ≡'Hello' 'World'
2
     ≡('Edgar' 'Codd') ('Chris' 'Date') ('Adam' 'Smith')
3
~~~

The depth of a array may be thought of as the number of 
steps needed to navigate to a particular 
simple scalar. In the last example above, we would need 3 moves
to navigate to the 's' in Chris: first go to the 2nd item, then
to the 1st item (of that 2nd item), and finally to the 5th item
(of the 1st item of the 2nd item).


## The Each Operator 

Consider this nested array of names, and the tally of it:

~~~
        Names←('Chris' 'Date') ('Edgar' 'Frank' 'Codd') ('Adam' 'Smith')
        Names
┌────────────┬──────────────────┬────────────┐
│┌─────┬────┐│┌─────┬─────┬────┐│┌────┬─────┐│
││Chris│Date│││Edgar│Frank│Codd│││Adam│Smith││
│└─────┴────┘│└─────┴─────┴────┘│└────┴─────┘│
└────────────┴──────────────────┴────────────┘
        ≢Names
3      
~~~

The `each` operator allows us to compute the length of each item:

~~~
     ≢¨Names
2 3 2
~~~

Remember, an operator is a higher order function that takes a function as input
on its left (the left operand), and returns a new function that 
applies the left operand in a special way. The `each` operator applies its operand,
in this case tally, to each item in the array instead of to the whole array.

To find the length of each item inside each item, apply `each`  twice:

~~~
      ≢¨¨Names
┌───┬─────┬───┐
│5 4│5 5 4│4 5│
└───┴─────┴───┘
~~~

To compute the total number of letters in each full name:

~~~
      +/¨≢¨¨Names
9 14 9
~~~

To compute the length of the longest name:

~~~
      ⌈/+/¨≢¨¨Names
14
~~~

It is never necessary to use the `each` operator with a scalar function operand:
as, by definition, scalar functions have a built-in `each`:

~~~
                 2 3+¨(1 2 3) (4 5 6 7)
┌─────┬────────┐
│3 4 5│7 8 9 10│
└─────┴────────┘
                 2 3+(1 2 3) (4 5 6 7)
┌─────┬────────┐
│3 4 5│7 8 9 10│
└─────┴────────┘
~~~

## Vectors and Matrices: Trading Depth and Rank

Consider this vector of vectors:

~~~
Trucks←'Ford' 'Chevrolet' 'Honda' 'Ram'
      Trucks
┌────┬─────────┬─────┬───┐
│Ford│Chevrolet│Honda│Ram│
└────┴─────────┴─────┴───┘
      ⍴Trucks
4
      ≢⍴Trucks
1
      ≡Trucks
2
~~~

The `mix` function (monadic up-arrow) will take a vector of vectors and construct a 
matrix. If the individual vetors are not the same lenght, they are padded with blanks
as necessary: 

~~~
      TrucksMatrix←↑Trucks
      TrucksMatrix
Ford     
Chevrolet
Honda    
Ram      
      ⍴TrucksMatrix
4 9
      ≢⍴TrucksMatrix
2
      ≡TrucksMatrix
1
~~~

Mix increases the rank and moves us upward in depth (decreasing the depth). Conversely,
the `split`  function (mondadic down-arrow) 
decreases the rank, and moves us deeper in depth (increases the depth):

~~~
     ↓TrucksMatrix
┌─────────┬─────────┬─────────┬─────────┐
│Ford     │Chevrolet│Honda    │Ram      │
└─────────┴─────────┴─────────┴─────────┘ 
~~~

`Split` does not remove trailing blanks, so mix and split are only perfect
inverses if the original vectors are all the same length.


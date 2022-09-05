# Chapter 5 - Arrays in Depth

> In which the glyphs `≡ ¨ ⊂ ⊃ ,` are introduced and the monadic use of `↑ ↓` demonstrated. 
> The terms simple array, nested array, juxtaposition, catenation, strand notation,
> and depth are discussed.
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


## Depth

The **depth** of an array is a measure of how nested it is:

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
steps needed to navigate to the deepest 
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

## Match

The `match` function, dyadic `≡`, compares two arrays for equality.
While the equals function (`=`) is a scalar function that compares simple scalars
`match` is a structural function that compares whole arrays:

~~~
      'Ram' = 'Car'
0 1 0
      'Ram' ≡ 'Car'
0
      'Ram'='Ford'
LENGTH ERROR: Mismatched left and right argument shapes
      'Ram'='Ford'
           ∧
      'Ram'≡'Ford'
0
      'a'='Ram'
0 1 0
      'a'≡'Ram'
0
~~~

The equals function, like all scalar functions requires conforming array arguments, supports
scalar extention, and returns a result that is the same structure as its argument.
The match function will never give an error, and always produces a scalar 1 or 0 as a result.

## Catenate and Ravel

So far, we have created simple and nested arrays by juxtapostion:

~~~
      2 7 1 8 2 8
2 7 1 8 2 8
      (1 2 3) (4 5 6 7)
┌─────┬───────┐
│1 2 3│4 5 6 7│
└─────┴───────┘
      'One' 'Two' 'Three'
┌───┬───┬─────┐
│One│Two│Three│
└───┴───┴─────┘ 
       m1←2 3⍴⍳6
       m2←2 2⍴'abcd'
       m1 m2
┌─────┬──┐
│1 2 3│ab│
│4 5 6│cd│
└─────┴──┘
~~~

That is, we have simply placed arrays
next to each other and they are bound together to form a larger
and deeper array. The `catenate` function (dyadic comma `,`)
joins two arrays without (generally) increasing the depth:

~~~
      (1 2 3),(4 5 6 7)
1 2 3 4 5 6 7
      'One','Two','Three'
OneTwoThree
      m1,m2
1 2 3 ab
4 5 6 cd 
~~~

Thus juxtapostion (almost) always increases depth, while catenation does not. 
The only exception to this rule are simple scalars.
If simple scalars are joined by catenation, the depth is increased. 


Monadic comma is the function `ravel`. It takes its argument
and returns a vector of its elements:

~~~
     ,m1
1 2 3 4 5 6
     ,m2
abcd
~~~

In this particular case, ravel is like the inverse of `reshape`.
By definition, when applied to a vector, `ravel` returns
its argument unchanged:

~~~
      ,'One'
One
      ,1 2 3 4
1 2 3 4
~~~

When applied to a scalar, `ravel` returns a 1-item vector:

~~~
     5
5
    ,5
5 
~~~

These look alike in the session, but one is a scalar and the 
other is a one item vector. The `]Display` user command uses
an extra level of boxes (over ]Box on), and some additional
decoration, that makes clear the difference between
a scalar and a 1-item vector:

~~~
      ]display 5
 
5
 
      ]display ,5
┌→┐
│5│
└~┘
~~~

Note the difference between justaposing and catenating
scalars versus 1-item vectors:

~~~
      s←3
      v←,5
      ]display s s s
┌→────┐
│3 3 3│
└~────┘
      ]display v v v
┌→────────────┐
│ ┌→┐ ┌→┐ ┌→┐ │
│ │5│ │5│ │5│ │
│ └~┘ └~┘ └~┘ │
└∊────────────┘
      ]display s v s
┌→────────┐
│   ┌→┐   │
│ 3 │5│ 3 │
│   └~┘   │
└∊────────┘
      ]display s,v,s
┌→────┐                                         a
│3 5 3│
└~────┘
~~~

## Enclose and Disclose

We have seen how mere juxtapostion creates depth indirecty. The enclose function (left shoe, `⊂`)
explicitly adds a level of depth to its argument:

~~~
      Greeting←'Hello world!'
      Greeting
Hello world!
      ⊂Greeting
┌────────────┐
│Hello world!│
└────────────┘
      (≢Greeting) (≡Greeting)
12 1
      (≢⊂Greeting) (≡⊂Greeting)
1 2
~~~

In this case, `enclose` take a list of 12 things and returns 1, deeper thing. Enclose will 
add a level of depth to an any item, even a nested scalar, so we may enclose multiple times:

~~~
      ⊂⊂⍳10
┌──────────────────────┐
│┌────────────────────┐│
││1 2 3 4 5 6 7 8 9 10││
│└────────────────────┘│
└──────────────────────┘
~~~

Note however that `enclose` has no effect on a simple scalar:

~~~
      5
5
      ⊂5
5
      5≡⊂5
1
~~~


To see the usefulness of `enclose`, consider the following two variables `Names` and `NewName`

~~~
       Names←'Bach' 'Hayden' 'Mozart'
       NewName←'Beethoven'
       Names
┌────┬──────┬──────┐
│Bach│Hayden│Mozart│
└────┴──────┴──────┘
       NewName
Beethoven
       (≢Names) (≡Names)
3 2
       (≢NewName) (≡NewName)
9 1
~~~
`Names` is a 3-item nested vector of character vectors. Its tally is 3, its depth is 2.
`NewName` is a 9-item simple vector. Its tally is 9, its depth is 1.' 
Consider how to combine `Names` and `NewName` to create the 4-item vector of vectors:

~~~
┌────┬──────┬──────┬─────────┐
│Bach│Hayden│Mozart│Beethoven│
└────┴──────┴──────┴─────────┘
~~~ 

First let's join all the names using juxtaposition:

~~~
      AllNames1←Names NewName
      AllNames1
┌────────────────────┬─────────┐
│┌────┬──────┬──────┐│Beethoven│
││Bach│Hayden│Mozart││         │
│└────┴──────┴──────┘│         │
└────────────────────┴─────────┘
      (≢AllNames1)(≡AllNames1)
2 ¯3
~~~

The result of the juxtapostion is a deeper, 2-item vector. Clearly not what we are looking for.
Second, let's join via the catenate function (comma `,`):

~~~
      AllNames2←Names,NewName
      AllNames2
┌────┬──────┬──────┬─┬─┬─┬─┬─┬─┬─┬─┬─┐
│Bach│Hayden│Mozart│B│e│e│t│h│o│v│e│n│
└────┴──────┴──────┴─┴─┴─┴─┴─┴─┴─┴─┴─┘
      (≢AllNames2) (≡AllNames2)
12 ¯2
~~~

Here the catenate function has a 3-item nested vector as its
left argument and a 9-item simple vector as its right argument yielding a 12-item vector
where the first three items are vectors and the last 9 items are simple scalars. This is also
clearly not what we are looking for.

Third and finally, lets try catenating the enclose of `NewName` on to the end of `Names`:

~~~
      AllNames3←Names,⊂NewName
      AllNames3
┌────┬──────┬──────┬─────────┐
│Bach│Hayden│Mozart│Beethoven│
└────┴──────┴──────┴─────────┘
      (≢AllNames3)(≡AllNames3)
4 2
~~~

The `enclose` function makes a scalar out of `NewName`, increasing its depth by 1, with 
the salutary result that the catenate function is now adding only one nested item to the list of names
rather than 12 simple scalars. 
 
The inverse of `enclose` is `disclose` (right show `⊃`). That is, for any array `A`,
`A≡⊃⊂A` is true. Thus disclose removes a level of nesting:

~~~
      ⊂'Hello world!'
┌────────────┐
│Hello world!│
└────────────┘
      ⊃⊂'Hello world!'
Hello world!

~~~

However, `disclose` is not only the inverse of `enclose`; it has some additional functionality,
and is also called `first`.
It selects and then discloses the first item in its argument:

~~~
      Names
┌────┬──────┬──────┐
│Bach│Hayden│Mozart│
└────┴──────┴──────┘
      ⊃Names
Bach
      ⊃4 5 6
4
      ⊃'Hello world!'
H
~~~      

The `first` function works on arrays of any rank:

~~~
       M←3 4⍴⌽⍳12
       M
12 11 10 9
 8  7  6 5
 4  3  2 1
       ⊃M
12
~~~

`First` is a more general definition that encompasses the definition of `disclose`.
It follows from this additional functionality that while `disclose` 
is strictly t.he inverse of `enclose`, the reverse is not true:

~~~
      Names
┌────┬──────┬──────┐
│Bach│Hayden│Mozart│
└────┴──────┴──────┘
      ⊃Names
Bach
      ⊂⊃Names
┌────┐
│Bach│
└────┘
      Names≡⊂⊃Names
0  
~~~

`Disclose`, like `enclose` has no effect on a simple scalar:

~~~
      ⊃5
5
      ⊃⊃5
5
      ⊂'a'
a
      ⊂⊂⊂'a'
a
~~~





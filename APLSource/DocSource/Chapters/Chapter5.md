# Chapter 5 - Arrays in Depth, Part 2

> In which the glyphs `⊂ ⊃ ,` are introduced, dyadic `≡` is explained 
> and the terms juxtaposition, catenation, strand notation, are defined.

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

## Disclose and Reduction

Consider the  













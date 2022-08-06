# Chapter 7 - Indexing, Sorting, and Searching 

> In which the glyphs `[] ⍒ ⍋ ⍸` are introduced dyadic `⍳` is explained,
> and the terms grading and sorting explained.

## Bracket Indexing

 (Note that reach indexing and functional indexing and scatter point indexing - later chapter)

## Index Of (Dyadic Iota)


## Grading and Sorting

Sorting in APL is a two-step process that begins with **grade up** (`⍋`) or **grade down** (`⍒`)' 
Grade up and grade down do not directly sort their arguments. They return the indices that may then be
used to sort the argument. Consider a set of test scores S, and the application of grade up and grade down:

~~~
      S←99 98 100 72 86 81 79  
      ⍋S
4 7 6 5 2 1 3
      ⍒S
3 1 2 5 6 7 4
~~~

In the result of grade up, the first item 4 indicates that the 4th item of S is the lowest item.
The second item 7 indicates that the 7th item of S is the next lowest item, and so on.
lowest item, and so on. In other words, grade up returns the indices of the items in S ordered
from the lowest to highest. Similarly grade down returns the indices of the items in S ordered from highest
to lowest. These indices may then be used to sort the vector S:

~~~
     S[⍋S]
72 79 81 86 98 99 100
     S[⍒S]
100 99 98 86 81 79 72  
~~~

Observe that the result of grade up and grade down is always a permution of integers from 1 to N, where N is the
length of the vector being graded, and thus indexing by the grade vector will select all of the items in a particular order.
Observe further that in the example above, the grade down of S is the exact reverse
of the grade up of S. This is only the case if the items of the argument are unique. If there are duplicates, the 
grade vectors will not be the reverse of each other: 

~~~
       V← 'Zuchini' 'Brocoli' 'Aubergine' 'Spinich'  'Zuchini' 
       ⍋V
3 2 4 1 5
       ⍒V
1 5 4 2 3
~~~

The usefulness of having sorting as a two-step process is that it is often the case that we want sort
one array based on another corresponding array. Consider:

~~~
          Names←'Bob' 'Lisa' 'Joe' 'Dave' 'Mary'
          Scores←101 99 85 99 89 
          Times←19 15 16 18 17
          M←⍉↑Names Scores Times
          M
 Bob   101 19
 Lisa   87 15
 Joe    85 16
 Dave   99 18
 Mary   99 17
~~~

To order the names by highest score:

~~~
      Names[⍒Scores]
 Bob  Dave  Mary  Lisa  Joe 
~~~

To order the names by fastest time:

~~~
      Names[⍋Times]
 Lisa  Joe  Mary  Dave  Bob 
~~~

To sort the rows of the matrix M by the highest score:

~~~
      M[⍒M[;2];]
 Bob   101 19
 Dave   99 18
 Mary   99 17
 Lisa   87 15
 Joe    85 16
~~~

The grade functions are deceptively simple. Consider the application of grade up applied to a grade up/down vecotor on the scores:

~~~
      Scores
101 87 85 99 99
      ⍋Scores
3 2 4 5 1
      ⍋⍋Scores
5 2 1 3 4
~~~

The grade of a vector is not in one to one correspondance with the original vector. In the above
example, the 3 in ⍋Scores does not correspond or relate to the 101 in Scores. Rather it indicates
the 3rd item in Scores (85) should be placed first when sorting in ascending order.

When grade up is applied a second time, we get a result where each item does indeed correspond directly to each item in 
the orginal vector. That is, 101 is the 5th lowest score, 87 is the second lowest score, 85 is the first lowest score, etc.

Double grade up thus *ranks* the scores.







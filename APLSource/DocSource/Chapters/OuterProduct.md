# Hints for Solving Problem 6 "Pyramid Scheme"

There are three hints provided by Dyalog for solving this problem: the minimum function, the reverse function,
and the outer product operator.

Let's review the outer product operator, read as "jot dot". This operator applies its
function between all possible combinations of its left and right arguments. Consider a simple multiplication table:

~~~
      1 2 3∘.×1 2 3
1 2 3
2 4 6
3 6 9
~~~

Every item in the left argument is multiplied by every item in the right argument, producing a matrix.
The arguments do not need to be the same length:   

~~~
      2 4 6∘.×1 2 3 4 5
2  4  6  8 10
4  8 12 16 20
6 12 18 24 30
~~~

Note that the shape of the left argument determines the number of rows in the result, and the shape of the
right argument determines the number of columns. The power of operators of course comes from the fact that we can use
different functions as the operand:

~~~
     1 2 3∘.⌈1 2 3 4 5
1 2 3 4 5
2 2 3 4 5
3 3 3 4 5 
~~~

Consider how the middle row of the result is determined. This is computed by taking the middle item from the left argument, 2, 
and applying the maximum function to the entire right argument:

~~~
      2⌈1 2 3 4 5
2 2 3 4 5
~~~

Final hint: It should be easy to generate the sequence of integers from 1 to N, 
*catenated* to the *reverse* series from N-1 back down to 1. Start with this.

Bonus: The commute operator `⍨` takes a function and makes a new function that applies the right argument as
the left argument as well. That may sound confusing. Put it this way: whenever we have the exact same argument on both the left and right side
of a function, we may eliminate the left argument by inserting the commute operator:

~~~
     2+2
4
     +⍨2
4
~~~

So `+⍨` is a new function that adds its right argument to its right argument, or to itself.
It is often the case with outer product that the left and right arguments are the same.

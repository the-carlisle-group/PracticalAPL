# Chapter 4: Matrices

> In which the symbols `≢ ⍉ ⊖ ⌿ . ∘ ⌿` and the terms matrix, dimension, axis,
> and rank are introduced.

In the previous chapters, all arrays have been either scalars (a single number or character)
or vectors (a list of numbers and/or characters). A scalar is a zero-dimensional array.
A vector is one-dimensional array. 

The left argument to `reshape` may be a two-item vector (and even longer, as we shall see later):

~~~
        m←4 5⍴⍳20
        m
 1  2  3  4  5
 6  7  8  9 10
11 12 13 14 15
16 17 18 19 20   
~~~

The result is neither a scalar nor a vector, but a table or, more formally, a **matrix** of 4 rows and 5 columns. 
Scalar, vector, and matrix are the three most commonly used APL array shapes, but
APL can create and manipulate arrays of many more dimensions.
 
Just as scalar extension applies to vectors, it applies to matrices, so we can multiply an entire
matrix by 10: 

~~~
      10×m
 10  20  30  40  50
 60  70  80  90 100
110 120 130 140 150
160 170 180 190 200
~~~

or mark the items that are greater than 7:

~~~
      m>7
0 0 0 0 0
0 0 1 1 1
1 1 1 1 1
1 1 1 1 1
~~~

APL functions work on arrays of all dimensions, so all the primitive functions we have
learned to apply to vectors may be applied to matrices.

For example, to sum across the matrix, use the `reduce` operator:

~~~
      +/m
15 40 65 90
~~~

But a matrix has two dimensions or axes. We have seen how to sum across the rows with  We can sum down the matrix using the `reduce first` (`⌿`) operator:  

~~~
      +⌿m
34 38 42 46 50
~~~

Summing along the last axis is summing the columns:

 ┌──┐  ┌──┐  ┌──┐  ┌──┐  ┌──┐ 
 │ 1│  │ 2│  │ 3│  │ 4│  │ 5│ 
 │ 6│  │ 7│  │ 8│  │ 9│  │10│ 
 │11│  │12│  │13│  │14│  │15│ 
 │16│  │17│  │18│  │19│  │20│ 
 └──┘  └──┘  └──┘  └──┘  └──┘ 

                              

Summing along the first axis is summing the rows:






The structural functions  `take` (`↑`) and `drop` (`↓`) extract or remove rows and columns from a
matrix. For matrices we may use one or two items as a left argument, the first for rows,
the second for columns:  

~~~
      2↑m
1 2 3 4  5
6 7 8 9 10
      ¯2↑m
11 12 13 14 15
16 17 18 19 20
      2 3↑m
1 2 3
6 7 8
      2 ¯3↑m
3 4  5
8 9 10
      3↓m
16 17 18 19 20
~~~

The reverse function reverses the order of the columns:

~~~
       ⌽m
 5  4  3  2  1
10  9  8  7  6
15 14 13 12 11
20 19 18 17 16
~~~

The rows of a matrix may be reversed using the `reverse first` function:

~~~
      ⊖m
16 17 18 19 20
11 12 13 14 15
 6  7  8  9 10
 1  2  3  4  5
~~~

We can also apply `rotate` to move columns from the front to the back
(and from the back to the front with a negative left argument), and `rotate first`
to do the same for rows, moving them from top to bottom
(bottom to top with a negative left argument): 

~~~
      2⌽m
 3  4  5  1  2
 8  9 10  6  7
13 14 15 11 12
18 19 20 16 17
      ¯1⊖m
16 17 18 19 20
 1  2  3  4  5
 6  7  8  9 10
11 12 13 14 15
~~~

The `transpose` function (`⍉`) rotates a matrix along its diagonal, turning rows into columns
and columns into rows:

~~~
      ⍉m
1  6 11 16
2  7 12 17
3  8 13 18
4  9 14 19
5 10 15 20
~~~

## Shape, Tally and Rank

The **shape** of an array is a vector describing the 
length of each dimension. Scalars have no dimensions, 
thus the shape of a scalar is an empty vector:

~~~
     ⍴5

~~~

The shape of a vector is a 1-item vector,
describing the number of items in the vector:  

~~~
      ⍴1 2 3
3
      ⍴'Hello World'
11
~~~

The shape of a matrix is a two-item vector describing the number
of rows and columns:

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
are needed to locate an item in the array. A scalar requires no
information, as there is only one item. A vector requires one piece of infomation,
(the item number), and a matrix requires two pieces of information (a row number
and a column number).

The first item of the shape of an array has a special name: **tally**, and its own function to
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

## Outer Product

The monadic outer product operator (`∘.`), commonly known as *jot dot*, applies its dyadic operand
function to all of the pairings of the items in the left and right argument.

> Note: this operator is composed of two glyphs, an historical anomaly of APL. It could easily be one symbol,
> and should be thought of as such. 

If the left and right arguments are vectors, the result is a matrix. For example, consider
*jot dot times*:

~~~
      1 2 3 4∘.×1 2 3 4
1 2  3  4
2 4  6  8
3 6  9 12
4 8 12 16 
~~~

By inspection we see we have created a multiplication table up to the 4s. We have multipled each
value in the left argument by every value in the right argument.
If the vector arguments are the same
length, the result is a square matrix, as above. If the vectors are different lenths, the result will
be a matrix with as many rows as the left argument has items, and as many columns as the right argument
has items: 

~~~
      'aeiou'∘.='ubiquitous'
0 0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0
0 0 1 0 0 1 0 0 0 0
0 0 0 0 0 0 0 1 0 0
1 0 0 0 1 0 0 0 1 0
~~~

Here we have compared each vowel with every letter in the word *ubiquitous*.
Each row in the result corresponds to a vowel. The last row represents the vowel
`u` and has three 1's in it, marking the occurences:  

~~~
    u b i q u i t o u s
  ┌─────────────────────
a │ 0 0 0 0 0 0 0 0 0 0
e │ 0 0 0 0 0 0 0 0 0 0
i │ 0 0 1 0 0 1 0 0 0 0
o │ 0 0 0 0 0 0 0 1 0 0
u │ 1 0 0 0 1 0 0 0 1 0
~~~

If the left argument is longer than the right argument, we will get a matrix
with more rows than columns:

~~~
      2 3 4 5 6∘.*2 4 8
 4   16     256
 9   81    6561
16  256   65536
25  625  390625
36 1296 1679616
~~~

Outer product is useful for generating identity matrices:

~~~
      i←⍳5
      i∘.=i
1 0 0 0 0
0 1 0 0 0
0 0 1 0 0
0 0 0 1 0
0 0 0 0 1
~~~

and upper and lower triangular matrices:

~~~
      i∘.>i
0 0 0 0 0
1 0 0 0 0
1 1 0 0 0
1 1 1 0 0
1 1 1 1 0
      i∘.≤i
1 1 1 1 1
0 1 1 1 1
0 0 1 1 1
0 0 0 1 1
0 0 0 0 1
~~~








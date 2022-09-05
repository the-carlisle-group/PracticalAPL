# Chapter 4: Matrices

> In which the symbols `≢ ⍉ ⊖ ⌿ . ∘ ⌿` and the terms matrix, dimension, axis, and rank are introduced.

In the previous chapters, all arrays have been either scalars (a single number or character)
or vectors (a list of numbers and/or characters). A scalar is a zero-dimensional array.
a vector is one-dimensional array. APL has two-dimensional arrays 

  
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

The structural functions  `take` (`↑`) can extract rows and columns


Reverse also works 

~~~
       ⌽m
 5  4  3  2  1
10  9  8  7  6
15 14 13 12 11
20 19 18 17 16
~~~

reversing the columns of the matrix, just like it reverses the items of a vector. 

The rows of a matrix may be reversed using the `reverse first` function:

~~~
      ⊖m
16 17 18 19 20
11 12 13 14 15
 6  7  8  9 10
 1  2  3  4  5
~~~

> A matrix has two dimensions or axes. The rows are the first dimension, and the columns are the second
dimension. The glyphs `⌽` is   

The `transpose` function (`⌽`) rotates a matrix along its diagonal, turning rows into columns
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

The shape of an array is a vector describing the 
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
are needed to locate an item in the array. A scalar requires no
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


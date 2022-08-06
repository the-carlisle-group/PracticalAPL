# Chapter 6 -  Set Functions

> In which the glyphs `∊ ∪ ∩ ⍷ ⍸` are introduced, dyadic `~` is presented,
> and the dual role of the glyphs `/` and `\\` as operators and functions is explained.


## Set Functions

The epsilon glyph `∊` in its dydadic form is the ***member of*** function. It tests
whether or not any item in the left argument is found anywhere in the right argument,
returning a boolean array:

~~~
      'Orange'∊'aeiouAEIOU'
1 0 1 0 0 1
~~~

The shape of the result is always the same shape as the left argument:  

~~~
       (3 4⍴⍳12)∊2 7 1
1 1 0 0
0 0 1 0
0 0 0 0                                                                      
~~~

The shape of the right argument is immaterial, as ***member of*** is just looking for the existance
of an item:

~~~
      'Orange'∊2 5⍴'aeiouAEIOU'
1 0 1 0 0 1
~~~

***Member of*** is a structural function, not a scalar function:

~~~
      'lime' 'mango'∊'apple' 'pear' 'lime'  
1 0
      'lime' 'mango'∊'apple' 'pear' (⊂'lime')  
0 0
      'lime'∊'apple' 'pear' 'lime'  
0 0 0 0
~~~

The downshoe glyph `∪` in its dyadic form is the ***union*** function, which returns
its left argument catenated with all of the items in its right argument that are not found
in the left argument:

~~~
      'Edgar' 'Allan' ∪ 'Allan' 'Poe'
┌─────┬─────┬───┐
│Edgar│Allan│Poe│
└─────┴─────┴───┘
      1 1 2 3 ∪ 4 1 1 2 3  5
1 1 2 3 4 5
~~~

***Union*** requires both of its arguments to be rank 1 or less (a vector or scalar),
and it always returns a vector. 

Monadic `∪` is the **unique** function. It returns the unique items in its argument,
or alternatively may be thought of as removing duplicate items:  

~~~
      ∪'Edgar' 'Allan' 'Allan' 'Poe'
┌─────┬─────┬───┐
│Edgar│Allan│Poe│
└─────┴─────┴───┘
      ∪'Mississippi'
Misp
~~~

***Unique*** works on higher rank arrays, but this behavior will be coverered in chapter X.

The upshoe glyph `∩` only has a dyadic form, the ***intersection*** function, which yields
all of the items in the left argument that exist in the right argument:

~~~
     'aeiou'∩'iota'
aio
~~~ 

Dyadic tilda (`~`) is the function ***without***. It returns its left argument without any
items that are found in the right argument: 

~~~
      2 7 1 8 2 8 1 8~2 8
7 1 1

~~~

The left argument to **without** must be a scalar or vector, and the result is always a vector.
Like **member of**, the shape of the right argument is immaterial.

> Reminder: monadic `~` is the ***logical not*** function, introduced in Chapter 2.

These set functions are structural functions. As such, they operate on the top level items of their
array arguments.  

## Replicate and Expand

The slash glyph '/' has already been introduced as the reduce operater. However it does
double duty as the *replicate*** function when there is an array and 
not a function to its immediate left:

~~~
      1 1 2 1 2 1 2 1/'Misisipi'
Mississippi
~~~

Each item in the right argument is replicated the number of times indicated by the correponding
integer in the left argument. A zero in the left argument indicates zero replications, effectively
omitting the corresponding item on the right:

~~~
      2 0 3/7 8 9
7 7 9 9 9
~~~

A negative integer in the left argument means replicating a zero or a blank for the corresponding 
item in the right argument, depending on whether or not the right argument is character or numeric:

  
The left argument must be conform in shape to the right argument,
though, scalar extention applies:

~~~
      3/⍳3
1 1 1 2 2 2 3 3 3
~~~

The most common use of replicate is with a boolean left argument, where the 1's indicate
items to keep, and the 0's items to discard. The boolean left argument may be thought of 
as a selelction vector, or a mask:

~~~
      1 0 1 0 1/⍳5
1 3 5
~~~

Often the left argument is itself a function of the right argument.
For example, to select all the values in a vector that are greater
than 100:

~~~
      v←99 99 95 91 94 101 91 104 94 88
      v>100
0 0 0 0 0 1 0 1 0 0
      (v>100)/v
101 104
~~~
         
When the left argument is a boolean vector, the **replicate** function is sometimes 
called **compress**.

The backslash glyph '/' has already been introduced as the scan operater. Like `/` is also 
does double duty as a dyadic function **expand**.






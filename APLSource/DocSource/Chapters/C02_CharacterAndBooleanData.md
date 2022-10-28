# Chapter 2 - Character and Boolean Data

> In which the glyphs `⌽ ↑ ↓ = ≠ ≤ < > ≥ ∧ ∨ ~ ∊` are introduced
> and the terms character data, numeric data,
> Boolean value, Boolean function, true, false,
> and type are informally defined. The dual use of the glyphs `/` and `\\` 
> is explained.

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
~~~

We may choose to view a character vector as a word, phase, or sentence
but APL knows only about arrays of characters.

The symbol circle style (`⌽`) in its mondaic form is the structural function `reverse`. It reverses the items in a vector:

~~~
      ⌽'reverse'
esrever
      ⌽⍳5
5 4 3 2 1
~~~

The dyadic form is `rotate`, which moves items from the front to the back with a positive left argument,
or the back to the front with a negative left argument:

~~~
      1⌽⍳5
2 3 4 5 1
      4⌽'oversleep'
sleepover
      ¯5⌽'oversleep'
sleepover
~~~

Note that `reverse` reverses the ordering of the items, while `rotate` maintains the overall ordering while moving items from
the back to the front or vice versa.

The dyadic functions `take` (`↑`) and `drop` (`↓`) extract or remove items from the beginning or the end of a vector:

~~~
      4↑'lookout'
look
      4↓'lookout'
out
      ¯3↑'lookout'
out
      ¯3↓'lookout'
look
~~~

Items from the middle of a vector may be extracted using `take` and `drop` repeatedly or in combination:

~~~
      5↑2↓'redoubtable'
doubt
      2↓¯4↓'redoubtable'
doubt
~~~
   
And of course these structural functions operate on all arrays, not just character arrays:

~~~
     4↓⍳10
5 6 7 8 9 10
~~~

## Relational Functions and Boolean Values

The scalar dyadic functions represented by the glyphs `= ≠ < > ≤` and `≥` are relational functions.
Each one of these functions returns a Boolean value,
true or false, represented by a numeric 1 or 0:

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

Or how many letter ls are in the phrase 'hello world':

~~~
      +/'l'='hello world'
3
~~~ 

## Boolean Functions

Boolean functions take Boolean values as arguments and return
Boolean values as results.The two primary boolean functions are
`and` (`∧`) and `or` (`∨`):

~~~
      1∧1 
1
      1∧0
0      
      0∧0
0
      1∨1
1
      1∨0
1
      0∨0
0
~~~

Like the `plus` function `+`, these are scalar functions, and they are commutative: `1∧0` yields the same result as `0∧1`.
Also like the `plus` function, these functions are supremely useful with the reduce operator.
***And reduction*** answers the question are ***all*** items true, while ***or reduction***
answers the questions are ***any*** items true:

~~~
      ∧/1 0 1 0
0
      ∧/1 1 1 1
1
      ∨/1 0 1 0
1
      ∨/0 0 0 0
0
~~~

The tilda glyph `~` in its monadic form is the Boolean `not` function. It simply
turns a 1 to 0 and vice versa. If something is not true it is false, and if something is not false it is true:

~~~
   ~1
0
   ~0
1    
   ~1 0 1
0 1 0
~~~

## The Membership Function

The epsilon glyph in its dyadic form is is the function `membership`. It returns a Boolean array indicating if an item
in the left argument is found anywhere in the array on the right:
 
~~~ 
      'aeiou'∊'ineluctable'
1 1 1 0 1
      'aeiou'∊'unequivocally'
1 1 1 1 1  
~~~   
   
Thus the shape of the result is the same as the left argument. Note that membership is not a scalar function.
 
## Replicate and Expand

We have already seen the reduce and the scan operators '/' and '\\'. These two glyphs, slash and backslash, however, perform double
duty, and are stand-alone functions when there is an array to their immediate left.
The slash glyph is the function `replicate`:  

~~~
      1 2 1 0/'loki' 
look                                                  
~~~

Each item on the right is repeated according to the corrsponding integer on the left. Repeating an item 0 times effectively removes it.
When `replicate` has a Boolean left argument, it may be thought of as a selection mask, keeping the items corresponding
to 1, and removing the items corresponding to 0:

~~~
    a←5 99 8 101 141 15
    a<100
1 1 1 0 0 1
    (a>100)/a
101 141
~~~

A Boolean left argument is the most common application of `replicate`. The left and right arguments must be the same
length, however, while replicate is not a scalar function, scalar extension applies:

~~~
      2 3 4/'a'
aaaaaaaaa
      3/'abc'
aaabbbccc
~~~

The backslash glyph is the function.       
   
   
   
   
   
   
   
   
   


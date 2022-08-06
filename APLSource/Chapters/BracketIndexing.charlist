# Bracket Indexing on Vectors

Consider the character vector 'Hello world':

~~~
      v←'Hello world'
~~~

The 11th item of array `v` may be selected using brackets:

~~~
      v[11]
d
~~~

Multiple items may be selected from a vector in any order using a vector of integers:

~~~
      v[7 2 10 11]
weld      
~~~

The scalar integers inside the brackets are called **indices**.
In addition to being in any order, indices may be repeated, selecting the same item multiple times:

~~~
      i←7 2 2 11
      v[i]
weed
      ⍴i
4
      ⍴v[i]
4
~~~

Note that the shape of the result of bracket indexing on a vector is the shape of the indices used to index it.
A vector may be indexed by a matrix of indices, returning a matrix:

~~~
      m←2 5⍴1 2 3 4 5 7 8 9 10 11
      m
1 2 3  4  5
7 8 9 10 11
      v[m]
Hello
world
~~~

Repeating indices is a useful technique for converting numeric patterns to character representations:

~~~
      '-+'[1 1 1 2 2 2]
---+++
      '-+'[1 2 1 2 1 2]
-+-+-+
      '-+'[1 1 2 1 1 1 2 1 1]
--+---+--
~~~

Now try to solve contest problem 5: Stairway to Heaven. 

For an alternative solution that does not use bracket indexing, consider using reshape and the fact that
when a vector of varying length character vectors is mixed into a matrix, they are padded to the same lenght using blanks.    



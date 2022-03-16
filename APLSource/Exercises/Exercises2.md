# Chapter 2 - Exercises

1. Write a function that returns the integers from `⍺` to `⍵` inclusive of the end points:

~~~
      5{your code here}10
5 6 7 8 9 10
~~~

Once you have your function working, give it a name so we can talk about, and so it is easy
to use over and over again:

~~~
     to←{your code here}
~~~

What kind of function did you write? Is it a scalar function? Or a structural function? How would
we call the function if we wanted to generate integers from 5 to 10 and also from 8 to 21?  


2. Write a function that converts Celsius to Fahrenheit:

~~~
      {your code here}0
32
      {your code here}100
212
~~~

Give this function a name:

~~~
     C2F←{your code here}
~~~

What kind of function is `C2F`? Is it a scalar function? Or a structural function? How would
you apply it to a vector or matrix of Fahrenheit values? 

3. Using the above two functions `to` and `C2F`, write a new function that converts a range of
integer Celsius values from `⍺` to `⍵` to the corresponding Fahrenheit values:    

~~~
      C2FRange←{your function here}
      20 C2FRange 25
68 69.8 71.6 73.4 75.2 77
~~~

4. Write a function that that takes the same arguments as `C2FRange` above,
but produces a matrix of Celsius and Fahrenheit values:

~~~
      C2FRangeMat←{your function here}
      20 C2FRangeMat 25
20 68  
21 69.8
22 71.6
23 73.4
24 75.2
25 77  
~~~

5. Why is Fahrenheit a better temperature scale than Celsius?


## More Exercises

Create three variables as follows:

~~~
       First←'Edgar'
       Middle←'Allan'
       Last←'Poe'
~~~

In each of the challenges below, write an expression using
the three variables to produce the requested result.
The results are displayed using the `]display` user command,
so make sure you use that to check your answer.

> Remember that when the instructions are to write an "expression"
> you do not need to use a function with `{...}` and `⍺` and `⍵` (although you may).
> You may just operate on the variable names themselves.

1. Write an expression using the three variables that yields a simple vector of the 
full name separated by spaces. The result should `]display` 
as follows:

~~~
┌→──────────────┐
│Edgar Allan Poe│
└───────────────┘ 
~~~

2. Write an expression that returns a matrix with the three
names shown vertically, in columns rather than rows:

~~~
┌→──┐
↓EAP│
│dlo│
│gle│
│aa │
│rn │
└───┘  
~~~

3. Write an expression that returns a matrix of names backwards
right aligned:

~~~
┌→────┐
↓ragdE│
│nallA│
│  eoP│
└─────┘
~~~

4. Write an expression that returns a matrix of names right
aligned (Hint: Sometimes we want to operate on each item 
of an array with a structural function}:

~~~
┌→────┐
↓Edgar│
│Allan│
│  Poe│
└─────┘
~~~

5. Write an expression that produces:

~~~
┌→──────────────────────────┐
│ ┌→──┐ ┌→────────────────┐ │
│ │Poe│ │ ┌→────┐ ┌→────┐ │ │
│ └───┘ │ │Edgar│ │Allan│ │ │
│       │ └─────┘ └─────┘ │ │
│       └∊────────────────┘ │
└∊──────────────────────────┘ 
~~~

6. Write an expression that produces:

~~~
┌→──────────────┐
│ ┌→──┐ ┌→────┐ │
│ │Poe│ ↓Edgar│ │
│ └───┘ │Allan│ │
│       └─────┘ │
└∊──────────────┘
~~~

7. Write an expression that produces:

~~~
┌→────────────────────┐
│ ┌→──┐ ┌→──────────┐ │
│ │Poe│ │Edgar-Allan│ │
│ └───┘ └───────────┘ │
└∊────────────────────┘
~~~







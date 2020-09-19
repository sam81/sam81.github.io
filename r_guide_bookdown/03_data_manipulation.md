# Data types and data manipulation {#datamanip}



## Vectors

One of the simplest and among the most important data types in R is the vector, which can be numerical or containing strings of characters. A simple way to build a vector is through the `c` function, which concatenates a series of data values, for example:

```r
temperature = c(34, 45, 23, 29, 26, 31, 44, 32, 19, 22, 34)
```
in this case `c` concatenates a series of numerical data into a vector and the assignment operator `=` assigns it to the variable "temperature", so that it can be retrieved later. Once the variable is created you can apply functions to it, for example

```r
mean(temperature)
```

```
## [1] 30.81818
```

will compute the mean of the data vector. If you want to save the result of this function, you just have to assign it to another object:

```r
mean_temp = mean(temperature)
```

note that in this case the value is assigned to the object `mean_temp` but it is not printed, you can display it with

```r
print(mean_temp)
```

```
## [1] 30.81818
```

or for short, just calling the object

```r
mean_temp
```

```
## [1] 30.81818
```

You can also perform simple arithmetic operations on a vector, for example:

```r
temperature + 10
```

```
##  [1] 44 55 33 39 36 41 54 42 29 32 44
```

will add 10 to each element of the vector.

You can also build vectors of characters, quoting each element of the vector

```r
color = c("blue", "green", "red" )
```

The `length` function is used to access the number of element present in a vector

```r
length(color)
```

```
## [1] 3
```


## Indexing vectors

It is possible to access only subsets of data in a vector and also assign them to another vector. The most basic form of indexing is based on the position of the data in the vector. For example, to access only the datum in the third position of a vector called temperature, you would simply type:

```r
temperature[3]
```

```
## [1] 23
```
if you would like to access the data in more than one position of the vector, let's say the first, the third and the sixth, you can again use the function concatenate inside the indexing command:

```r
temperature[c(1, 3, 6)]
```

```
## [1] 34 23 31
```
to access the data from, say, the third position to the tenth position you can use:

```r
temperature[3:10]
```

```
## [1] 23 29 26 31 44 32 19 22
```
and if you want to assign this subset to another vector called `"white"`, you can just type:

```r
white = temperature[3:10]
```
if you want to access all the vector but the first five positions:

```r
temperature[-(1:5)]
```

```
## [1] 31 44 32 19 22 34
```
since in R there is not a "delete" command, you can use this form of subsetting to remove elements of a vector, for example, if you would like to cancel the fourth element of the `temperature` vector you would write:

```r
temperature = temperature[-4]
```

Furthermore, to access subsets of data you can do much more magic using logical operators (see Table \@ref(tab:logicalops)) and other tricks, for example if you want to access in a vector only the data greater than a certain value, you can use the `>` (greater than) logical operator:

```r
temperature[temperature>30]
```

```
## [1] 34 45 31 44 32 34
```
In order to concatenate logical commands, you can use the & (and) logical operator:

```r
temperature[temperature>30 & temperature<35]
```

```
## [1] 34 31 32 34
```

Operator     Description
--------     -----------------------
`&`          Intersection ("and")
`&&`         "and" (lazy evaluation)
`|`          Union ("or")
`||`         "or" (lazy evaluation)
`!`          Negation
`xor`        Exclusive "or"
`isTRUE(x)`

Table: (\#tab:logicalops) Logical Operators.


Operator    Description
--------    -----------------------
`<`         Less than
`<=`        Less than or equal to
`>`         Greater than
`>=`        Greater than or equal to
`==`        Equal to
`!=`        Not equal to

Table: (\#tab:relationalops) Relational Operators.


It is also possible to apply labels to the positions of a vector, and then access the datum in a given position through its label:

```r
temperature = c(34, 45, 23, 29, 26)
names(temperature) = c("Johnny", "Jack", "Tony", "Pippo", "Linda")
temperature["Tony"]
```

```
## Tony 
##   23
```

### The `seq` function

The `seq` function can be used to create evenly spaced sequences of numbers

```r
seq(1,10)
```

```
##  [1]  1  2  3  4  5  6  7  8  9 10
```
the default increment is 1, but you can change it with the option `by`:

```r
seq(1, 1.9, by=0.1)
```

```
##  [1] 1.0 1.1 1.2 1.3 1.4 1.5 1.6 1.7 1.8 1.9
```

There's a shortcut for sequences with an increment of 1

```r
a = 1:10
a
```

```
##  [1]  1  2  3  4  5  6  7  8  9 10
```

### The `rep` function
You can use the `rep` function to create vectors which contain repetitions of the same elements. Let's start from the most simple use:

```r
vector1 = rep(3, 13)
```
simply creates a vector of 13 elements, all having the value 3. More interestingly, you can repeat sequences of numbers:

```r
rep(1:4, 3)
```

```
##  [1] 1 2 3 4 1 2 3 4 1 2 3 4
```
as you see the above command repeats the sequence 1,2,3,4 three times. Furthermore, you can also specify the number of times a given element of the sequence should be repeated:

```r
rep(1:4, 3, each=2)
```

```
##  [1] 1 1 2 2 3 3 4 4 1 1 2 2 3 3 4 4 1 1 2 2 3 3 4 4
```

There are other ways to achieve this same effect, for example:

```r
rep(rep(1:4, c(2, 2, 2, 2)), 3)
```

```
##  [1] 1 1 2 2 3 3 4 4 1 1 2 2 3 3 4 4 1 1 2 2 3 3 4 4
```
would yield the same effect.

Even if it can look pretty useless at first, the `rep` function comes in very handy for example, when you want to transform the data in a table from "one row per participant", to "one row per observation", which is necessary for example to run a repeated measures ANOVA with the `aov` function. `rep` makes it all easier as you can create vectors in which the occurrence of the levels of a factor are repeated over and over.


## Matrix facilities

There are different ways for creating a matrix in R, you often start from a vector, and then transform it into a matrix with the `matrix` function:

```r
matr = c(3, 5, 6, 2, 5, 7, 9, 1, 5, 4, 2, 3)
matr = matrix(matr, ncol=3, byrow=TRUE)
matr
```

```
##      [,1] [,2] [,3]
## [1,]    3    5    6
## [2,]    2    5    7
## [3,]    9    1    5
## [4,]    4    2    3
```
you give to the `matrix` function either the `ncol` or the `nrow` parameters to specify the layout of the matrix. The default method that R uses to fill in the matrix is by columns, so if you want to fill it by rows, you need to set true the option `byrow`, as in the example above.

Matrix indexing is similar to vector indexing:

```r
matr[2,1]
```

```
## [1] 2
```

the first index refers to the row number, and the second index to the column number. Omitting one of the two indexes is useful for slicing, for example

```r
matr[,2]
```

```
## [1] 5 5 1 2
```
gives *all the rows* in the second column. This could alternatively been written as

```r
matr[1:4,2]
```

```
## [1] 5 5 1 2
```
where the index is a \emph{range} of rows. This notation is useful when you want to extract only part of the rows or columns, for example

```r
matr[2:4,2]
```

```
## [1] 5 1 2
```
When the rows or columns to be extracted are not consecutive, you can use a *vector* of indexes for slicing

```r
matr[c(2,4),2]
```

```
## [1] 5 2
```
To query the dimension of the matrix you can use the `dim` function, in this case our matrix has 4 rows and 3 columns:

```r
dim(matr)
```

```
## [1] 4 3
```

The rows and columns of a matrix can also be assigned a name attribute through the `dimnames` function. The dimnames have to be a list of character vectors the same length as the matrix dimensions they refer to:

```r
dimnames(matr) = list(c("row1", "row2", "row3", "row4"), c("col1", "col2", "col3"))
dimnames(matr) #dimnames is a list
```

```
## [[1]]
## [1] "row1" "row2" "row3" "row4"
## 
## [[2]]
## [1] "col1" "col2" "col3"
```

```r
matr #now the matrix is printed with its dimnames
```

```
##      col1 col2 col3
## row1    3    5    6
## row2    2    5    7
## row3    9    1    5
## row4    4    2    3
```
the names of the rows can be retrieved with \verb+rownames+ and the names of the columns with \verb+colnames+:

```r
rownames(matr)
```

```
## [1] "row1" "row2" "row3" "row4"
```

```r
colnames(matr)
```

```
## [1] "col1" "col2" "col3"
```
and these names can be indirectly used for sub-setting;

```r
matr[1, which(colnames(matr) == 'col2')]
```

```
## [1] 5
```

### Matrix operations

The function `t` gives the transpose of a matrix. The inverse of a
matrix is obtained through the function `solve`. Some other
operators are listed in Table \@ref(tab:matrop)). Example:

```r
beta = solve(t(x)%*%x)%*%(t(x)%*%y)
```

Operator    Function
--------    -----------------------
`%*%`       Matrix multiplication
`det`       Determinant
`solve`    Inverse

Table: (\#tab:matrop) Matrix operations.

## Lists {#Lists}

Lists are objects that can contain elements of different modes (e.g numeric, character, logical), as well as other objects (vectors, matrices and also other lists). Let's build a small list to see how we can work on it:

```r
vec1 = 1:12
vec2 = c('w','h','m')
mylist = list(vec1, vec2)
mylist
```

```
## [[1]]
##  [1]  1  2  3  4  5  6  7  8  9 10 11 12
## 
## [[2]]
## [1] "w" "h" "m"
```

The syntax for subsetting a list is a bit awkward (but as we'll see later, naming the elements of a list makes things easier). To access an element of a list you can use the double brackets notation, for example

```r
mylist[[1]]
```

```
##  [1]  1  2  3  4  5  6  7  8  9 10 11 12
```
returns the first element of the list `mylist`, which is a vector of length 12, if you want to access, say, the third element of this vector, the syntax is as follows

```r
mylist[[1]][3]
```

```
## [1] 3
```
Naming the elements of the list makes things easier

```r
mylist=list(a=vec1, b=vec2)
mylist
```

```
## $a
##  [1]  1  2  3  4  5  6  7  8  9 10 11 12
## 
## $b
## [1] "w" "h" "m"
```
now the first element of the list is named `a`, and the second `b`, and we can access them with a special "dollar sign" notation

```r
mylist$a ## return element of the list named a
```

```
##  [1]  1  2  3  4  5  6  7  8  9 10 11 12
```

```r
mylist$a[1:3]
```

```
## [1] 1 2 3
```
It is also possible to use the double brackets notation with names

```r
mylist[["a"]][3]
```

```
## [1] 3
```

To eliminate an element of a list, set it to `NULL`

```r
vec1 = c(1, 2, 3)
vec2 = c("a", "b", "c")
myList = list(vec1=vec1, vec2=vec2)
myList
```

```
## $vec1
## [1] 1 2 3
## 
## $vec2
## [1] "a" "b" "c"
```

```r
myList$vec1 = NULL
myList
```

```
## $vec2
## [1] "a" "b" "c"
```
note that this is different from eliminating an element of the vectors contained in the list, you can do the latter with

```r
myList$vec2 = myList$vec2[-1]
myList
```

```
## $vec2
## [1] "b" "c"
```

## Dataframes {#dataframes}

Dataframes are one of the most important objects in R. You can think of it as a rectangular data structure, in which each column stores either the values of a numeric variable, or the levels of a factor, and each row represents an observation. Let's look at an example, we'll build a dataframe from 3 vectors, the first vector stores a variable, number of beers drunk during a week for twelve young people, the second is a factor vector, that specifies for each person whether he/she is a university student or not, so it has two levels, the third is also a factor vector, which tells the sex of each person, so it has two levels as well.

```r
n_beers = c(6, 8, 4, 8, 9, 4, 5, 3, 4, 2, 3, 1)
occupation = rep(c("s", "w"), 6)
sex = c(rep("m", 6), rep("f", 6))
occupation = as.factor(occupation)
sex = as.factor(sex)
```

well, now let's create the dataframe:

```r
dats = data.frame(n_beers, occupation, sex)
```

it's as simple as this, you have just put the three vectors together, let's have a look at it

```r
dats
```

```
##    n_beers occupation sex
## 1        6          s   m
## 2        8          w   m
## 3        4          s   m
## 4        8          w   m
## 5        9          s   m
## 6        4          w   m
## 7        5          s   f
## 8        3          w   f
## 9        4          s   f
## 10       2          w   f
## 11       3          s   f
## 12       1          w   f
```

as we said, each row holds the data of a single observation, in this case it corresponds to the data of a participant, but as we'll see later, this is not always necessarily true. Each row gives a full specification for each observation, we know that the first participant drunk 6 beers, he's a student, and he's male, and we could tell the same data for the other participants. Since we have all this information, we could now compare for example the number of beers drunk by male vs females, or by male students vs female students. There are special functions to compute these values quickly like `tapply` and `by` (see \@ref(tapply)), and other functions to get statistical tests, they will be dealt with as we go along.

### Accessing parts of a  dataframe

You can access, or refer to a column of a dataframe with the `$` operator, in the example above suppose we removed all the original variables after creating the dataframe

```r
rm(n_beers, occupation, sex)
```

we can't now access them directly by name

```r
mean(n_beers) #this will throw an object 'n_beers' not found error
```

we have to retrieve them from the dataframe

```r
mean(dats$n_beers)
```

```
## [1] 4.75
```

the example might seem artificial (why did I remove them in the first place?), but very often you read in the data directly as a dataframe with the `read.table` function (see sec.~\ref{read.table}, so you'll have to access them from the dataframe. Another option is to use the function `attach`, which attaches the dataframe to the path that R searches when evaluating a variable, in this way you don't have to refer to the dataframe to access the values of a variable

```r
attach(dats)
mean(n_beers)
```

```
## [1] 4.75
```

this is OK only if you're working with a single dataframe, and you don't want to manipulate the variables in it. In fact if you accidentally attach two dataframes that share some variable names, or you try to change an object of a dataframe after it has been attached, strange things may happen, you've been warned, the details are in the R manual. The function `detach` detaches the dataframe from the search path.

### Changing the names of variables in a dataframe

Sometimes you might want to change the names of the variables in a dataframe, for example when you create new dataframes with the `unstack` function, or just because you don't like the way you called it initially. You can visualise the names for the variables with the function `names`

```r
names(dats)
```

```
## [1] "n_beers"    "occupation" "sex"
```

or if you want to see just the first one:

```r
names(dats)[1]
```

```
## [1] "n_beers"
```

you can change it with a simple assignment:

```r
names(dats)[1] = "beers"
```

or if you want to change more than one:

```r
names(dats) = c("brs", "occ", "sx")
```

### Other ways to subset a dataframe

A dataframe actually is just a special kind of list (a list of class dataframe), so we can use the normal list notation to subset dataframes

```r
dats[[1]]
```

```
##  [1] 6 8 4 8 9 4 5 3 4 2 3 1
```

Sometimes it's useful to think of a dataframe as a matrix, and use matrix notation for subsetting

```r
dats[1,] ## extract first row, all columns
```

```
##   brs occ sx
## 1   6   s  m
```

```r
dats[which(dats$n_beers>4),]  ## records for subjs who drink > 4 beers
```

```
## [1] brs occ sx 
## <0 rows> (or 0-length row.names)
```

## Factors {#factors}

Data Vectors can be made not only of numerical values or of strings, but also *factors*. Factor vectors are very similar to character vectors, and could be seen as character vectors with some special properties. A factor vector usually consists of two or more levels, and can be created with the `factor` function. For example, suppose we are studying the drinking habits of 6 individuals, and we have measured their alcohol consumption (in alcohol units) during a week:


```r
alcoholUnits = c(7, 2, 15, 10, 1, 4)
```

the first three individuals are males, and the last three females and we can encode this information using a `factor` vector:


```r
sex = factor(c("m", "m", "m", "f", "f", "f"))
sex
```

```
## [1] m m m f f f
## Levels: f m
```

as you can see a factor has a `levels` attribute that specifies the possible values the factor can assume, and by default it is given by the unique values the factor vector can assume, sorted in alphabetical order.

One important side effect of the `levels` attribute is that the way factor levels are ordered determines the sorting order of statistical summaries and graphics. For example, if we use the `tapply` function to calculate the average alcohol units consumption by sex:


```r
tapply(alcoholUnits, list(sex), mean)
```

```
## f m 
## 5 8
```

the results for females are shown before the results for males. If we want the results for males to be presented before the results for females we can specify this ordering when creating the factor:


```r
sex = factor(c("m", "m", "m", "f", "f", "f"), levels=c("m", "f"))
tapply(alcoholUnits, list(sex), mean)
```

```
## m f 
## 8 5
```

ordering the levels of a factor for display purposes should not be confused with the concept of an `ordered` factor.

### Renaming the levels of a factor

To rename the levels of a factor we can use the `labels` argument to the `factor` function. Suppose that we have a sex factor coded as 'f' for females and 'm' for males:


```r
sex = factor(c("m", "m", "m", "f", "f", "f"))
```

we can change the coding to 'Male' and 'Female' as follows:


```r
sex = factor(sex, levels=c("f", "m"), labels=c("Female", "Male"))
sex
```

```
## [1] Male   Male   Male   Female Female Female
## Levels: Female Male
```

alternatively we can also use the `levels` function:

```r
sex = factor(c("m", "m", "m", "f", "f", "f"))
```

currently the levels are `c('f', 'm')`:

```r
levels(sex) #this gets the current levels
```

```
## [1] "f" "m"
```

we can change them with:


```r
levels(sex) = c("Female", "Male") #this sets the levels
```

we can also change just the name of one of the levels if we want to:


```r
sex = factor(c("m", "m", "m", "f", "f", "f")) #original factor
levels(sex)[which(levels(sex) == "m")] = "Male"
sex
```

```
## [1] Male Male Male f    f    f   
## Levels: f Male
```

### Creating factors with `gl` {#gl}

A handy function for creating factors for data with a regular pattern of factor levels is `gl`:

```r
sex = gl(2, 3, label=c("male", "female"), length=6)
sex
```

```
## [1] male   male   male   female female female
## Levels: male female
```
the first argument to the function specifies the number of levels, and the second argument the number of consecutive repetitions of each level, the pattern is repeated up to the number of elements specified by the length argument. Note the different pattern created when the number of consecutive repetitions is set to 1 and the total length is left unchanged:

```r
sex = gl(2, 1, label=c("male", "female"), length=6)
sex
```

```
## [1] male   female male   female male   female
## Levels: male female
```

### Natural sort order for character and factor vectors

It's common to assign identifiers to participants in an experiment as:

```r
ids = c("P1", "P2", "P3", "P4", "P5", "P6","P7", "P8", "P9", "P10", "P11")
```

when you use these identifiers for summarizing or plotting data as a function of participant id R will sort the output in strict alphabetical order, which is equivalent to the output of this function:

```r
sort(ids)
```

```
##  [1] "P1"  "P10" "P11" "P2"  "P3"  "P4"  "P5"  "P6"  "P7"  "P8"  "P9"
```

often what you want instead is a natural sort order, in which `P10` comes after `P9` and not after `P1`. To force R to use a natural sort order you can use a factor vector and sort the factor levels using the `mixedsort` function in the package `gtools`:

```r
library(gtools)
fids = factor(ids, levels=mixedsort(levels(ids)))
sort(fids)
```

```
## factor(0)
## Levels:
```

## Getting info on R objects {#objinfo}

The most useful function to summarise information about many R object is `str`:

```r
a = seq(0, 10, .1)
str(a)
```

```
##  num [1:101] 0 0.1 0.2 0.3 0.4 0.5 0.6 0.7 0.8 0.9 ...
```

```r
b = c("a", "b", "c")
str(b)
```

```
##  chr [1:3] "a" "b" "c"
```
in the example shown `str` gives information on the storage mode of the two vectors, numeric for `a` and character for `b`, it also gives information on the number of elements present and on the shape of the array (compare the output of `str` for a vector and a matrix). `str` gives also useful compact summaries of the contents of lists, including nested lists.

Another useful function is `mode`, which gives the storage mode of an R object:

```r
mode(a)
```

```
## [1] "numeric"
```

```r
mode(b)
```

```
## [1] "character"
```

## Changing the format of your data

In general statistical software require your data to be entered in a specific format in order to perform statistical analyses on them, and R is no exception. R provides many powerful functions to change the format of your data if they happen to be in a format that is not suitable for applying a given statistical function on them. The process of changing the format of your data with these functions might seem very complicated at first, however you should keep in mind the following things:

- You don't really need to learn all of the functions that R provides to manipulate your data and change their format. Once you learn a procedure that does the job you can stick with it and you'll be fine most of the times.

- Once you understand the "logic" and the structure of the data format that R expects to apply some statistical functions, changing the layout of your data to match this structure will be easy. Moreover, the data format that R wants is most of the times one and only one: The "one row per observation format", which will be explained below.

- In this tutorial you will see different examples in which the format of the data, stored in a given file don't match the format R wants. This is just for illustrative purposes. In real life if you're doing a research or an experiment, you  can often gather your data in a format that is already suitable for performing statistical analyses.

- You don't really have to use R to change the layout of your data if you don't like the functions it provides to do this job. You can always use some external programs to achieve the same results, for example spreadsheets programs such as Libreoffice Calc. What's really important is that you understand the structure of the format the R expects.

This said, I would advice you to learn some of the functions that R provides to rearrange your data for at least two reasons: 1) They're very powerful and can actually save you time once you learn them, and 2) many examples in this tutorial and in other books use them, so you'll often need to know them to understand what's going on :)

### The "one row per observation" (long) format {#onerowperobservation}

While statistic textbooks and scientific articles often show data in a "wide" format that is suitable and immediate for the "human eye", like the one shown in Table \@ref(tab:formath), statistical software often don't quite like it and would rather have the same data rearranged in a "long" format, as shown in Table \@ref(tab:formatss).

Group A  Group B  Group C
-------  -------- -------
5         4       7
2         4       5
3         5       7
4         5       8
6         4       7

Table: (\#tab:formath) Data format suitable for the "human eye".

Value  Group
-----  -----
5      A
2      A
3      A
4      A
6      A
4      B
4      B
5      B
5      B
4      B
7      C
5      C
7      C
8      C
7      C

Table: (\#tab:formatss) Data format suitable for R.

The main difference is that while in the first format you have more than one observation in the same row, and you can identify the group to which each observation belongs to through the column headers ("Group A", "Group B" and "Group C"), in the second format you have only one observation for each row. This observation is then fully identified with a "label" that appears in the second column. A better way to describe the second column is to say that in the above case "Group" is a **factor**, and "A", "B" and "C" define the **levels** of this factor for each group.

You could also be running an experiment in which you manipulate more than one factor. For example you might have two groups, "Patients" and "Controls", which are tested under two conditions: condition "1" and condition "2". In this case you might display your data as shown in Table \@ref(tab:formath2) to make them easily readable to humans. However for analyzing your data with R you would need to reshape the data as shown in Table \@ref(tab:formatss2), with one column specifying the group, and another column specifying the condition for a given observation.

Patients Condition1  Patients Condition 2  Controls Condition 1  Controls Condition 2
-------------------  --------------------  --------------------  --------------------
7                    6                     6                     4
5	                 4                     5                     2 
8	                 7                     7	                 4 
8	                 8                     6	                 5 
6	                 5                     5	                 3

Table: (\#tab:formath2) Data format suitable for the "human eye" with more than one factor.


Value  Group  Condition
-----  -----  ---------
7      P      1
5      P      1
8      P      1
8      P      1
6      P      1
6      P      2
4      P      2
7      P      2
8      P      2
5      P      2
6      C      1
5      C      1
7      C      1
6      C      1
5      C      1
4      C      2
2      C      2
4      C      2
5      C      2 
3      C      2

Table: (\#tab:formatss2) Data format suitable for R with more than one factor.

Finally, you might be running an experiment with a repeated measures design, in which all participants are exposed to all the levels of the within subjects factors. For example you might have your participants recall word lists either under the effects of a drug or not (factor 1) and with words concrete or abstract words (factor 2). In this case the data for presentation might look like the ones in Table \@ref(tab:repeatedh), in which each row represents a single participant. Again for R you need to rearrange the data so that each row represents a single observation, and in the case of a repeated measures design you need to add another column that identifies the levels of the "participants" factor as shown in Table \@ref(tab:repeatedr)

Drug Concrete  Drug Abstract  No-Drug Concrete  No-Drug Abstract
-------------  -------------  ----------------  ----------------
7              6              6                 4 
5              4              5                 2 
8              7              7	                4 
8              8              6	                5 
6              5              5	                3 

Table: (\#tab:repeatedh) Data format suitable for the "human eye" in a repeated measures design.

Value  Drug Exposure  Word Type  Participant
-----  -------------  ---------  -------
7      D              C          1
5      D              C          2
8      D              C          3
8      D              C          4
6      D              C          5
6      D              A          1
4      D              A          2
7      D              A          3
8      D              A          4
5      D              A          5
6      N              C          1
5      N              C          2
7      N              C          3
6      N              C          4
5      N              C          5
4      N              A          1
2      N              A          2
4      N              A          3
5      N              A          4
3      N              A          5

Table: (\#tab:repeatedr) Data Format Suitable for R, repeated measures design.

### The `stack` and `unstack` functions {#stack}

One of the utilities provided by R to manipulate the format of your
data is the `stack` function. If you have your data in a **dataframe**
with a layout similar to that shown in Table \@ref(tab:formath), you
can use the `stack` function to get a "one row per observation" format. What the `stack` function does is to create a single long vector from the vectors you have in your dataframe, and an additional factor vector which identifies the level for each observation. Here's an example, the data are in the file `stack.txt`:


```r
dats = read.table("stack.txt", header=TRUE)
```
this creates the dataframe

```r
dats = stack(dats)
```
this reshapes the dataframe into a "one row per observation" form.

Please note that R assigns names to the vectors in the new dataframe, you can see them in the header of the dataframe, you might need to know them for successive operations.

The `unstack` function simply does the opposite of the `stack` function, and you can use it if you want to switch back to your original dataframe format.

```r
dats = unstack(dats)
```

The `unstack` function can do more tricks, if you have a dataframe with one observation per row, a column with a response variable, and two or more factor columns, you can unstack the values of the response variable according to the levels of only one factors or according to the levels of two or more factors. Suppose `lat` is your response variable, and you have two factors, `congr` with 3 levels and `isi`, also with 3 levels. The command:

```r
dats = unstack(dats, form=lat~congr)
```
unstack the values of the response variable according to the levels of the `congr` factor, thus creating 3 columns, one for each level.

The command:

```r
dats = unstackd(ats,form=lat~congr:isi)
```

unstacks the values of the response variable according to the levels of both factors, thus creating $3x3= 9$ columns, the first column contains the values at level 1 of `congr` and level 1 of `isi`, the second one contains the values at level 1 of `congr` and level 2 of `isi`, and so on for all the possible combinations.

### The `tapply` and `aggregate` functions {#tapply}

The `tapply` function allows you to extract information from a dataframe, for example the mean or standard deviation of a given variable on the bases of one or more factor. The function name is related to the fact that it is used to *apply* a function (e.g. the mean) to a subsets of the dataframe chosen on the basis of one or more factors. We'll use the `InsectSprays` dataset to illustrate the use of `tapply`. The dataset contains the number of insects still alive in agricultural experimental units treated with six different types of pesticide.

```r
data(InsectSprays)
head(InsectSprays)
```

```
##   count spray
## 1    10     A
## 2     7     A
## 3    20     A
## 4    14     A
## 5    14     A
## 6    12     A
```

```r
meanSpray = tapply(X=InsectSprays$count,
                  INDEX=InsectSprays$spray,
                  FUN=mean)
meanSpray
```

```
##         A         B         C         D         E         F 
## 14.500000 15.333333  2.083333  4.916667  3.500000 16.666667
```

the arguments to `tapply` are `X`, the column of the dataframe to which the function should be applied, `INDEX`, the factor used for subsetting the dataframe, and `FUN`, the function to be applied. The function returns an array, in this case a vector, but can be a matrix, or multi-dimensional array.  The `INDEX` argument indeed can be a *list* of factors, in this case the function chosen is applied to group of values given by a unique combination of the levels of these factors. We'll see an example by modifying the `InsectSprays` dataset including another fictitious factor. The new factor will be the season in which the fields were sprayed. The values will be returned in a matrix.

```r
season = gl(4, 3, 72, labels=c("winter", "spring",
                                 "summer", "autumn"))
Ins = data.frame(InsectSprays, season)
meanSpray = tapply(X=Ins$count,
                   INDEX=list(Ins$spray, Ins$season),
                   FUN=mean)
meanSpray
```

```
##      winter    spring    summer    autumn
## A 12.333333 13.333333 16.666667 15.666667
## B 16.333333 13.666667 17.666667 13.666667
## C  2.666667  2.000000  2.000000  1.666667
## D  6.666667  4.333333  5.000000  3.666667
## E  3.666667  4.666667  1.666667  4.000000
## F 11.666667 17.666667 16.333333 21.000000
```

The `tapply` is often very useful, for example, after having calculated the means in this way, it is very easy to visualise the data with a barplot

```r
barplot(meanSpray, beside=TRUE, legend=T)
```

The `aggregate` function is very similar to tapply, but rather than returning an array, it returns a dataframe, which can be useful in some situations.

```r
InsDf = aggregate(x=Ins$count,
                  by=list(sprayType=Ins$spray, season=Ins$season),
                  FUN=mean)
InsDf
```

```
##    sprayType season         x
## 1          A winter 12.333333
## 2          B winter 16.333333
## 3          C winter  2.666667
## 4          D winter  6.666667
## 5          E winter  3.666667
## 6          F winter 11.666667
## 7          A spring 13.333333
## 8          B spring 13.666667
## 9          C spring  2.000000
## 10         D spring  4.333333
## 11         E spring  4.666667
## 12         F spring 17.666667
## 13         A summer 16.666667
## 14         B summer 17.666667
## 15         C summer  2.000000
## 16         D summer  5.000000
## 17         E summer  1.666667
## 18         F summer 16.333333
## 19         A autumn 15.666667
## 20         B autumn 13.666667
## 21         C autumn  1.666667
## 22         D autumn  3.666667
## 23         E autumn  4.000000
## 24         F autumn 21.000000
```

if you don't give a name to the grouping factors as we did with `SprayType=Ins$spray` a default name will be given. One slightly annoying thing is that, as far as I know, it is not possible to assign a name to the resulting variable (it will just be named `x`). However it can be changed afterwards, here's a solution that should work whatever the number of factors in the dataframe:

```r
names(InsDf)
```

```
## [1] "sprayType" "season"    "x"
```

```r
names(InsDf)[which(names(InsDf)=="x")] = "count"
names(InsDf)
```

```
## [1] "sprayType" "season"    "count"
```

## The `scale` function

The `scale` function can be used to easily transform your data into $z$ scores. Here's a trivial example:

```r
a = c(1, 2, 3)
scale(a)
```

```
##      [,1]
## [1,]   -1
## [2,]    0
## [3,]    1
## attr(,"scaled:center")
## [1] 2
## attr(,"scaled:scale")
## [1] 1
```

## Printing out data on the console

As you probably have already noted, after you've created a data object, just typing its name on the console and pressing `Enter` will display the values it contains. Sometimes however, you might want to see only part of the data, for example to do some checking, or because the data object is too big and it's not printed nicely on the console. The functions `head` and `tail` let you look only at the first or the last part of your data respectively. For example, let's load the `iris` dataset in the `datasets` package, the command:

```r
library(datasets)
head(iris)
```

```
##   Sepal.Length Sepal.Width Petal.Length Petal.Width Species
## 1          5.1         3.5          1.4         0.2  setosa
## 2          4.9         3.0          1.4         0.2  setosa
## 3          4.7         3.2          1.3         0.2  setosa
## 4          4.6         3.1          1.5         0.2  setosa
## 5          5.0         3.6          1.4         0.2  setosa
## 6          5.4         3.9          1.7         0.4  setosa
```
will print only the first 6 observations. You can visualise more (or less) than 6 observations by setting the `n` option:

```r
head(iris, n=10)
```

```
##    Sepal.Length Sepal.Width Petal.Length Petal.Width Species
## 1           5.1         3.5          1.4         0.2  setosa
## 2           4.9         3.0          1.4         0.2  setosa
## 3           4.7         3.2          1.3         0.2  setosa
## 4           4.6         3.1          1.5         0.2  setosa
## 5           5.0         3.6          1.4         0.2  setosa
## 6           5.4         3.9          1.7         0.4  setosa
## 7           4.6         3.4          1.4         0.3  setosa
## 8           5.0         3.4          1.5         0.2  setosa
## 9           4.4         2.9          1.4         0.2  setosa
## 10          4.9         3.1          1.5         0.1  setosa
```

using `tail` will show the last rows of the dataframe, and again you can instruct the function to show more or less rows by passing the `n` argument:

```r
tail(iris, n=2)
```

```
##     Sepal.Length Sepal.Width Petal.Length Petal.Width   Species
## 149          6.2         3.4          5.4         2.3 virginica
## 150          5.9         3.0          5.1         1.8 virginica
```

A data structure that is a modern take on dataframes, the `tibble`, will be described in Chapter \@ref(tidyverse). Tibble printouts by default only show the rows and columns that fit on the screen (a default which I find annoying). See Section \@ref(tibbles) for instructions on how to override this default if you're using tibbles and don't like their default print behavior.

### Reading numbers in exponential notation

Often R prints out numbers in exponential notation. In order to understand exponential notation it's first necessary to introduce *scientific notation*.

 A number in scientific notation is in the form $a\cdot 10^b$, for example 300 could be written in scientific notation as $3\cdot 10^2$. The components of a number in scientific notation are also named as $mantissa \cdot 10^{characteristic}$. Remember that a number with a negative exponent, for example $10^{-2}$ can be rewritten as
\[10^{-2}= \frac{1}{10^2}=0.01\]
so, for example, $0.003$ can be rewritten in scientific notation as $3\cdot 10^{-3}$, because
\[3\cdot 10^{-3}= 3\cdot \frac{1}{10^3}= 0.003\]

R, as most calculators doesn't actually use scientific notation, it uses instead *exponential* notation. The exponential notation is a shorthand version of the scientific notation, in which, for example, $10^3$ is replaced by $e3$, where $e$ stands for *exponent*. So in our previous examples 300 would be written as $3e2$ and 0.003 would be written as $3e3$. Below are a few conversion examples.

Number    Scientific Notation  Exponential Notation
------    -------------------  --------------------
$10$      $1\cdot 10^1$        $1e1$
$20$      $2\cdot 10^1$        $2e1$
$200$     $2\cdot 10^2$        $2e2$
$350$     $3.5\cdot 10^2$      $3.5e2$
$0.00353$ $3.53\cdot 10^-3$    $3.53e-3$

Table: (\#tab:numberformat) Number Formats.

As a quick and dirty rule, remember that when you're multiplying a number by $10^{exponent}$, as you add to the exponent, you're adding a 0 to the number, or shifting the point by one position towards the right if it's a decimal number. As you subtract to the exponent, you're deleting a 0 from the number, or shifting the point by one position towards the left.

## Creating and editing data objects through a visual interface

If you want to use a visual interface for creating a dataframe, first create an empty dataframe with:

```r
mydataframe = data.frame()
```

then you can call a spreadsheet like editor to fill in the dataframe with:

```r
data.entry(mydataframe)
```

or

```r
fix(mydataframe)
```

If the data object is a vector, `fix` will call a text editor to edit the object instead of the spreadsheet like interface, so if you want the latter, use the function `data.entry` instead. However, using a simple text editor for fixing a vector might be more practical, if you want to use a different text editor from the one that `fix` calls by default, you can change the `editor` option:

```r
fix(myvector, editor="emacs")
```

or just call the editor on the object:

```r
emacs(myvector)
```

On Windows you could try:

```r
fix(myvector, editor="notepad")
```


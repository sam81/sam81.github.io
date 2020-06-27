# A simple introduction to R

This chapter will give a simple introduction to R, just to get familiar with it and get a general idea of how it works. This chapter assumes no previous knowledge of programming. If you know other computer languages, or have even a basic knowledge of programming, getting started will be easy. If you don't, don't worry, R syntax is very elegant and simple, it might take a little while, but after looking at some examples, and importantly, trying them out yourself, you'll be up and running without problems. This tutorial deals only with learning to use R from the command line, if you'd rather use R with a GUI, that is, with a "point and click" interface, please have a look at Section \@ref(gui) for some information on how to get started.

## Datasets

This guide uses several datasets. If you want to follow the examples given in the guide you can download the datasets from this URL: https://sam81.github.io/r_guide_bookdown/rguide_datasets.zip

## Firing up and quitting R

Under GNU/Linux systems you can start R  from a shell, just type `R` and press <kbd>Enter</kbd>. Under Windows you can click on the R icon to start the R GUI. 

You can save yourself a lot of typing by using the up arrow key &#8593; to retrieve past commands.

Commands can be terminated either by a semi-colon `;` or by pressing <kbd>Enter</kbd> and starting a newline. If you start a newline before a command is complete, R will prompt you to complete the command with a `+` sign, you can then complete the command. If you don't know how to complete the command and get stuck, you can stop R prompting you with the plus sign by pressing the <kbd>CTRL</kbd> and <kbd>C</kbd> keys simultaneously.

To quit R type `quit()` or `q()`\footnote{On a Unix terminal you can also press `Ctrl-d` to exit}, R will ask you if you want to save the current session, if you answer `y`, R will save all the objects active in the current session and the command history.

## Starting to work with R

The first thing you can try, is doing some math, at the command prompt type `5+4` and press <kbd>Enter</kbd>, the result will be


```r
5 + 4
```

```
## [1] 9
```

well, obviously 9. Other arithmetic operators are listed in Table \@ref(tab:arithmetic)

Symbol Function
------ --------
`+`    Addition
`-`    Subtraction
`*`    Multiplication
`/`    Division
`^`    Exponentiation

Table: (\#tab:arithmetic) Arithmetic Operators. 

Now let's create a variable, we'll call it `foo`, and assign to it a number

```r
foo = 5
```

the equal sign `=` is the assignment operator in R\footnote{You can also use the arrow `<-`as an assignment operator}, in the above case it means the value of `foo` is 5, `foo` is an object.

<div class="rmdnote">
<p>In R both <code>=</code> and <code>&lt;-</code> can be used as assignment operators. For example, the expression <code>x = 5</code> and the expression <code>x &lt;- 5</code> are equivalent, and both are in common use, although some style guides recommend one over the other. In a few corner cases using <code>&lt;-</code> instead of <code>=</code> may lead to different behaviors (e.g. when trying to make assignments inside function calls; something that you generally shouldn’t do).</p>
<p>I tend to stick to using <code>=</code> because it requires only a single keystroke, looks more readable to me, and is the assignment operator used in almost all other programming languages.</p>
</div>

Because `foo` is an object of numeric type, we can perform arithmetic operations on it:


```r
foo * 2
```

```
## [1] 10
```

we can also store the results of an arithmetic operation in an object:

```r
another_foo = 5^2
```
if you want to display the value of this new object you can use

```r
print(another_foo)
```

```
## [1] 25
```
or, for short, just type its name and press <kbd>Enter</kbd>

```r
another_foo
```

```
## [1] 25
```
since the two objects we have created are both numeric we can also sum them:

```r
foo + another_foo
```

```
## [1] 30
```

Let's look at something more interesting, we can create an object that stores a series of numbers, for example the money we have spent each day of a week, in Euros; we can do this using the `c` function, which concatenates a series of values into a *vector*:

```r
expenses = c(7,8,15,20,9,45,3)
```
you might want to find out how much you've spent in average during the week, this is easily accomplished with the function `mean`

```r
mean(expenses)
```

```
## [1] 15.28571
```
the function `sd` gives you the standard deviation

```r
sd(expenses)
```

```
## [1] 14.24446
```

Surely you've wondered why `[1]` appears every time R gives you a result, now that we've introduced the vector we can get to it. Try to create a long vector, you can easily do this by creating a sequence of numbers, for example:

```r
long_vec = seq(1, 100, by=1)
```
will create a vector containing the sequence of numbers from 1 to 100, now try to display it and see what happens. All the elements of the vector won't fit in a single line of the screen, and at the start of each line you'll get between \verb+[ ]+ the index, that is the position, of the first element on that line. There's also a shorthand to create such a vector:

```r
long_vec = 1:100
```

R can also deal with string variables:


```r
name = "John"
msg1 = "said to check the supplies"
```

strings can be concatenated together with the `paste` function:


```r
paste(name, msg1)
```

```
## [1] "John said to check the supplies"
```

by default strings pasted together with the `paste` function are separated by a blank space, but this can be changed by supplying a `sep` argument to the function:


```r
paste(name, msg1, sep=";")
```

```
## [1] "John;said to check the supplies"
```

if we want the strings to be attached together with no separator at all, we can write `paste(name, msg1, sep="")`, but it is more convenient to use the `paste0` function instead:


```r
paste0(name, msg1)
```

```
## [1] "Johnsaid to check the supplies"
```

more string facilities are presented in Section \@ref(stringprocessing).

## Getting help {#gettinghelp}

R comes with an excellent online help facility which documents and gives examples for all available functions. There is also a web interface for the help system which is easier to use, you can start it with

```r
help.start()
```
this fires up a web browser from which you can access a search engine for all the available documentation. The documentation is also available as a `pdf` file, the ``Full Reference Manual'' which documents the base system. Printable `pdf` manuals are also available for all the other additional packages. 

### The online help system
You can quickly look up the documentation for a function, for example `sd`, with

```r
?sd
```
or

```r
help(sd)
```
it is often indifferent using quotes or not, but sometimes they are required, for example 

```r
?* ## doesn't work
Error: syntax error
?"*"  ## this works!
```
to quit the help screen press <kbd>Q</kbd>.

You can easily run the example code given in the help pages for a given function with

```r
example(function_name)
```
it is better to set the graphics parameter `ask` as `TRUE` before running the examples

```r
par(ask=TRUE)
```
to pause between successive plots, if there is more than one.

The online help system searches by default the available documentation for the base system and all the packages that are currently loaded. If you want to look the documentation for a function present in a package that is not loaded, you need to specify the package in question:

```r
help("levene.test", package=car)
```
if you know the function exists but don't know the package it is in, try

```r
help(levene.test, try.all.packages=TRUE)
Help for topic 'levene.test' is not in any loaded package but can be
found in the following packages:

Package               Library
car                   /usr/lib/R/site-library
```

The function `help.search` can be used when you don't exactly know the name of the function you're looking for

```r
help.search("levene")
Help files with alias or concept or title matching 'levene' using
fuzzy matching:


levene.test(car)        Levene's Test
...
```

## Working with a graphical user interface {#gui}

The default version of R for Windows and macOS comes with a very limited graphical user interface (GUI), while the GNU/Linux version comes with no GUI at all. There are however several independent projects aimed at developing a GUI for R. The following sections give some information on them.

### R Studio

R Studio is currently the most popular GUI for R. It can be installed on all major operating systems: <https://www.rstudio.com>.

### The R Commander

An extensive GUI for R is provided by the `Rcmdr` package ([R commander](https://www.rcommander.com/)). This GUI allows you to do many of the operations you can do using R from the command line, through a point and click visual interface. The R commander is just a R package, so in order to use it, you need to have R installed in the first place, then you have to install the `Rcmdr` package and all the other packages it depends on. After everything is installed correctly, fire up R and call the R commander as you do with any other package:

```r
library(Rcmdr)
```

you will be greeted by a GUI with menus that allow you to type in data, perform statistical analyses and create graphs. The R commander works on both GNU/Linux and Windows platforms. For further information, please refer to the R commander manual or look up the following web page: <http://www.rcommander.com/>.

### JGR

The JGR package provides a clean, simple graphical user interface for R, which is platform independent. The package is written in JAVA and requires the JAVA SDK to run. More info is available at the project webpage: <https://www.rforge.net/JGR/>






 

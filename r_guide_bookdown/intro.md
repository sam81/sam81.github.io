# Installing R {#intro}



The information provided in this section is quite generic and limited. For detailed information please refer to the "R Installation and Administration Manual" available at the CRAN website https://cran.r-project.org. CRAN stands for "Comprehensive R Archive Network", and is the main point of reference for the R software, where you can find R sources, binaries, documentation, and add-on packages.

## Installing the Base Program

First of all you need to install the base R program. There are two ways of doing this, you can either compile the source code yourself, or you can install the precompiled binaries for your specific operative system. The second way is the easiest one, and usually you will want to go with it.

### GNU/Linux and Other Unices

Precompiled binaries are available for some GNU/Linux distributions, there is a list of these on the R FAQ. For other distributions you can build R from source.
There are precompiled binaries for Debian GNU/Linux, so you can install the R base system and a number of add-on packages with the usual methods under Debian, that is, `apt-get`, `Synaptic` or whatever else you use.

### Windows

There are precompiled binaries for Windows, you just have to download them, then double click on the installer's icon to start the installation. Most Windows versions are supported.

### Mac Os X

Precompiled binaries are also available for Mac Os X, you can download them and then double click on the installer's icon to start the installation.


## Installing Add-On Packages

There is a vast number of packages that implement statistical functions that are not available with the base program. They're not strictly necessary, but if you keep using R, sooner or later you will want to install some of these packages.

### GNU/Linux and Other Unices

There are different ways to get packages installed. For Debian there are precompiled versions of some packages, so you can get them with `apt-get` or whatever else you usually use to install Debian packages, this will also take care of possible dependencies (some packages need other packages or system libraries to be installed in order to work). For other packages, from an R session you can call the `install.packages` function to install additional packages, for example:

```r
install.packages("gplots")
```
will install the `gplots` package. You can also install more than one package in one go:

```r
install.packages(c("gplots", "signal"))
```
will install the `gplots` and `signal` packages. Another way to install a package is to download the related tarball and then from a root console issue the command:

	R CMD INSTALL /packagename

where `packagename` is the full path to the tarball you have downloaded. There are other ways and other specific options to install add-on packages. You should refer to the "R Installation and Administration Manual" for further information. 

### Windows

The R's GUI on Windows provides an interface to download and install the packages directly from the internet. You can also install packages from a local repository, this is necessary for example if you don't have an internet connection from the computer you want to install the package on. In this case, you can download the package you want from another computer as a zip file, and then transfer it to your computer. At this point to install the package don't unzip it, just start the R GUI and click on etc...


# A Simple Introduction to R

This chapter will give a simple introduction to R, just to get familiar with it and get a general idea of how it works. This chapter assumes no previous knowledge of programming or anything similar. If you know other computer languages, or have even a basic knowledge of programming, getting started will be easy. If you don't, don't worry, R syntax is very elegant and simple, it might take a little while, but after looking at some examples, and importantly, trying them out yourself, you'll be up and running without problems. This tutorial deals only with learning to use R from the command line, if you'd rather use R with a GUI, that is, with a "point and click" interface, please have a look at Section \@ref(gui) for some information on how to get started.

## Datasets

This guide uses several datasets. If you want to follow the examples given in the guide you can download the datasets from this URL: https://sam81.github.io/r_guide_bookdown/rguide_datasets.zip

## Firing up and Quitting R

Under GNU/Linux systems you can start R  from a shell, just type `R` and press <kbd>Enter</kbd>. Under Windows you can click on the R icon to start the R GUI. 

You can save yourself a lot of typing by using the up arrow key &#8593; to retrieve past commands.

Commands can be terminated either by a semi-colon `;` or by pressing <kbd>Enter</kbd> and starting a newline. If you start a newline before a command is complete, R will prompt you to complete the command with a `+` sign, you can then complete the command. If you don't know how to complete the command and get stuck, you can stop R prompting you with the plus sign by pressing the <kbd>CTRL</kbd> and <kbd>C</kbd> keys simultaneously.

To quit R type `quit()` or `q()`\footnote{On a Unix terminal you can also press `Ctrl-d` to exit}, R will ask you if you want to save the current session, if you answer `y`, R will save all the objects active in the current session and the command history.


## Starting to Work with R

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

the equal sign `=` is the assignment operator in R\footnote{You can also use the arrow `<-`as an assignment operator}, in the above case it means the value of `foo` is 5, `foo` is an object, since it is of a numeric type, we can perform arithmetic operations on it:


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
since the two objects we have created are both numeric we can also do

```r
foo + another_foo
```

```
## [1] 30
```

Let's look at something more interesting, we can create an object that stores a series of numbers, for example the money we have spent each day of a week, in Euros, we can do this using the `c` function, which concatenates a series of values in a *vector*

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

Surely you've wondered why `[1]` appears every time R gives you a result, now that we've introduced the vector we can get to it. Try to create a long vector, you can easily do this by creating a sequence of numbers, for example

```r
long_vec = seq(1, 100, by=1)
```
will create a vector containing the sequence of numbers from 1 to 100, now try to display it and see what happens. All the elements of the vector won't fit in a single line of the screen, and at the start of each line you'll get between \verb+[ ]+ the index, that is the position, of the first element on that line. There's also a shorthand to create such a vector

```r
long_vec = 1:100
```


## Getting Help {#gettinghelp}


R comes with an excellent online help facility which documents and gives examples for all available functions. There is also a web interface for the help system which is easier to use, you can start it with

```r
help.start()
```
this fires up a web browser from which you can access a search engine for all the available documentation. The documentation is also available as a `pdf` file, the ``Full Reference Manual'' which documents the base system. Printable `pdf` manuals are also available for all the other additional packages. 

### The Online Help System
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



## Working with a Graphical User Interface {#gui}

The default version of R for Windows and macOS comes with a very limited graphical user interface (GUI), while the GNU/Linux version comes with no GUI at all. There are however several independent projects aimed at developing a GUI for R. The following sections give some information on them.

### R Studio

R Studio is currently the most popular GUI for R. It can be installed on all major operating systems: <https://www.rstudio.com>.

### The R Commander

An extensive GUI for R is provided by the `Rcmdr` package (R commander). This GUI allows you to do many of the operations you can do using R from the command line, through a point and click visual interface. The R commander is just a R package, so in order to use it, you need to have R installed in the first place, then you have to install the `Rcmdr` package and all the other packages it depends on. After everything is installed correctly, fire up R and call the R commander as you do with any other package:

```r
library(Rcmdr)
```
you will be greeted by a GUI with menus that allow you to type in data, perform statistical analyses and create graphs. The R commander works on both GNU/Linux and Windows platforms. For further information, please refer to the R commander manual or look up the following web page: <http://www.rcommander.com/>.

### JGR

The JGR package provides a clean, simple graphical user interface for R, which is platform independent. The package is written in JAVA and requires the JAVA SDK to run. More info is available at the project webpage: <https://www.rforge.net/JGR/>




# Organising a Working Session

## Setting and Changing the Working Directory

The command `getwd` displays the pathname of the current working directory, that is where R will look for and store files if not otherwise instructed.

To change the current working directory, use the command `setwd("dirname")`, where `dirname` is the pathname of the working directory you're moving into. Note that this has to be an existent working directory, because R cannot create a new directory with this command. Here's an example of how to specify the pathname:

```r
setwd("C:/mydata/rats")
```
note that you have to use a slash `"/"` and not backslash  `"\"` like you usually do in Windows to specify the pathname.
You can also specify a pathname relative to your current working directory, without specifying the full pathname. It is indifferent using single `' '` or double `" "` quotes, this holds true when you need to quote character strings.

If you want to see the files present in your current working directory, use the command `dir`.

It is also possible to issue commands to the OS from within R with the `system` function, for example

```r
system("ls")
```
under GNU/Linux or Unix systems, will list the files present in the current directory.

##Objects

All the variables, functions, arrays etc... you work with in R are stored and manipulated as *objects*. To list all the objects currently active in your *workspace*, you can use the command:

```r
objects()
```
or alternatively

```r
ls()
```

if you want to remove some of these objects from memory, you can use the command:

```r
rm(X, Y, W, foo, rats)
```
you can also give the variables to be removed, as a character vector:

```r
rm(list=c("X","Y","foo","rats"))
```
if you want to remove all the objects in your workspace, you can combine the `ls` and `rm` commands as follows:

```r
rm(list=ls())
```
this however doesn't remove objects whose name starts with a dot, to remove also those you can use:

```r
rm(list=ls(all=TRUE))
```

## Saving and Using the "Workspace Image"

You can use a ``workspace image'' that you have previously saved by starting R from the directory in which it was saved. In this way you can use the objects created in a previous session and the up arrow as well to retrieve commands from that session. To take full advantage from workspace images you'd better use different working directories for different analyses, studies, experiments and so on, in this way you can restore the workspace image of a specific analysis you were running and above all, you avoid accidentally overwriting objects from different analyses by creating another object with the same name during your current analysis.

When saving the workspace image R stores two files in the current working directory, one with the objects and one with the command history. This files begin with a dot under GNU/Linux and so are hidden.

You can save the workspace image either on exit, answering yes to the prompt you're given, or during a session with the `save.image` function, the latter is a good measure against accidental losses of objects due to a power failure.

## Working in Batch Mode

### Executing commands written in a file from an R session
 Instead of writing and executing commands line by line, it is often convenient to write the commands in a text file and then run them all at once in batch mode. You just write the commands with a text editor in a file, as if it were on the R console, save it in a directory, and then from within an R session issue the command:

```r
source("C:mydata/myfile.txt")
```
By default R displays only the results of the commands written in the source file, you can change this using the option:

```r
source("C:mydata/myfile.txt", echo=TRUE)
```

### Executing commands written in a file from a shell
It is also possible to execute R commands written in a file without starting an R session: From within your system's shell (for example bash on GNU/Linux or dos on Windows) issue the command
```
  $ R CMD BATCH myfile.R
```
where `myfile.R` is the file you've written the R commands in.


# R programming {#programming}



## Control structures {#contrstruct}


### Conditionals

#### If..else conditional execution {#conditionalexecution}

It is possible to insert and execute control structures directly from the R interpreter, but for the following examples, I'll assume you're writing the commands to a batch file, and then executing them through the `source()` command.

The general form of conditional execution in R is:

```r
  if (cond){
    do_this
   } else {
     do_something_else
   }
```
here's a trivial example:


```r
money = 1300
if (money > 1200){
    print("good!")
} else {
    print("troubles...")
}
```

```
## [1] "good!"
```

it's important that the `else` statement is on the same line where the previous command ends (in the above example that's the closing brace on the fourth line), otherwise the interpreter sees it as unrelated to the previous `if` and will give an error (the `if` statement could also be used by itself, so it would be seen as a complete statement if `else` does not appear on the same line).

It is also possible to execute more than one command upon the fulfilment of a given  condition:


```r
money = 900
expenses=1200
if (money > expenses){
    print("good!")
    shopping= money-expenses
} else {
    print("troubles...")
    shopping=NA
}
```

```
## [1] "troubles..."
```

```r
print("Money available for shopping:")
```

```
## [1] "Money available for shopping:"
```

```r
print(shopping)
```

```
## [1] NA
```
Finally it is possible to add branches to your control structure with the `else if` statement:

```r
expenses = 1000
laptop  = 1000
if ((money-expenses) > 1000){
    print("great!! buy new laptop")
    shopping=(money-expenses)-laptop
} else if ((money-expenses) > 0 && (money-expenses) <= 1000){
    print("no laptop, just shopping and save some")
    shopping= (money-expenses)/2
} else {
    print("troubles...")
    shopping=NA
}
```

```
## [1] "troubles..."
```

```r
print("Money available for shopping:")
```

```
## [1] "Money available for shopping:"
```

```r
print(shopping)
```

```
## [1] NA
```


#### `ifelse` {#ifelse}

The `ifelse` function is handy for testing all the elements of a vector on a given condition, the general form is:

```r
  ifelse(condition, value_if_cond_true, value_if_cond_false)
```
for example, let's say we want to categorise the results of a classroom test, scored from 1 to 10 as "pass" if the score was equal to, or greater than 6 and "fail" if the score was less than 6:


```r
score = c(4,7,6,5,8,6,7)
admission = ifelse(score >= 6, "pass", "fail")
admission
```

```
## [1] "fail" "pass" "pass" "fail" "pass" "pass" "pass"
```

so, the first argument of the `ifelse` function, is the condition that we want to test, the second argument is the value that should be returned if the condition is met, and the third argument is the value that should be returned if it is not.


#### `xor` {#xor}

The  function `xor` implements the exclusive logical "or" operator, that is, it evaluates to `TRUE` if exclusively one of two alternative conditions is met, otherwise, it evaluates to false. The latter occurs both, when none of the conditions is met and when both are met simultaneously.


```r
a=6
xor(a>5, a>7)
```

```
## [1] TRUE
```

```r
xor(a>5, a>3)
```

```
## [1] FALSE
```

in the first example, only the first condition (a>5) is met, so the function evaluates to true. In the second example, both conditions are satisfied, but since we're using `xor` and you can have one thing or the other, but not both together, the function evaluates to false.


```r
a
```

```
## [1] 6
```

```r
a=a[-which(a>5)]
```

### Loops

#### `for` loops

`for` loops can be used to iterate over elements of a vector:

```r
for (i in 1:4){
    print(i)
}
```

```
## [1] 1
## [1] 2
## [1] 3
## [1] 4
```

in the above case the vector is given by the sequence of numbers `1:4`. The following example uses a vector consisting of a sequence of letters:

```r
letts = c("a", "b", "c", "d")
for (ltt in letts){
    print(ltt)
}
```

```
## [1] "a"
## [1] "b"
## [1] "c"
## [1] "d"
```

`for` loops can iterate also over elements of a list:

```r
lst = list(a=c(1, 8), b=c(2, 7), c=c(3, -4))
for (itm in lst){
    print(itm[2])
}
```

```
## [1] 8
## [1] 7
## [1] -4
```

`for` loops can be nested at will:

```r
for (ltt in letts){
    for (i in 1:4){
        print(paste0(ltt, " - ", i))
    }
}
```

```
## [1] "a - 1"
## [1] "a - 2"
## [1] "a - 3"
## [1] "a - 4"
## [1] "b - 1"
## [1] "b - 2"
## [1] "b - 3"
## [1] "b - 4"
## [1] "c - 1"
## [1] "c - 2"
## [1] "c - 3"
## [1] "c - 4"
## [1] "d - 1"
## [1] "d - 2"
## [1] "d - 3"
## [1] "d - 4"
```

#### `while` loops

`while` loops continue until a certain condition is met:

```r
i = 1
while (i<4){
    print(i)
    i = i+1
}
```

```
## [1] 1
## [1] 2
## [1] 3
```

## Functions

Functions are one of the fundamental building blocks of programming. While using R we call functions all the time, but why write new ones? A major reason is to make your analysis code more modular, readable, compact, and reusable. Whenever you're repeatedly performing a series of operations on well-defined inputs and outputs in your analyses scripts you should consider "packaging" those operations into functions. The syntax for declaring a function is:

```r
my_func = function(arg1, arg2){
    ## function code here
}
```

where `my_func` is the name of the function, and `arg1`, `arg2` are the arguments we want to pass to the function. For example let's define a function that computes the [root mean square (RMS)](https://en.wikipedia.org/wiki/Root_mean_square) of a vector:

```r
RMS = function(vls){
    RMS_val = sqrt(mean(vls^2))

    return(RMS_val)
}
```

the last line indicates the value returned by the function, if no explicit `return` statement is provided, the function will return the value of the last evaluated expression. We can now run the function:

```r
RMS(c(-2, 5, -8, 9, -4))
```

```
## [1] 6.164414
```

Let's suppose now that we want our `RMS` function to handle the possibility of missing values in the input vector. If there are one or more `NA` the `RMS` function, as is defined right now, will return `NA`:

```r
RMS(c(-2, 5, -8, 9, NA))
```

```
## [1] NA
```

we want this to be the default behavior, but we want to give users the option to ignore the missing values and compute the RMS of the subset of remaining values. We can do this by defining the `RMS` function as follows:

```r
RMS = function(vls, na.rm=FALSE){
    if (na.rm == TRUE){
        vls = vls[is.na(vls) == FALSE]
    }
    RMS_val = sqrt(mean(vls^2))

    return(RMS_val)
}
```

running the function with the default value for `na.rm` still gives the same result as before:

```r
RMS(c(-2, 5, -8, 9, NA))
```

```
## [1] NA
```

specifying `na.rm = TRUE`, instead, computes the RMS of the non-missing values

```r
RMS(c(-2, 5, -8, 9, NA), na.rm=TRUE)
```

```
## [1] 6.595453
```


## String processing {#stringprocessing}

One of the strengths of R, in my opinion, lies in the way it deals with character strings. Certain objects, for example dataframes, allow to mix strings with other data types, subsets of certain objects (again dataframes are an example, but also lists), can be easily given meaningful names and retrieved. This adds much flexibility and ease of use to R compared to other languages (e.g. MATLAB). One aspect that is perhaps less known however, are the powerful string processing functions that R gives you. Once you get to know them you'll realise you can do all your data analysis in R, without the need to use other languages, like python or perl for pre-processing.

The simplest thing you can do with a string, is counting its characters, which you can do with the `nchar` function:


```r
my_string = 'I love R'
nchar(my_string)
```

```
## [1] 8
```

The second thing you can do with strings is extracting parts of them. There are various way to achieve this. Two of the most useful functions are `substr` and `strsplit`.

`substring`, as the name suggests, returns part of a string:


```r
substr(my_string, start=1, stop=4)
```

```
## [1] "I lo"
```

if you want to get a portion of a string from some point in the middle, to the end:


```r
substr(my_string, start=5, stop=nchar(my_string))
```

```
## [1] "ve R"
```

`substr` can be also used to replace parts of a string


```r
substr(my_string,start=1,stop=3)='qqq'
my_string
```

```
## [1] "qqqove R"
```

### Using regular expressions {#regexp}


```r
b=c('the','atheist','theme','therion','thin','jjthe')   
grep('^the',b,value=TRUE) ## match only when pattern appears at the beginning
```

```
## [1] "the"     "theme"   "therion"
```

```r
grep('the$',b,value=TRUE) ## match only when pattern appears at the end
```

```
## [1] "the"   "jjthe"
```

```r
grep('^the$',b,value=T) ## match exactly 'the' not followed or preceded by anything
```

```
## [1] "the"
```

```r
grep('the[i,m]',b,value=TRUE)## match 'the' followed by 'i' or 'm'
```

```
## [1] "atheist" "theme"
```

```r
grep('the[^i]',b,value=TRUE)## match 'the' followed by anything except 'i'   
```

```
## [1] "theme"   "therion"
```

```r
grep('the.',b,value=TRUE)## match 'the' followed by anything
```

```
## [1] "atheist" "theme"   "therion"
```

```r
grep('.the',b,value=TRUE)## match 'the' preceded by anything
```

```
## [1] "atheist" "jjthe"
```

`glob2rx` translates a wildcard pattern, as used in most shells (for example for listing files with the Unix `ls`), in a regular expression, so if you're used to wildcards this comes is handy

```r
glob2rx('the*')
```

```
## [1] "^the"
```

```r
glob2rx('the')
```

```
## [1] "^the$"
```

## Tips and tricks

### Convert a string into a command


```r
cmd = "vec = c(1,2,3)"
eval(parse(text=cmd))
```

## Creating simple R packages {#creatingsimplerpackages}

If you start writing your own functions and you use them often, probably you will soon get tired of sourcing the files containing each function to make them available at each session. There are at least two ways around this problem:


- put all your function files in a directory and write a function that systematically sources them all.
- build a R package

The first solution is rather simple, give the `.R` extension to your R function files and put them in a directory. Although there is not a built-in function to source all the R files present in a directory, the documentation for the `source` function gives an example on how to do it (see `?source`):


```r
  ## If you want to source() a bunch of files, something like
  ## the following may be useful:
  sourceDir = function(path, trace = TRUE, ...) {
    for (nm in list.files(path, pattern = "\\.[RrSsQq]$")) {
      if(trace) cat(nm,":")           
      source(file.path(path, nm), ...)
      if(trace) cat("\n")
    }
  }
```

the function sources all the files with the `.R` extension found in the directory indicated by the `path` argument. You can copy this function to a file, let's say `sourceDir.R`, put it in your HOME directory and source it in your `.First` function in `.Rprofile` (see Section \@ref(custom) for details the `.First` function and the  `.Rprofile` file)


```r
  ## This goes in .Rprofile in ~/
  .First = function(){
     source("~/sourceDir.R")
  }
```

now each time you call `sourceDir` with a directory as an argument, you will have all the functions defined there available. If you want them available at the beginning of each session, just add a call to `sourceDir` for the directories you want to add in your 
`.First` function as well. So for example, if your R function files are in the directory `myRfunctions`, add the following to your `.First` function:


```r
  ## This goes in .Rprofile in ~/
  .First = function(){
     source("~/sourceDir.R")
     sourceDir("~/myRfunctions")
  }
```


Building a R package requires a bit more work. The detailed documentation for doing this is provided in the **Writing R Extensions** manual available at the CRAN website http://cran.r-project.org/. That documentation looks at best daunting for a beginner, indeed writing a R package is not trivial, however if all you have is pure R code, and you just want to build a simple package for your own use, the task should not be too difficult to achieve. A very useful document is **An introduction to the R package mechanism**, it can be found at the following URL http://biosun1.harvard.edu/courses/individual/bio271/lectures/L6/Rpkg.pdf. In the following sections I'll try to explain how to build a simple R package, much of what I say is drawn from the above cited documents.

### The bare minimum to create a package

The quickest way to get started is to use the function `package.skeleton` to create the first ``draft'' of your package. Start a R session, make sure that there are not R objects in your session, otherwise they will be bundled in your package


```r
rm(list=ls(all=TRUE))
```

now source all the function files you want to include in your package, for example


```r
source("/home/sam/myFunctions.R")
source("/home/sam/soundFunctions.R")
```

and the call the `package.skeleton` function with the name you want to give to your package as the argument, for example ``mypkg''


```r
package.skeleton("mypkg")
```

this will create a directory called `mypkg` with two sub-directories, `R` containing your code, and `man` containing the documentation files. Furthermore a file called `DESCRIPTION` will be created. If the objects you are packaging include datasets, a `data` directory will also be created. These are the essential elements needed to build a package. The exact content of these files and directories, and how to edit them will be explained later, for the moment I'll give an overview of the steps required to start using your package. The next step consists of building the package. Start a shell (not a R session), move one directory above the `mypkg` directory we've just created and give the command

```
$ R CMD build mypkg 
```

to build the package, this will create a tar gzipped file with everything necessary to install the package, the next step to do is indeed the installation. I would recommend installing your own packages in a separate directory from the default package installation directory, let's say `~/personalRLibrary`, to install the package in this directory, still from the shell call

```
$ R CMD INSTALL -l ~/personalRLibrary nameOfTarFile.tar.gz
```

now from a R session you can call your package with

```r
library(mypkg, lib="~/personalRLibrary")
```

if you want to add permanently your personal R library to the library search path, you can add the following line to the `.First` function in your `.Rprofile`


```r
## This goes in .Rprofile in ~/
.First = function(){
    .libPaths(c((.libPaths()), "~/personalRLibrary/"))
}
```

in this way, after starting a new session you'll be able to load your package without having to specify in which library it is located


```r
   library(mypkg)
```

The one described above is a very quick but rough way of creating a package, in order to properly create a R package a number of additional steps, like writing the documentation, and adding examples, need to be followed. Some of these steps will be described in the following sections. Always remember that a very useful thing to do when learning how to build a package is to download some source packages and explore their contents.

#### Editing the `DESCRIPTION` file

The `DESCRIPTION` file follows the Debian control format, and has a key-value pair syntax. The default fields created by `package.skeleton` are pretty much self-explanatory. Other fields that can be added are


- *Depends* If your package depends on a particular version of R, or on other packages, these should be listed here. For example:

```
  Depends: R (>= 1.9.0), gtools, gdata, stats
```
  
- *URL* The URL of a website where you can find out mode about the package. For example:

```
URL: http://www.example.com
```


#### Editing the documentation

The documentation files reside in the `man` directory of your package. There is one documentation file for each function or data set present in the package. The documentation files are written in a \LaTeX\ like format called `Rd`. `package.skeleton` creates a skeleton of the documentation file, which just needs to be edited, the default fields are pretty much self-explanatory. For a more detailed explanation you can read the *Writing R Extensions* manual http://cran.r-project.org/doc/manuals/R-exts.html. I'll give you just a few tips:

It is possible to add additional sections beside the default ones, for example it may be useful to add a ``Warnings'' section if you have any warnings to give on the use of the function

```
\section{Warning}{Calling this function with arguments foo foo2 can cause ...}
```

In the `seealso` section, you can refer to other functions contained in your package, for example

```
\code{\link{functionFoo}}
```

will automagically add a hyperlink to the documentation for the function `functionFoo`

In the `Examples` section, you write code as if you were writing it in a R script. You can use datasets from your own package, or from the standard R dataset. Keep in mind that the examples should be directly executable by the user, either through copy and paste, or through the `example()` function. When the package is installed, the examples will appear in a directory called `R-ex`, however you do not need to bother about this, the R code for your examples needs to be written within the documentation `Rd` files.

The documentation requires the presence of one or more standard keywords. One way to get a list of these keywords is to download the tarball with the R sources, after unpacking it, you can find the keywords in a file within the `doc` directory called `KEYWORDS.db`. 

#### Converting Rd files to other formats

HTML and LaTeX versions of the documentation files are automatically produced in the package installation process, you can find them in the `html` and `latex` directories of your package installation directory, respectively. You can also produce a single pdf or dvi file containing all the documentation using the following command from a shell

```
$ ## produce dvi
$ R CMD Rd2dvi /path/to/your/package/sources/
$ ## produce pdf
$ R CMD Rd2dvi --pdf /path/to/your/package/sources/
```

#### Adding additional function or data files to the package

Adding additional function files is quite straightforward, the files contained in the `R` sub-directory of your package directory are plain R files, so you can just write your functions, drop the files with your functions there, and next time you build the package the new functions will be included. The function `prompt` can be used to build the documentation templates for new functions:


```r
myfun = function(arg){val = arg + 3}
prompt(myfun)
```
this will create a `.Rd` file which you can edit, and drop in the `man` directory.

Datasets can be saved using the `save` function:


```r
mydata = seq(1, 10, .1)
save(mydata, file='mydata.rda')
```

documentation again can be produced using the `prompt` function


```r
prompt(mydata)
```

#### Checking the package

The sanity check for the package can be done by issuing the following command from a shell

```
$ R CMD check /path/to/your/package/sources/
```

#### Other resources

@Wickham2015 is a book dedicated to the development of R packages. The draft of the 2\textsuperscript{nd} edition of the book is available [here](https://r-pkgs.org/).

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

## Objects

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

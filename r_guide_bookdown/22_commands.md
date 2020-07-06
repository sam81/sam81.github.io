# Miscellaneous commands

## Data

- `head(dat)` print the first part of the object `dat`
- `tail(dat)` print the last part of the object `dat`


## Help

- `?foo` find help on command `foo`

## Objects

- `class(foo)` get the class of object `"foo"`
- `ls()` or`objects()` list objects present in the workspace

## Organize a session

- `dir()` or `list.files()`list the files in the current directory
- `getwd()` get the current directory
- `library(foo)` load library foo
- `library()` list all available packages
- `q()` or `quit()` quit from current session
- `require(foo)` require the library foo, use in scripts
- `setwd("home/foo")` set the working directory
- `system("foo")` execute the system command `foo` as if it were from the shell


## R administration

- `library()` list all installed packages
- `R.version` info on R version and the platform it is running on


## Syntax

- `#` starts a comment
- `mydata$foo` refer to the variable `foo` in the dataframe `mydata`
- `:` interaction operator for model formulae, `a:b` is the interaction between `a` and `b`
- `*` crossing operator for model formulae, `a*b = a+b+a:b`








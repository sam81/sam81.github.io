
# Environment Customisation {#custom}


Perhaps the best way to customise your R environment is through the use of a `.Rprofile` file, that you put in your `HOME` directory. This is a simple text file that is sourced every time R is started, so you can put in it your own functions, and any operations that you would like R to perform at start-up. Also in this file, you can write two special functions, the `.First` is executed first at the beginning of a session, and the `.Last` is executed at the end of a session. The `.First` function is normally used to initialise the environment setting the desired options. Here's an example of a `.Rprofile` file


```r
##This is my .Rprofile in ~/
.First <- function(){
    options(prompt=">>> ", continue="+\t")  ##change the prompt
    options(digits=5, length=999)           ##display max 5 digits
}

setwd("~/rwork")   ##Start R in this directory
```

for a full listing of the options that can be set see `?options`.
You can change temporarily the options, only for the running session, directly from R, for example:

```r
options(digits=9)
```


The `.Rprofile` file in the user's `HOME` is only one of the files that can be used to initialise the environment and set the options. The file that is looked up first by R is the one defined by the `R_PROFILE` environment variable if this variable is set. To verify the value of this variable you can use


```r
Sys.getenv("R_PROFILE")
```

```
## [1] ""
```

```r
system("echo $R_PROFILE")
```

if the variable is unset, R looks for a file called `Rprofile.site` that is in the `etc` sub-directory of your R installation directory. To find out your R installation directory on a Unix-like system you can use


```r
system("echo $R_HOME")
```

The `Rprofile.site` or the file pointed to by the `R_PROFILE` environment variable can be used for system-wide configuration. Note that if the `R_PROFILE` environment variable is set the file pointed to by this variable is used, and if this is not `Rprofile.site`, the latter is ignored.

The `.Rprofile` file in the user's `HOME` can be used for user specific initialisation, and the functions written in this file  overwrite, or better "mask" functions with the same name defined in either  the file pointed to by the `R_PROFILE` variable or in `Rprofile.site`. Moreover a `.Rprofile` file can be put in any directory, then, if R is started from that directory, this file is sourced, and it masks the definitions given in the user's `HOME` `.Rprofile` file, in this way, it is possible to customise the initialisation for a particular data analysis. Finally, a directory specific initialisation can be given in a `.RData` file, the definitions given here mask also the definitions given in any  `.Rprofile` files.


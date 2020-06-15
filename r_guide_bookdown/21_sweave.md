# Using Sweave to write documents with R and LaTeX



## What is Sweave?

Sweave provides a great way of writing documents or reports that contain both text and R objects, namely figures, syntax and raw or nicely formatted output produced by R. Sweave is also a way to automate the production of documents. Usually when you write a  document with some statistical content, you first write the text, and then add the graphics produced by some statistical software or spreadsheet application. Of course this process is time consuming, not to mention the fact that often you have to manually fill in tables with the statistics produced by the application you're using. Even worse, if you've already prepared your report, but then you have to change something in the statistical analyses, for example you have to add or drop a subject, the entire process must be repeated again from scratch. Sweave is aimed at easing this process, it works like this: you write a single source file which contains  your text, written with the LaTeX markup language, and embed in it the R syntax needed for producing the figures, and tables to appear in your document. You process this file with Sweave through R, and you get a plain LaTeX source file which you can run with, well LaTeX of course, to get your nicely formatted output. If your data happen to change for any reasons, you don't have to start again from scratch, you can just run your old Sweave source file on your new dataset, and everything will be updated automatically.

If you already know LaTeX and R, learning to use Sweave will be easy, the additional syntax required by Sweave to integrate R and LaTeX source code is minimal. As a caveat, I have to say that Sweave is still in its infancy, so, for the moment probably you won't automatically get all the tables, with complicated layouts that you'd like to add to your document. In my opinion, however, Sweave already does a great job, and it's well worth using.

## Usage

Recent versions of R come with Sweave already in the base system (in the package `utils`), so you don't need to install it separately. Of course you need LaTeX installed to produce the document later, Sweave only outputs a LaTeX source file and all the graphics needed for your document.

You start the Sweave source file as a normal LaTeX document with the usual preamble, if you name the file with the `.Rnw` extension, Emacs should recognise it and highlight the syntax for you. Then, at the point where you want to embed a chunk of R syntax, you add the following tag:

```
<<>>=
```

after you've finished with the R syntax chunk, you need to add the following tag to start another piece of text written with LaTeX:

```
@
```

in  this way you can alternate chunks of R code with pieces of LaTeX syntax. If you forget to add the `@` tag before a piece of LaTeX syntax Sweave will complain and abort the process, if you instead make a mistake with LaTeX syntax, Sweave won't complain and will normally produce the `.tex` file, however the compilation with LaTeX won't work.

There are different options that determine which R objects will appear in the final document. If you want both the R code, and its output to appear in the document, just use the `<<>>=` empty tag. They will both be inserted in the LaTeX file in a redefined `verbatim` environment. Setting the option `echo=FALSE` lines of R code are not included in the document, while with the option `results=hide` the output of the R code won't appear in the document. Therefore if you want to run some R code, but neither the code nor its output should appear in the document, use:

```
<<echo=FALSE, results=hide>>= 
```

Another option will suppress all the output, except the figures:

```
<<echo=FALSE, fig=TRUE>>=
```

Sweave will automatically put a `\includegraphics{}` command for a figure.

Finally, if you want to use some utilities, like `xtable`, that automatically produce LaTeX objects from R objects, you will want to use the following options to tell Sweave not to put the R output in a `verbatim` environment:

```
<<echo=FALSE, results=tex>>=
```

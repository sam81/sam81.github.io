# ESS: Using Emacs Speaks Statistics with R

If you have  the `ESS` package installed, you can use Emacs for both editing R source files when working in batch mode, and running a R process from  within Emacs. ESS provides an extended set of facilities for both these tasks, among these are syntax highlighting, indentation of code and the ability to work with multiple buffers.

To get syntax highlighting, just use the `.R` extension for naming your file.

To start an R session from within Emacs, press `M-x`, type R in the minibuffer, and press `Enter`.

Another nice way of running an R session from inside Emacs is to run first a shell in Emacs (press `M-x` and then type shell in the minibuffer), and then calling R from that shell. However, this doesn't involve ESS, so you won't have all the features that ESS adds to the Emacs editing facilities.

For sake of clarity, the use of ESS for editing R source files, and for running a R session will be addressed in two separate sections, however this separation is quite artificial, first because the most proficient use of ESS involves editing a `.R` while running a R session, and second because some of the tips given in one section apply also to the usage of ESS illustrated in the other section. Therefore you're invited to at least skim through both sections, even if you're interested on one usage of ESS only.

##Using ESS for Editing and Debugging R Source Files

If you have a basic knowledge of Emacs you will feel at home. A good way to use ESS is to split the Emacs window horizontally (`C-x 2`) and have a R source file in one buffer and a R process running in the other buffer. You basically write the R code in the first buffer, and then send it to the R process for evaluation. Here are the shortcuts for sending input to the R process:


- `C-c C-b` Evaluate buffer. This means that all the commands present in the source file will be executed

- `C-c C-j` Evaluate only the current line

- `C-c C-r` Evaluate selected region


There is a set of commands that is equivalent to the above, but moves the cursor to the R process window after the evaluation, that is they `Evaluate and go' (to the other window)


- `C-c M-b` Evaluate buffer and go

- `C-c M-j` Evaluate line and go

- `C-c M-r` Evaluate  region and go

To comment/uncomment a region you can use

- `M-x comment-region` Comment region

- `M-x uncomment-region` Uncomment region 

- `M-;` Comment/Uncomment region

If you want to switch from a buffer to the other one you can use `C-x o`. Moreover, when you are in a buffer you can make the other window scroll without moving the cursor with `C-M-v`.

#### Italian Keyboard Mapping Issues

Many Italian keyboards don't have the  braces `{` `}` and some other symbols like the tilde `~`, however, in Linux, if you've chosen an Italian keyboard layout they're mapped somewhere, and you can access them through a combination of keys, likely combinations are:
```
Shift + AltGr + [   for {
Shift + AltGr + ]   for }
AltGr + ^           for ~
```
  
if you still can't find them, try pressing `AltGr` with some keys in the upper part of the keyboard, and see if you can spot them.

In Microsoft Windows the braces `{` `}` can be accessed as above, while to access the tilde in most applications, including the R console, you have to write the ASCII code `126` with some modifier function key (in my laptop that's `Alt+Fn 126`). In Emacs however you can't write the tilde in this way, a solution is to write the character in octal code by typing:

```
Ctrl-q 176 RETURN
```

## Using ESS to Interact with a R Process

To start a R process type `M-x R`. `C-p` and `C-n`, or `C`-&#8593; and `C`-&#8595; are for scrolling through the command history.

One thing you need to know, is the ESS "smart underscore" behaviour, that is if you press the underscore once, you get the assignment operator `<-`, if you press the underscore twice you get a literal underscore `_`. This shortcut for the assignment operator can be very annoying if you use the underscore often in variable names, to turn-off this smart behaviour you need to put the following line somewhere in your `.emacs` file, but before ESS is loaded:

```
(ess-toggle-underscore nil)
```

When you are running R inside Emacs, if the cursor is not positioned at the current command line, and you try to retrieve some command from the command history with `C-p` or `C`-&#8593;, Emacs will complain saying that you are "not at command line". To get at the command line without using the mouse, press `M-Shift->`.

If you set the cursor at a previous command, and then press `Enter}, that command will be evaluated again.

There is a quick way to source a file, `C-c C-l filename` will load and source your file.


`C-c C-q` is another way of quitting R in ESS mode, but I guess `C-d` remains the fastest one.

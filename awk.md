# Awk Cheatsheet

## Awk Basics

`.. | awk '<script>'` — apply a script to the stdout  
`awk '<script>' <file>` — apply a script to a file  
`awk -f <script file> <file>` — same, but with a script file  
`awk -F<char> ..` — set a field separator (explained below)


## Awk Scripts

> A script for awk is basically a sequence of `"<pattern> { <command> }"`.  
> Awk (not **awk**ward, but **A**ho, **W**einberger and **K**ernighan) is a handy scripting programming language.  
> It's main mission is text processing. It splits a text to smaller parts (records), for each record awk checks if it match a selector (pattern) and executes corresponding commands. Simple as that. 

### Patterns

> It's important to notice that any selector (instead of BEGIN and END) could be replaced by a if/else statement within commands. See an examples section below.

``` `` ``` — accepts everything  
`<boolean>` — any boolean expression you can imagine  
`/<regex>/` — any regex you can imagine  
`<pattern>,<pattern>` — records that lies within this range will match a selector

`BEGIN` — special selector, commands within it executes before awk begins processing a text  
`END` — special selector, commands within it executes after awk ends processing a text

### Commands

> In a nutshell, commands together — is a simple code like on any other language. It has simple syntax, very similar to C. Commands are separated by a semicolon. They operate on custom and builtin variables. Before processing a record, awk splits it to smaller parts considering a **field** separator.

`$0` — current record  
`$<number/variable>` — denotes a **field** in a record with a given number ($ can be considered as a function)  
`<variable> = <value>` — assign a value to a variable (by default they are zero or an empty string)  
`<space>` — space concatinates things (e.g. 'x=2; print 1 "," x;' gives "1,2")  
`a[<key>] = <value>` — awk supports associative arrays  
`a[1 "," 2]` — and some kind of multidimensional arrays (via associative ones)

#### control flow statements

> Awk supports all common control flow statements like if/else, while, for, for/in, do/while, break and continue. It's obvious how to use them, so I'm not going to cover them. 

`exit` — quit awk  
`next` — stop processing current record go to the next one

#### arithmetic and boolean operators

> Awk support all common arithmetic and boolean operators like +, -, *, /, % (modulo), ++, --, ==, !=, >, <, >=, <=, !, &&, || and space (as concatenation). Like C, in awk 0 (zero) is false and everything else is true.

`<string> ~ /<regex>/` — regex check ("!~" to invert match)

#### user defines functions

> GAWK and NAWK support user defined function.

`function <name> (<arguments>) {<commands>}` — define a function  
`function .. {.. return <value>;}` — to return a value from a function  
`<name>(<arguments>)` — call a function

```gawk
/^#/ {
    log_comments($0);
}

function log_comments (message) {
    print message >> "comments.log";
}
```

#### builtin functions

> Awk has a variety of useful builtin functions. GAWK and NAWK extends this list.  
> Builtin functions for math: cos, exp, int, log, sin, sqrt, atan2, rand, srand.

`system("<command>")` — executes system command and returns it's exit status  
`systime()` — returns the current time of day as the number of seconds since Midnight, January 1, 1970  
`strftime("<format>")` — returns a formatted date

`length(<string>)` — length of a string  
`index(<string>, <substring>)` — index of first occurance  
`substr(<string>, <index>, <length>)` — returns a substring  
`split(<string>, <array>, <delimiter>)` — split a string by a delimiter and put it to an array

`print <something>, <something>, ..` — just prints things  
`printf("<format>", <arguments>)` — prints a formatted string (same as C's printf)  

`>` — send output to a named file (e.g. 'printf("hi") > "./hi";')  
`>>` — append output to a named file

#### builtin variables

`FS` — field separator
> By default it equals a single space. It denotes how to split a record. It can be set with a "-F" option of awk. But setting it with FS is more flexible. For example, you set string separator instead of char ('FS=": "'). Also, you can dynamically change it in runtime.

`OFS` — output field separator (e.g. 'OFS="#"; print 1, 2' will print "1#2")  
`RS` — record separator, by default it equals "\n" (use it with BEGIN selector)  
`ORS` — output record separator  
`NF` - number of fields in a current record  
`NR` - number of a current record  
`FILENAME` — name of the current file


## Awk Examples

#### How selectors can be replaced by if/else statements:
```awk
{ if (NR <= 10 ) {print} }
NR <= 10 {print}
```

#### How to use variables from bash:
```bash
#!/bin/sh
column = $1 # positional parameter
awk '{print $'$column'}'
```
So, we should dynamically generate awk script by concatenating things.  
We can use the script above as follows:
```bash
ls -l | Column 3
```

#### How to initialize an array:
```awk
data = "apple_orange_banana"
split(data, fruits, "_")
```
# Sed Cheatsheet

## Sed Basics

`sed -e '<script>' <file>` — apply script to a file and print the results  
`sed -e 'script' -e 'script' ..` — even more scripts to apply  
`sed '<multiple lines with scripts>'` — same as above  
`sed -f <script file> <file>` — same, but with a script file  
`sed -n ..` — suppress strings not affected by a script


## Sed Scripts
 
> A script for sed is basically a `"<pattern> <command>"`.  
> Sed (**s**tream **ed**itor) applies the command to those lines which match the pattern.  
> Commands usually have their own arguments and also they can be grouped.

### Patterns
`<number>` — line with a given number will match the pattern
`$` — last line will match the pattern  
`/<regex>/` — any regex you can imagine  
`<pattern>,<pattern>` — lines that lies within this range will match the pattern

### Commands

#### substitute
`s/<regex>/<string>/<flags>` — substitute a first match with a string considering flags  
`s:/<regex>:/<string>:/<flags>` — same, but sometimes more convinient  
`s_/<regex>_/<string>_/<flags>` — another way of doing it  
`s|/<regex>|/<string>|/<flags>` — and one more (getting rid of picket fence!)

`s/<regex>/&/<flags>` — & refers to the found pattern (e.g. "s/foo/&bar/")  
`s/<regex>/\<number>/<flags>` — \ refers to the corresponding group in regex (e.g. "s/\\(bar\\)\\(foo\\)/\2\1/")

`s/<regex>/<string>/p` — print a line after the substitute (use it with -n)  
`s/<regex>/<string>/g` — substitute all matches in a line  
`s/<regex>/<string>/<number>` — substitute a matche with a given number in a line  
`s/<regex>/<string>/<number>g` — substitute all matches begining with a match with a given number in a line  
`s/<regex>/<string>/I` — ignore case

#### other commands
`a\<text>` — append a line (e.g. "sed '10 a\hi' .." to append a line after 10th line)  
`i\<text>` — same as above, but inserts a line before matched lines  
`c\<text>` — substitute a matched line with a given text  
`d` — delete a matched line  
`y/<chars>/<chars>/` — transform chars (e.g. "sed '10 y/ab/AB/' .." to uppercase 'a' and 'b')  
`q` — quit sed (e.g. "sed '10 q' .." to quit after 10th line)  
`!` — invert match (e.g. "sed '$ d' .." to delete all lines except of the last one)

#### grouping
```sed
<pattern> {
	<command> # each command should start
	<command> # with the new line!
}
```
> Order of execution is straightforward. 
> In case of multiple sed scripts (e.g. with -e option), 
> first script is applied to the input and it's results are passed to the next script line by line (this is necessary to understand!). For example, `echo '#foo' | sed -e '/#/ y/#/@/' -e '/@/ p'` will print "@foo", `echo -e 'foo\nbar' | sed -n -e 'p' -e 'p'` will print "foo\nfoo\nbar\nbar".

#### multiple lines
`n` — print current line (unless the "-n" is used) and read the next line  
```bash
echo '
	#foobar
	barfoo
' | sed -n '/#/ {
	# current string is "#foobar"
	n # current string is "barfoo"
	s/bar/foo/ # current string is "foofoo"
	p # print it!
}'
```
`N` — add the next line to the current (with \n char)
```bash
echo '
	#foo
	bar
' | sed -n '/#/ {
	# current string is "#foo"
	N # current string is "foo\nbar"
	s/\n// # current string is "foobar"
	p # print it!
}'
```

#### hold buffer
> There are two special locations in sed: pattern space and hold space.  
> Pattern space is just a currently processed line, we're already familiar with that.  
> Hold space (buffer) is more tricky and used to remember the data for later.

`h` — put current line to the hold buffer  
`H` — append current line to the hold buffer  
`x` — exchange the hold buffer with pattern space  
`g` — copy the hold buffer to the pattern space  
`G` — append the hold buffer to the pattern space
```bash
echo -e '#xyz\n#foo\nbar' | sed -n '
	/#/ {
		# current string is "#foo", hold buffer is empty
		h # current string is "#foo", hold buffer is "foo"
	}
	'/#/ !{
		H # current string is "bar", hold buffer is "#foo\nbar"
		x # current string is "#foo\nbar", hold buffer is "bar"
		p # print current string
	}
'
```

#### labels
`:<label>` — create a label (remember goto?)  
`b <label>` — branch to a label (if label is empty go to the end of the script)
```bash
echo -e "one\ntwo\n\n@three\n\nfour" | sed -n '
	/^$/ b paragraph # if an empty line, goto paragraph
	H # else, append lines to the hold buffer one by one
	$ b paragraph # at the end of the file
	b # skip paragraph processing
	:paragraph
	x
	/@/ p # print a paragraph which has @ in it
'
```

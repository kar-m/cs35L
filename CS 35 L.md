Lecture 1/6 

the use of file systysems how they are implemented

a linux file system and how it is used 

scripting

shell emacs and lisp python

domain specific language: works well in application area and does not work well elsewhere

a computer language that is designed for a specific application domain, or problem space

distributed systems 

version control: written vairants of the same program and you want to make sure the right version is being used and poeple can understand the changes

client server is most common form of distributed computing 

leave the project up to you, 35 projects 35 groups of 5 for the projects

open notes create good notes for this year

robust programs make sure can handle all input

# start of lecture

___


hardware 

assume x 86 -64 

on top of this the linux kernal is ran

halt instruction can halt the computer but your computer cannot

you can do ordinary things like add, sub, load and store

all these run direcetly on the hardware

linux kernal applications can do reads and writes open a file and do a shutdown

a kernal decides what to do with requests from applications


diagram
app1 app2 app3

kernal 

hardware

standard function called memcpy to copy one arrray into another (dest, src, nbytes)

dest is ptr and nbytes is an integer

call the function it arrnages to copy as fast as possible

looks what what x86 model you have and tailors the instructions

these live in c library these are useful collections of code that get used by lots of applications

ldd/user/bin/sh shows libraries used by a program

on a fancier program it show s]more libararies

from an applications point of view it can talk to the keral but it can also talk to libraries

- the difference between a kernal and a library is a kernal can use privledged instructions can do networking and I/O directly

the libraries are shortcuts for the computer, the code could just be copied but it minimizes the amount of copying and reduces application startup speed

# Emacs
____
Emac lets you run the emacs appliaction it knows about windowing systems and starts up a graphical user interface to let the user talk to emacs

- know hot to exit a piece of software at first
- the way to exit emacs is type C-x C-c
- crtl('A) gives one maps to the ascii chracter
- ctrl x is x & 0x1f it lobs off the higher order bits with the and opertion
- you can start up emac by using an emacs -nw flag, default in seasnet this creates a non graphical user interface only using characters however only using the terminal, a reversion to technology of the 19070s
- is popular when setting up servers remotely, when using graphical user interface it increases the latency, using a non graphical user interface for emacs is popular for dev ops
- C-u followed by anythiny : do that thing 4 times
- C- u 40 anything: to an input 40 times
- emacs is a modeful editor, what emacs does depends on the character you type depends on the mode it is in, the internal state greatly affects its responses to user input
- keep track in your mind of what emacs is doing based on your key commands not desiged for novices
- read the chapter of emacs on mode bind
- M-x shell RET hold down the key meta key or alt, RET is enter, allows for subshell running under emacs

pwd show your home directory

# Lecture Jan 8 
___
IDE EMACs

- Simple scripting method the shell
- filesystem, Linux POSIX
### Posix - a set of instructions for an application to run on multiple operating systens
___
1. survive power outage save work at all times
2. be fast and have a high speed 
3.  be understandble

- Store is persistent and survives power outages
- RAM and CPU is volatile 
- we have ram because it is fast and storage is slow
- know when you are going to lose your work and when you are not going to
- for the developers of the systen
- have to know the rules of persistence vs speed

### EMACs GUI
---
the shell is running on top of the kernel which is running on top of the hardware

the shell is not doing anything at all except for waiting for EMACs to finish

scripting sh controls the kernel

- C-x d drops into a directory editor where you specify the name of the directory. you would like to edi twith emacs
- / is the bottom directory of the file system
- directories can have files underneath them and subdirectories
- in EMACs you can provide a textual description of the directory
- the names is the right most column
- the directories map file name components to files
- file name components are a finite byte sequence with a few exceptions


### file system
___

you cannot put a slash into a file name component or a null byte, and in some special cases you cannot put "." or ".."

There is limit for how long a file name can be changes from one filesystem to another but usually 255 bytes

a file name must be non empty

you can have an empty file however






### shell
___

machine instructions load add and subtract

emacs and shell can run at full speed

some are security regulated I/O instructions do not want anyone to execute or privledged instructions teh kernel is the only part of the system that can execeute them

one of the sytstem calls is running another program to execute done by the kernel

usr user acessible
usr/bin tells from filename to the root

usr is part of the root directory

bin is mapping of the usr directory to another directory

can go to the end to do alt greater than or meta greater than to skip to the end of a file

zstd short for a compression program

a minus sign means a regular file

a very bushy structure in real trees are hard to draw and very complex

fully qualified name of a file /usr/bin/zstd look for usr component in tree, look for bin component and look for zstd component

# new command
___
alt-x shell enter start a user shell and run it under user control or m-x ret

subsidery shell running under EMACs, bash short for born again shell wrote after steve born went to become venture capatalist


### about trees in Linux
___

a directory can only have other entries, in a directory you cannot have loops or cycles to parent nodes, every directory other than the root has one parent

### file - saved state of anything program we happen to run

- if ordinary it is regular file it is a finite sequence of bytes
- a file can also be a directory
- not directory is a regular file, branded at the start of creation
- you can look at that directory by running a command in emacs

### bash commands
___
LS- tells you directory entries in a file system, tells subsiddery entries

LS -l / we will see more details about the particular directory, a different attitude

emacs treats this text differently as it does not know this is a directory

you can run any shell command you want under emacs direct

calls LS with a bunch of fancy options you normally would not use

-rwxrw-r-- contains what is called the mode and sometimes called the permissions of files

minus sign regular
the d is a dirctory
the l in cursive is a symbolic link

the permissions are an octal or binary and a number that tells who is owns the file, usually octal when wi\ritten by humans

first class is the owner of the file, all owned by root, which is the superuser with user ID 0

most people do not have root permissions enabled

r means read permissions
w means write permissions
x means execute and search permissions

id shows the id of your account

ls -ld ~/.

!!/. lookup what this mean slater

!ls

chmod g+w ~/. gives permissions and lets others write to this directory

Group - 

Other - 

directory on seas net is a symbolic link to somewhere else

symbolic links make easier to read

/. shows not intersted in symbolic link shows pointer and shows the actual directory it points to

chmod 755 ~/. the second part is the home directory

this gets rid of write permissions as 755 is the bit pattern that corresponds to writing

chmod 047 ~/.

!ls 

now one cannot access their home directory, locks themself out of home directory

cannot change home directory once changed, since cannot look at home directory cannot find . in it

chmod 755 /w/fac.2/cs/eggert by passes home direcoty on upper right of screen by bypassing symboic link

numeric mode is for people who know what all of the bits means, some people know what all the symbols mean, chmod manual page to find all the details

M-x man for go into the menual pages 

chmod ug+x(turns on the x bit and r bit for both the user and the group)

chmod 987, insists something that makes sense of does not work

precedence between read wriete and execute, 3 bits for owner start, 3 bits middle group not owner, last 3 for neither

the users are in some sense just numbers, users can own files, the user that is associated with that file is the owner

# CD 
___
makes change directory

it tells the shell to turn its attention to a different directory than the directory it was already in

a current directory is any directory in the system, sometimes called the working directory

every program on the system has its own idea of the working directory

the shell says where is based on the cd command and changes where a user is

cd with no argument goes to the home directory


cd /etc is the grab directory where they could not decide where other files would go

ls -l passwd file contains every users name and their IDs and their home directory

C-x C-f to find a fle and then you can find the file, and shows a list of the users who are allowed to login into the particular machine

halt -  halts the system

pipewire user 994

super user can set roots for users decides what roots a user belongs to

write permission is the most dangerous allows other users to rewrite over your content

in linux a user can be in multiple groups and change a group of a directory to any group you may be in to access the directory currenlty in

C-x(interactive search sort of like regex and searching in vs code)

12 bit system ordinarily the beginning bits are 0, ocassionally you run across files were the bytes are not 0

search suv you find a red s under sudo, su is short for substitute user id, take whatever you want, substitute one user id for another

su then asks for root password,

C-c sends a quit signal

if you know the root password you can become anyone

works because of the special permissions on the command

4 th letter if x means 000 is 100, by putting an s in rwsr instead of rwr

first is setid bit, next setgrid bit, next sticky bit

touch shell creates a file 

chmod 4755 sets a uid so others can write to it

ls - l ouch lets you see the permissions on the files

id shows the id of the file

ls -l tells you the meta data of the file

shows timestamp on the file

setuid lets anyone run the file

dif between set uid and lei people run

# second part of the lecture
___
Link count says how many directories have entires pointing to the final

can create another name for the same file ln

ln is short for link, another file you do not have the name of

ln ouch junk/off now have junk directory next to home directory

ls -ld junk

can see the directory, learn about directory itself

ls -l ouch junk/ouch

ls -l ouh junk/oof

if file is now modified using command like echo aaaaak >ouch please run the subcommand and send its output tio the fike ouch


echo accckk outputs to standard output

echo aaack >ouch

ls -l ouch adds output to new file

ls -l junk/oof has also grown, two names for the same file, 

two arrows pointing to file or 2 and that is why link count is 2

ls -ld . .. junk junl/. junk/..

tells about junk directory itself, two different names for the same file

ls -ld . .. junk junk/. junk/.. . points to that directory, every directoru with a subdirectory and has .. that point to the directory can tell the numnber of subdirectorires by counting the link count

the link count will be high if there are a ton of subdirectorires

mkdir xyx

los -ld xzy

link count is 2 makes a directory, 

df -h .  command used to display information about the file systems on your Linux system
shows in human readable format

filesytem, tells how many bytes it takes to represent the directory

mode line white line in emacs

the rm command is very fast and makes a very small change to a directory doesn't delete data in a file

if there is another hard link it does not keep it safe, removes a pointer from a file does not actually delete the file

link count of file will drop, that does not mean the contents of the file have gone away they can still e accessed

cat wiht no arguments reads from standard input and copies to standard ouput

cat reads from input and prints to standard output

cat - ouch reads from standard input and then ehoced it back out and then can combine a cat of another file

cat - ouch < ouch

ouch is the standard input and then it will read from ouch

can do in a parathensized way as well

(cat - ouch; cat ouch) < ouch

read all in an environment where standard input is ouch

(rm ouch; cat) < ouch in files where ouch is a standard input its link count will drop to 0

ln rm creates new directory entries, rm mearly removes the directory entry file, are fairly efficient commands

rm ouch; cat < couch

less  than has higher precedence than semi colon so does not work

once the last command that has access to the file exits it exits

### symbolic links
___
can link to files that do not exist yet

link to directories 

ls -ld /bin

the link is imaginary

not the same as hard links

can put in a directory

can also link to other file systems

df tells all files sytems we have and each file system is a tree

every file system is named by files in the work filesystem

df /tmp 

hard links can only go within a file system

crossing file system boundaries you have to use a symbolic link

how they work?

the contents of a sym link is a byte string that gets spliced into a file name, other files systems

ls -l /bin

ls -l /usr/bin/sh

/usr/bin/bash

until no symbolic links in the same and know true name of the file

ln -s loop loop

process will go on forever byt linux has limit to symbolic link

cat loop when it says too many loops or too many

ls -l nowhere

symbolic links to files that do not exist, no such file or directory nowhere

ln -s '' empty

if starts with / it starts at the root

if it starts with a non slash bin/sh

looks for a bin in that directory

absolute and relative file names

symbolic link to be relative when spliced, relative to where it is
absolute symbolic link starts at the root directory

ls -s usr/bin bin

ls -l bin

ln a d suppose this is an existing directory then this is equivalent to ln a d/a

ln builds links

ln -s builds symbolic links

ls -al lists content of curren directory


# notes
___
make dribble files for each one 

make on for each section, cannot make up dribble file, only the things they ask for

metadata is managed by the operating system

OS  is in charge/ program makes requests

but ..  user and/or root can override this - later in course

M-x shell start a shell

c-x o switch to other buffer

pwd see where you are

.. is a link from current directory to home directory

#' this sign treats the file as a common

c-p goes up

c-f goes right

C-@ place a mark

C-e end of line

C-a start of line

C-y yank (paste from the kill buffer - clipboard)

C- d EOF (in shell mode)

M-w copy from the mark to here

make txt file for 1.5

make files in emacs

C- x C- f find file -specify a file name

file - data in secondary storage (OS notion - visible to aps)

metadata(timestamps, ownership, oermussuib, etc)

buffer - data in emcas ram

Assignment does say Emacs 29.4, because it was corrected recently; however you may be using a cached version in your browser 29.3, clear browser cache. shift reload

stick with lnxsrv11, 12, 13, 15 until clear up

C-v go down a screenfukl

window - Emacs "window " - view into a buffer

frame - Emacs frame is what linux, ms-windows, call a "window"

aposthrophies around file name for file, do not treat special characters
file name does begin with sharp sign

sh script runs script

ls -al you can look at files

C-x C-s save file (current buffer), inverse of C-f C-x

Emacs creates a file starting with '#' to hold the contents of buffers associated with files, even when yu haven't done C-x C-s. It does so periodicallu, not all the time that would be too slo

symbolic link everything after arrow

by convention, file names beginning with "." are less visible to the user.
ls normally doesn't list them ls- a lists them all

ls -a lists symbolic link files

emacs creates dangling symlinks with names starting with ".#"

cat file with symlink doesn't point anywhere you cannot open anything

ls -l !$ means repeat last argument in command line, we can look at the sym link and see where it points to

get information from that string

looks who is editing file in emacs

process id for emacs is the process I am ediitng

ps -ef | grep lets you find process id

ps - ef | head -n1

date + "%Y-%m-%d %H:%M%:%S % z % (%Z) %s"

date shows the dae

how can we protect processes information -  so that root (superuser_)

ps -f lists more details about processes


Ubuntu does let anybody but root see the processes

even OSes that do not have root still have problems aat the hardware level

if you turn off machine ram servives, starts decaying but survives for a few seconds. When you turn back it back on boot process ears ram

attacker can see ram  in charge of your boot prom can read that ram

so grep is essentially like c-f in process dodcument within status

sort of grep reads from stdin
and outoputs matching lines to stdout.

it is fancier than C-f because it uses regular expressions and it can negate and etc.

ps -f long versionsame processes
a few extra information, user id, parent process id,
every process has a parent

M-x man ps then you type c and learn what it does

stime, tty is controling terminal for processs
you can type in tty

ps - f give me more columns

ps -e gives more rows processes that do not even have control terminal

ps -ef gives more rows and more columns

same as other comand ps -e -f

shell commands:

- simeple commands
a bc def ghi

run the program "a" with the arguments "bc", "def", "ghi"


echo is a simple command

outputs the arguments seperated by spaces to stout

a bc def >h 

the less than sign means take stdin from that file

the greater than file means put stdout to that file

cat xxx
can show the files

cat < xxx reads from stdin

echo bc | cat, an arrow going throuhg eho to cat

echo def | cat less than h
creates a pipe between two progras

cat h

echo def | cat less than h

echo uv xy | cat h -

echo uv xy >tmp; cat h tmp a piepe will be faster run in paraallel

ls -l tmp shows speed

grep foo tmp shows for every instance of a line 
grep bc tmp

cat tmp

grep uv tmp outputs that line

grep uv tmp ouh

grep uv tmp ouch > tmp1 2> tmperr

cat tmperrr is the message

grep uv tmp touch 3> tmperr

stdin from that file stream 0

put stdout to that file stream 1

2> put stderr to that file stream 2

command. > outfile 2 > &1 make stream 2 point to the same thing as stream 1

grep uv tmp ouch > tmpb 2 >&1

a | b run a and b in parallel

a's output goes to b's input

a will wait if the pipe is "full"

b will wait if the pipe is empty

$? stands for the exit status of the most recent command

grep uv tmp ouch >tmpb 2>&1

0 means success anything else (1-255) means failure

true 

echo $?

true  b  > c

ls -l  b c

echo bye >>c

cat c

echo xx >>Yyyy

echo zzz > Yyyy

dangling symbolic links contents to help

steal file from other session or giveup

C-x C-c exit emacs

proc file system special directory a bunch of subdirectories

each number corresponds to a process

echo  echos the process id PID

ls -l /proc/PID shows information about the process and the file, visible as a filesystem

operating system gives you information about the file system

ls -l proc/cpu/info

regular file, size is 0

cat /proc/cpuinfo

shows the contents, changes at random
hardware associated with the operating system

C-s to search for names

emacs help key is C-h

C-g interrupt, go back to top leel

C-h help

C-h t tutorial

C-h info

C-x o switch to other buffer

C-x 2 split window or buffer

C-k kills everyting at end of line

C-x 3 split horizontally

Mark region in a lot of details,

emacs in mode tells in text mode C-x 1 want to focus on the help

C-k short of shortcuts

WS is white space mode

C-x 5 create new frame

C-x 0 delete window

C-x s save all files (all buffers) interactively

C-x C-b to list buffers

Note: Some of these shortcuts could be used in the shell command line and they would do the same like C-k

You gotta enable it in which ever terminal you use. I use iterm and its a option that you can select for the terminal to interpret option as Alt

C-d edit the directory

type g and grabs all information from file system and puts in new file

if you kill a buffer and type x then stops displaying that buffer and displays a different buffer

limited mouse support emacs

M-x voew-lossage to see what you can been up to

C-h can find out about what a particular keystroke will do help about a keystroke sequence

C-h k key help about a kerystroke sequence

you can change emacs control line kill buffer and not there

you can run emacs list code yourself

#lecture Jan 15th
___


Little language is designed to be small laguage captures specialized part
make small and easy to use DSL, domain specific

kitchen sink language one language to rule them all

C#,  Java, maybe C, C++, Python

the language of POSIX/ Linux file names(little language)

a file name is a string of bytes except with some rules: / is special (directory spe) '\0' is special because it ends the file name

a file name is a sequence of file name components each component is a nonempty sequence of bytes, none of these bytes can be '/' or '\o')

if a file name starts with '/' it's absolute: you start at the root directory to interpret it; otherwie it's relative and you start at the working directory

"" (empty string) is not a valid file name.

"/////"  is another name for "/", because two or more slashes are treated like a single slash

ls - i /. unqiue number for each file

ls -ldi '' / // /// //// ways to access

ls - lid / /. /.. // shows alll the file names, root is a special directory in its sense its own parent

"../" this is a relative file name the trailing slash means this file must be a directory

ls -lid /bin/sh/ unless this happens ot be directry cannot do

ls-lid/bin/sh symbolic link to directory

pwd shows working directory

touch -minus does not work, - signs start options and cannot create

rm -minus would not work because invalid option

needs ./-minus infront

- can put any character in filename with ./


a larger little language: globbing patterns (for the shell)

ls - l/bin/sh/ tell about all the files under the bin directory that end with sh

star - match zero or more characters in a file name component

ls -l '*x' match any with xs with the 'x' 

'?' - means match exactly one character in a file name component

[xyz] match exactly one character in a file name component this must be enclosed characters, brackets do not nest in usual way

[x-z] includes all the charatcers inside

[^] this symbol negates character classes in globbing

bracket names in file patterns do not work on slashes only n patterns

[^a-z] means everything but lowercase letters

[^[:lower:]] everything but lower case letters

[^[:alpha:]] - everything but letters

[ac?*] ? '*' are all the asame with staras


___
A directory hierarchy is a tree.
A file name specifies a path through that tree
	starts at either the root directory or current directory
		file name is a "little program" telling you where to go next
		the program counter points to the next file name component
		the state of the program is: which directory (or file ) are you at?
a globbing pattern is a more complicated program
	because, the instructions contain '*', ?, 


3. regular expressions which are used by the 'grep' (command)
		there are two or more commands
		fixed grep is grep - F
		ordinary grep - basic regular expressions (BREes), egrep gives extended regular expresssions (EREs)
		extended grep by using grep - e
EREs
	x any ordinary character matches itself
		ls - l / | grep -E rwx---
		P*  (P any pattern) matches zero or more instances of P
		ls- l / | grep - E 'rwx'- '*' means I want to have a minus sign
			ls -l / | grep 'ro*t *r'
		(pq)'*' matches zero or more insacnes of whatever pq matches
		grep stdin, does not care where data came from
			P{12}matches 12 instances of P
		P? matches 0 or 1 isntacne of P
		p{12,} match 12 or more
		p|q matches anythign matched by p or q
		. matches any single character
		bracket expressions (same as globbing)
		\x (x is a special character) matches that special character
		^P matches only at line start
		$ matches only at line end

find recurses through data in hierarchy a different problem, although notation may be similar

why is it matching 0. or more instances when you use '*'?
woudln't tht just shwo evertyhing regardless if the pattern aws found or not

yes the pattern "x*" will match any line

echo 'abc)def' | grep - E '('

ls - l /bin/ | grep '/$'

grep '$a' # never matches anything

grep 'a^' # never matches anything

Distributive law: P(Q|R) = (PQ|PR)

my invention
	~P == ...P... (not doable in general with any Pattern)
	~x == ([^x]|?...*)


Q. tabs and newlines

in grep a tab is not special matches itself
a new line in a pattern means you have multiple patterns, and you match any of them

Q. can matches cross line boundaries?
not in ordinary grep; look at 'grep -z' (GNU extension)

BREs like EREs except 
	?+{|()} lose special meaning; they're ordinary characters
	\{ \} \(\) are subsitute for {} and ()
plus:
\2 matches the same string that the 2nd parenthesized subexpression matches

regular expressions are used a lot in
	grep, sed, awk, sh, perl, python
Not everyone agrees on the syntax os regular epxrnessions

grep vs grep - E
awk - differs
perl - differs ( GNU grep -P)
Python differs yet

R.E. is a little program:
	progran counter - cursor in the R.E.
	data - cursor pointing into the input
this goes fast, if you are careful about |, *, +, ? -  as these create alternatives;
the state space can grow exponentially.

BREs tend to be faster because they lack | + ?; but watch out for \1,\2, as these can be very slow

? means 0 or 1 vs + means 1 or more vs * means 1 or more

The shell language
	Two uses:
	1. interactively
	2. shell scripts - as if you typed them by hand
			2a: It has to be executable (chmod +x...)
			2b. It haas to start with aline "#!/bin/sh"
			#! - spcial to the O.S.
			/bin/sh - specifies which "shell" to use
		"#!/usr/bin/[python]"
Each instance of a shell is a seperate process
with its own data, input/output, working directory, etc.

Shell syntax.
	Tokens: individual components of the shell
	xyzzy - any word, delimited by non-word characters
	special characters:
	white space (seperates words)
\x x
'xyz xy' string contents (cannot include single quote)

"abcdef" are simialr, but variable expansion occurs 

reserved words: { commands } start a sequence of commands keep going until of curly brace

if then else fi

case in esac Uses globbing

while do dome

for v in words; do done

white space indenting doesn't matter

! cat hello negation of command we ran

variables have string values

a = 'b c d'

$? - most recent exit status

$* -  all the arguments of the current shell

$@ -  likwise byt works better inside

$# -  number of args

$0 - name of the command 

$x - value of the variable x

${x-default} - x's value, unless unintialized; then 'default'

${x+ifset} - 'ifset' if x has a value, nothing otherwise

${x=default} - ${x-default} , except x to 'default' if unset

cat '$@'

### Lecture 1/22
___
a different way of hooking things together
the shell is a glue pot that lets you glue everything together 

with emacs the goal is in some sense bigger 

sh OS kernel, trpas, shell, pyhon

.c source code of emacs is here

sh configuring all reel apps 

if cmd arg
then x abc
else y = def ghi 
fi
cmd x y with dollar signs in front

complex app

extension language

programming language to do what you want to do 
this is how javascript started out, started as an extension langauge for a browser
sql is an exntension language for databases

tr -c
tr -s

elisp is an extension for emacs lisp, lisp is one of the oldest programming languages still in use

emac is mostly in c, c++ and c is intended for hardcore engineers and programmers

most people prefer to use elisp

if you want to use emacs that is not strictly out of the box you want to use elisp

___
Lisp basics
enough for a feel to get stuff done

a nice property of lisp is because it is so simple can give ideas about extension languages

a very very simple language is some sense

atomic values vs conses

have no furter values then we care about right now

integers and string as atomic values often

symbols are atomic values as well

if you want to have an expression that gives you that value means basically an identifier in lisp

self-evaluating forms includes numbers and strings and other sorts of things

non-self includes identifiers and symbols 

evaluates as you input it in

conses 
___
basic data structure of lisp

a conses is a pair of two other objects and anything can be put into it

cons expr expr

creates a new piece of storage with the values of two expressions in it

the way a cons is printed out is E1. E2

put a bunches of cons together to make a cons

marked with a special value with nil at the end to mark the end of a a list in emacs using conses

nil is the null ptr, or the emacs ender, sort of like linked list

(A,(B,(C, ni)))

car expr gives first value, the head

cdr expr gives second value, the tail

something stupid about brown mercedes license plate

about registers from IBM

setq ls to be all the stuff in cons and creates a list,

Conses can be a pair of any two objects, not every cons represents a list

- a conses is either the empty list of two conses
symbols are just globs of data

in lisp every lisp program is represented as a lisp or data

____
the ' you have to follow but it is very important, if you get it wrong you want understand the program wrong syntax

('cons 3 - 8) lisp--error(invalid function 'cons)

(cons . 8)

(cons cons 8)

(cons ''' cons 8)

(''cons . 8)

quote is another part of the lisp language

(eval 'cons 5 - 8)

(eval '(cons 5 - 8))
(5 . -8)

(eval(cons 'cons (cons 5(cons - 8 nil))))
(5 . -8)

car evaltes all the bumbers and then does a function call

special forms are not function calls
setq cannot be a function, would lead to runtime error if wasn't, doesn't evulate first variable the same

(defin f(a b c) code invoke abc)

(defun ma (a bc )(+(*ab)c))
ma

(ma 9 3 -26)

chars keystrokes are in c part of emacs

hit x can go int c code 

c-h k c-x h tell me what this keystroke set will do 

mark whole buffer command shows the source code of this

is a function that takes no arguments and does all this

C-x C-h marks the whole buffer

M-:  RET lets you type whathever lisp code you want

can evaluate 2+2 gives 4

the system we have here is one where we look through the source of emacs while writing the program

M-x mark-whole-buffer works also

some emacs functions are designed to be hooked up to keystrokes or commands

any elisp code can be called

can also be invoked by M-x mark whole buffer both ways will work

is sitting in some sort of compressed area, M-W, C-X, C-H, C-D to get into scratch buffer

change how emacs works fairly conviniently do not need to recompile source code, any good ide

*scratch* to get into scratch

(global-set-key '\ajakjlfa' gives a random global set key)

what should I do with q

which emacs lisp or c function to call

find out which function is invoked by doing hk

if you type in x will invoke this function written in c code

(setq program (cons 'cons (cons 3 (cons -8 nil))))
creates a piece of data, list of conses -> 3 __ - > 8 nil

program = 'echo hello'

eval "$program goodbye"



emacs -q means startup without any special configuration

variables are typically named by symbols 

setq can give variables a value 

forms n elsip, simplest form is self-evaluating 

setq symbol value - associates expression with a variable, assignment statement

two things going on here names and dots
'
'

___
emacs has a configuration system

go through professor eggerts email configuration system

"\C-cab"

"^C" is ascii character 3 gives ascii character 

escape sequence in emacs 

functions alway use a prefix notation

built in functions that happen to certain notations, gives certain amount of arguments

star

gives indentity element

dividing divides all numbers

divide a single number gives multiplicative inverse 

(/)

cannot call that lisp error

(/ 1 0)

cannot divide 1 by 0

C-c [some type of close bracket]

___
Python

line = '6006, 100, 20536'

types = [str,int, float]

fields = [ty(val)for ty,val in zip(types, line.split(',))]

[(str,'GOOG'), (int,'100'),'(float,202536)]

str('GOOG') 'GOOG'

int('100) 100

float('205.36) 205.36

rebllion against BASIC

BASIC rebellion against FORTRAN for scientific computing

BASIC - instructional language (Dartmouth 1963)

VBasic 'too big'

filled full of 1960s concepts like go tos

better langauge for teaching called ABC

in ABC indentation is automatic in an IDE is or required in other text editors

basic data structures and algorithms built into the system

once you have hash table how can you use hash tables to build more interesting things

floppy disks to all highschools in the netherlands because the students say the jobs are in basic

out of the ashes of abc arose python

was not intended as an instructional language but made it practical

do a lot of the things other programs could do

every value is an object 

Every Python object has h
- an identity
- a type 
- a value


every object has a unique identity

id(27)

8917232
pointer to object gives id

identity always the same identity

also has a type

will give the type

these are immutable

these can change, if the object itself is mutable 

lists are mutable

tuples are immutable

5,6,7 makes a tuple also

___

attributes o.a
methods o.m(e,...,En)

everything is done dynamically

a is b compares identities a==b 

ab == b
' * a == * b'

sequences ops

s[i] accesses indexes

s[i:j] selects subsequence 

s[i:] go to the end of the list

s[i:len(s)]

s[:j]

s[:] gives everything

len(s) will give non negative integer

min(s)

max(s)

list(s)

mutable sequences lists,

s[i] = v

s[i:j] = s1 entreis are copy of what was in s1

del s[i] removes piece of memory

list ops

s.append(v) o(1) amortized

list implementation pointer to actual values

first n slots occupied other memory is unused

store load store

alloc tells you total number of values allocated to an array

append then asks OS for more memory

order per call to append asssume a billion entries





arrays in stack memory sequence if length n

can also index other way with negative indexes

-len(s) <= i < =len(s)

Python has a lot of built in types

None type name find ptr

numbers in float symbols

### January 27 th
___
Client-sever computing

first venture into distributed computing

just two nodes a client and a server who talk to each other over a network

look how that networks and understand what problems you can run into

C -> S in network

our project will be doing client server. computing

Big topic, see some aspects for the designed project

one way to think about those aspect is technology recommended node and react

spend some time learning about it

constraints in client server computing

built atop what we have already seen with Linux and the GNU environment

built atop the POSIX environment which tries to standardize both

some concepts in common configuration is a big deal, configure them the right way if you do it wrong it will not be done correctly

very small problem of quoting

- quoting is putting something into a character sequence, ways people can attack your system with quoting 

### Client and server  is special case 
___
the server is the ultimate abritrarre if someone is enrolled in the class a simple way to keep track of who is enrolled in the class

the client state is less important

if two clients want to communicate they communicate via the server

architectures network within eacbother and the nesting is relatively common


### Peer to Peer systems P2P systems
___
does not have a central server

instead there is no single authority for anything just a bunch of nodes and a network that they can all talk to

no single peer is in charge

if any single peer goes down the algorithm can still get stuff done

decentralized data and decentralized control

are more complicated to setup and operate than client server

in peer to peer clients can communicate with each other without a server or layer in-between

the client is able to decide certain decisions

broadcast capability within the network itself

### primary and secondary system
___
Primary is the queen bee and the secondary is like the worker bees

the primaries job is to keep track of all the tasks that need to be done

the mirror of a client server system

the primary decides everything

# Issues
___

#### performance issues
---
	throughput
		how much useful work a system can do per second
		important to people who run the servers
	 latency
		 a measure of the gap in time between when you send a request to the server and when you get a response back
		these measures are independent
	if you spend more money on the system usually reduces latency and increases throughput
	these goals are competing with each other

do actions in parrallel

do actions of out of order

registrar can do both techniques

tricks can cause problems such as withdrawing money from a bank before adding money or other times

browser can use data cached to see if it can do actions

clients can cache data to reduce latency and avoid talking over network to the server improves latency

these common techniques have problems

### correctness issues
___
serialization

in order to justify any optimization

we may have done things in the wrong order but there is a good explanation for it

take all actions done put them in a serial order in your head and use that order to explain the symptoms that you observe

things that clients can observe from the messages they get back from the server

request, enroll, student 1 in class 1

the response was no

request, enroll, student 2 in class 1

the response was yes

can not abritarily serialize depends on the application, stricter on banks always some rules about serialization

packaging problem is more complicated with updates and how packages depend on each other

### issue with caching
___
must the cache be correct

cache can be out of date and incorrect

can the client code do useful work with an out of date cache

the issue of cache validation

by this i mean the process of determining if cache is out of date or not

refetching all the data would be effective, see when is the last time the data changed see if time stamp matches the time stamp for that data

eviction is application specific sometimes based on time stamp, handled by the application usually


# Networks
___
focus on the internet most widely known and widely used


### circuit switching
___

what did the internet replace before the internet, most widely used  networking technology was circuit switching

how traditional telephone system use to work

UCLA -> MIT two boxes on the desk called telephones, the telephone is connected to a central office by copper wires

sending analog signals over these copper wires

these central offices are on campus and be talked to over copper wires

AT&T set up switches across the country, connected to each other via high performance wires

the idea is that there is a network across the country, where everything is interconnected

an electrical circuit across the country that goes through amplifiers in between analog connection

circuits arranged differently for different cities, as long as heavy duty connections have enough capacity can support

reserve capacity 
once reserved you have good performance you have guaranteed performance
known constant delay always a constant service

circles larger than triangles, all automated

projection the 1920s that by 1960 they would need so many telephone operators that everyone in the country would be a telephone operator, now you have dial the number

## going to internet
___
was founded by darba or arba or an American agency about a nuclear war if nodes were destroyed

Pal Bran, UCLA alum RAND corporation

the idea of packet switching

- no reservation of capacity
- whatever it is you want you must divide it into small pieces of data called packets
a typical packet size might be say 1500 bytes usually depends on hardware

each packet is sent independently through the network, each node makes it best packet to get it where you want it to go

if one talks it breaks it up into packets and puts it on the internet goes to the UCLA router

then goes to a downtown router any router to map to the location

same sort of network that goes across the country, does not reserve capacity in advance all the packet make choices independently

if network is busy make take a while for packets to get through

now the technology exists to make reservations and get packets through the system always

packets are more efficient

the circuit switching approach, leaves wires unused

the packet switching gets rid of unused reservations using system to full capacity

less setup is involved, no circuit needs to be established

the system is less reliable

### problems with packets
___
3 problems packets can be lost
	- packets can be lost (router overload)
	- received out of order
	- can be duplicated(bridge/router misconfiguration)

there are more

by convention packets are divided into two smarts

header - contains meta information about the packet where it should go networks overhead
used by routers does not contain any data

rest of the packet is the payload - intended for the recipient
the network does not care what the payload is just cares that it sends it correctly

protocols - rules for exchanging packets

sending messages that other people do not understand and will not get the work people want to get done

in sharp contrast to the types of communications in C++ programs

in the internet protocols are in a suite 

that is constantly evolving that deals with many of the common problems in shipping data across the internet

RFC is short for request for comments which sounds like it is a very preliminery version for a protocol

the basic idea of the internet protocol suite is that it is layered

it is hard to think of the things that could go wrong in a protocol

it hard to map back to what packets actually are the bridge would be too far

sort of like the layers in the shell and how layers of applications are atop the shell

at the lowest level : the link layer, P2P connection, assume sender and recipient are directly connected

the next layer above that is the internet layer at this layer we have packets

the next layer is the transport layer we do not have individual packets we are sending video file
talks about setting up data channels and sending data across channels

the application layer is the most abstract and is at the top
this layer is app specific, classic example might be email built atop the transport layer

___

the internet layer's basic protocol is called the internet protocol called IP

trying to get a packet from point A to point B

most common used are IPv4 1983 specified by a pretty large team led by john postel who was a UCLA alum
worked down this hall to create it
connectionless layer, all you have a packet
the header of an IPv4 packet contains the following information about the packet, a byte quantity
a protocol number, length
has a source and a destination address 32 bytes each
conventionally written as 4 byte quantities 192.59.239.12
TTL: time to live field also called the hop count adjusted every time a packet is retransmitted by a router, ships to router each router decrements it, packet will be ignored if sent around too much
encase of a routing error, stops from chewing up capacity in the network for no good reason
checksum: 16 bytes it attempts to catch hardware errors in transmission if bytes are flipped by hardware
ETC:

IPv6 1998 changed the IP addresses to become 128 bit addresses,

simplest transport protocol is UDP user datagram protocol
designed by david Reed who is not from UCLA but is from MIT
a thin layer over IP
applications that do not care about the three problems

TCP: transmission control protocol
originally designed by vince cerf who was a ucla alum and bob kan who went to princeton

get around the biggest problem with packets
getting a way send big packets and you want to ship the data reliably

a data stream that has 3 properties
 - reliable - avoid issue of packets being lost
 - ordered - we want the data to be received the same order it was sent
 - error checked -IP has a checksum but only 16 bit checksum
never trust the next level down

divides the stream into packets

what packet size to use depend packet size on link it is talking to

does flow contol, most important part of TCP

problem with the naive application of sending 1 TB of data to MIT

problem is when those packets get to the routers they will swamp the routers

sluggish parts of the internet where all connections are good is wrong algorithm

ship out data more slowly big part if flow control

3 rd thing is does is retransmission and reassembly

if you send a packet and recipient does not get it you resend the packet

a recipient may get packets out of order deliver in order, reassembling in ram happens on recipient side, 

heaver weight contain more information in the header than IP

when designing Ipv4 certain rules for internal networks

internal routing at UCLA uses IPv6

___
# Application level protocols
Jitter occurs during reassembly

suppose the recipient gets the first 10 packets but packet 4 is missing

some of the information sits in the input buffer and the application cannot see it because TCP waits on certain data always

if a packet is lost a video application wants to keep going

some pixels can be lost but not jitter

RTP
	- short for real time protocol
	- runs atop UDP - because TCP jitters too much
	- designed for video and audio

Zoom uses something called WEbRTC which is built upon RTP, avoid the jitter issues

HTTP(HyperText Transport Protocol)
	-


originally designed as way to setup web pages, TIM beneslee did at CERV 1991

wanted to address physics papers the minute they came out
there were ways of transporting physics papers over the internet but they were awkward

World Wide Web

2 big ideas
1. protocol for sort of exchanging interesting documents
2. standard format for text and multi media documents

HTTP, HTML

telnet leapsecond.com http opens up tcp connection

can send a message to this website

http header is meta information contains the date, server, last modified, connection-type, connection, content-length, e-tag, Accept Ranges

gives status code first with the request

HTTP is a super simple protocol 
- each request plus response is in a separate TCP string
- requests can be seen on HTTP people can see everything about the connection

HTML 
- borrowed heavily from SGML


connections are done more securely through https

http atop a secure underlying connection

TLS - transport level security

____
# Lecture 1/29
___
Client-server web = HTTP + HTML + Javascript

worry about all protocols undestandin eachtoher and backwards compatilbility

### HTTP1
___
http/1 Tim Burners Lee designed in the 1990s 1991

very simple 

there are other versions of http that has come out since then 

there have been various degrees of success with the new versions

2024 data -30 % of requests

##### HTTP 1.1
____
HTTP 1.1 allowed multiple requests and multiple responses to be sent

the client sent a request to the server and time passed and  the client looks at request and then sends another response

most of the time the user spends is sending messages not doing computations

fix by allowing more parallelism from client and the server

routers get faster but not that much faster


HTTP 2
___
2015

2024 data -50 % of requests

better transport efficiency

both HTTP  and 2 are built atop tcp

if you lose a packet tcp stops and then resumes after finding the packet again gives jitter
###### HTTP2 unique features:
___
- pipelining: client send out several requests before receiving a response
	does not have to necessarily come back in the same order. The requests need tags and the responses need tags in the header to explain how the messages work. The server can ship the smaller responses faster and the larger responses later.
- multiplexing: can do two conversations at the same time, have two different tabs on your web browser communicating with the same web server
- compression of headers 
difference to HTTP1 the headers have to be ascii text, the headers are longer on HTTP2
gzip used on header to compress it in HTTP2
- server push: In HTTP1 the server never responds to the client unless the client asks it a question makes diffcult to set up situation where something important is about to happen
		client has to keep asking the server every time, keep pulling increases network traffic
		part of protocol allows server to send to client, server can reply multiple times for each request with ids on every request
	
### HTTP 3
___
2022
2024 data - 20 % of requests

HTTP3 is mostly about the problem of latency

HTTP3 is no longer based on TCP is based on UDP

UDP is simpler protocol where you send a packet and it supports unreliable streams

own substitute for UDP called QUIC invented by Google in 2012 to make chrome run faster 

standardized technology

avoid head of line blocking delays that are inherent to tcp

make situation of data not being visible up to application to decide whether packets are needed to display a webpage useful for zoom and applications that need good real time performance

if the app requires http you can only talk to 20 % of the browser

javascript might have to fall back on older http version even if browser supports it the website might not

many fallback decisions

two different versions of http for browsers in the past still happens with http2 and http3

### HTML
___
based on sgml

#### sgml
___
author 1, author 2... author n

where writing scientific papers

we also had different publishers publishing books

each publisher had their own way of doing documentation, Microsoft word wasn't good enough for most publishers

publishers had different ways, or different markup language to publish papers and books

SGML: Standard generalized markup language

common markup language in effect a super set of all publishing languages, hypertext

translate between all publisher formats

print in journals from SGML

pioneered by IBM, did not want to support each one independently

trying to get programs to be compatitble accross all publishers not possible

was declarative not imperative 

not a programming language 

imperative : print 3 at a location -9621.3

declarative:
`<superscript>h <superscript>`
gave ways to implement certain things, common markup language, avoid over specifying what one wants

do not want programs want data

setup a syntax involving angle brackets

`<quote> Hello. <quote>` was totally up the publisher how it works

focus on content not form, now doesn't have to worry about creating different versions of each paper for each publisher

each publisher had its own niches and own strengths

some of them could support some features and some couldn't support other features

used document type definitons

#### DTD
___
specificies exactly which of these elements are supported in your document and which are not supported

whether they are allowed to nest in eachother and what feature are allowd

the document type description would support what can be supported amongst these guys and authors and publishers could see what they could publish and how

### back to html
___
created DTD for html 

some things that did not work very well for the web

did not want to fit into the technology as could not work exactly

share documents on the web instead of among traditional publishers

html element represents a node in a document tree

the root of the tree is an html element that has a head and a body

there will be other stuff underneath that

there will be an html element and at the end it will be marked at the end with the element's tag again

the body will be inside this tag

very straight relationship between the text file and the document, easy vice versa

not every text file is valid in HTML but every HTML file is a valid text file

#### tags
___
mark end and beginning of an element

tend to be relatively small

elements are surrounded and delimited by tags

can omit closing tags
- person reading text will know where body ends and begins not always optional

when starting recommends to write all the closing tags 

`<br/>` is an element where closing and opening tag are the same thing equivalent to  `<br></br> same as <br>

#### have attributes 
____

each element can have an attribute for example

`<br align=right>` to give the tab an attribute 

can put several different attributes in an element

each node can have a set of name value pairs to tell information about a node and how to display a node

Eggert uses Mozzilla documentation network to know what elements are allowed


XML
___
extensible markup language

the basic idea is pretty simple that stands for a tree

what XML does is it standardizes the syntax of HTML

without standardizing the actual particular names, or 'head' of html

no mention of head or HTML

standardize exchange of any tree structure data where the nodes have attributes and the leaves are text or entities

was an attempt to do HTML all over again as XHTML but this did not work because HTML kept evolving 

originally HTML came out in different standards

HTML 4.01 had a DTD but nowadays HTML 5 whatever the HTML website says it means a sort of living standard

the living standard maintains a history

an advantage of html 5 is it has elements that look like this `<video> <audio>`

### HTML entity
___
either some ordinary text, some entities that are introduced with `&arrow or something like &#917`

`&amp ='&'`

`&lt;   <                     &gt;    >`
giving basic definitions for entities

The reason chrome will know is it knows the syntax for HTML 5

really intended for browsers

### Document object model (DOM)
___
tree and you want to write programs that deal with this tree
the tree will be reasonably large several paragraphs in the solution very bushy
DOM models the trees, how you access and modify the tree
easy way to refer to elements of the tree
`find the last paragraph in the tree'`
might have to tree traversal 
do in simple API
also want to traverse the tree
want to walk through the entire tree and insert hello at the beginning if every paragraph

DOM gives way of 
 - updating tree
 - traversing trees
gives APIS for doing the above

most commonly used language is Javascript
also available in other languages

can also go in and change attributes of an element

### cascading style sheets CSS
___
expand on the original goal of SGML and HTML

concentrate on content and not on form

keep that going 

goal: clean separation between form and content and we want both form and content to be declaritive

specify the file of the document specifically enough so can satisfy style contents

CSS talks about form

attach attributes to elements in your DOM, some of these attributws will be CSS relevant

called cascading style sheets

attributes tie on in the tree cascade down

the process for determining the style of a particular entity is more complicated that specifically looking it up

styles are inherited in DOM tree

you can get styles from: an element itself, ancestor of that element, usr specified style

the user of the browser can specify a font for example

the browser could have some specifics hardwired due to display problems

author can also specify

inline 

`<span style="font-variant: small-caps"> ucla </span>
`
The browser is supposed to use smaller caps than usual

the css spec will tell you how to resolve but it has its limits

the limits come from this design decision

lets the browser decide what it should do 

### Javascript
___
YAPL Python but c syntax, yet another programming language

can hook into HTML

there are ways to make the code not communicate with the server and run the code quicker

`<script> alert("Hello.js)"</script>`

this is an inline function single trip

script src = "hello.js"

this makes you access the server

the program that we are going to run 

will compile and run that code in this point of display of webpage gives more freedom allows for dynamic webpages over static webpages, adding element such as animations

use javascript to access the DOM for a webpage typically the code will do something more complex access the DOM, single bit of javascript code working behavior depends on the DOM that it sees

modify the DOM, modifies the webpage

inserting new script elements which contain new bits of code

an html document with javascript can be an example of a self modifying program

a program that changes its source code as it is running

this is done a lot

people angry at writing that type of code

create new part of webpage has to write a long string of commands to element to add a certain element such as `<br>` and an attribute `attribute(e,'color','blue')`

the code becomes hard to read is hard to develop

JSX
___
we want the capability to modify the DOM

const class = 'cs35L', const n = 4
const header = `<h1 lang= 'en {class} assign {n} h1>;`

const header = ____madeat('h1')

header ......... addatr('lang','en')

inside JSX it replaces the functions and executes in javascript in a more verbose way

produces a react element that you can use to fiddle with the DOM

ReactDOM.render('header', documentofElementByID('root'))

gives a lot of power but it is a lot more painful

may discover the code you thought would run does not run when you thought it would

### browser rendering pipeline
___
the basic idea is that browser starts loading a webpage over the network but the user is impatient

faster than this simple model
let us not read the entire webpage before we start displaying

the browser starts displaying before the entire webpage is read

`<html>... <head> ... </head> <body> <p> "Hello"`

knows there is a hello in there somewhere and puts on the screen does it incrementaly

instead of all at once

#### optimizations
___
An element will not be placed on the screen if there is a cutoff

does't even look at the others 

if element happens to be a script element the script does not run because it is not on the screen

priority can be set multiple factors

can execute element in priority decided by browser

the browser can also try to guess the whole layout and start rendering based on that guess and then update later when it finds the guess is wrong

##### example
- rendering a table
		tables do not list dimensions in advance the browser will have to guess
		will start filling out table and resize table making it smaller or larger
if application is high performance user isn't annoyed by table resizing or other pipeline optimizations

occasitionally runs javascript code, may not run in order one expected, one has to take into account if code is run or how it is run certain situations

optimizations depend on browser cannot force it to execute code

### JSON
___
JavaScript object notation

a very simple subset of Javascript

all you are doing in Json is constructing an object

`{"menu : { "id : "file", "value": "file", popup:{}}}`

competitor to XML

taking a tree data structure and converting to text

textual representation of a tree

JSON is more common then XML for transmitting trees

take a tag and represent a DOM in JSON

Node.JS
___
 a runtime written in javascript for asynchronous events

event driven programming

your top level program is not something that you write it is supplied by the system

sitting in the node library

while(true):
	e = getNextEvent();
	 handleEvent(e);


what you write is merely a sequence of event handles for all types of events

the arguments to your event handler will be information about the event

whatever you do in an event handler you must finish quickly

one way to think about this style of programming is it is a completly different architecture from the original C++ program

in event handling you are not in charge, directly of the events, only in charge indirectly

not in charge of the top level, forces to divide up differently divide into byte size chunks or else program will not work

# Feburary 3rd
___
Node: async runtime for javascript

wheile (true){
	e = optEvent()
		handleEvent(e);
}

multi threading is multiple Cpus running at the same time which share the same ram and run the same program

cpus can communicate with each other by loading from ram and storing to ram

if program 1 while running wants to send a message to program 2 it works with multi threading

with 4 cores you can solve a problem 4 times faster when organized correclty so nothing stepping on each other's toes

disaster of communication

two often to have bugs where they step on the same ground at the same time

race condition are the baine of multithreaded applications

do not do this race conditions

events handling brings a part of the solution to the problem

may have multiple things you want to do

with battery in embedded system monitor the battery

getting server responses

if you have an application that has many things to do like this put each thing you want to do in a separate process or separate CPU

if this is kind of application you want to make you can use event handling

each of the parts of your program can be thought of as a sequential program like in CS 31

write CS 31 code and then concatenate

split up each thread of execution into little pieces

the upside from a reliability point of view is that each handler runs by itself

each handler when its running has complete control of the system

event handler code is more reliable than multi threaded code

performance problem

each core is working parallel getting worked on

this model is called single threaded

only one event handler running at a given time

it means no longer running in parallel

other cores do not help

lose parallelism and gain reliability

this requires locks in order for this communication to work

to scale add cores(CPUs)

if you keep adding more and more cores

you will run out of what you can fit on a chip

the path to ram becomes a bottleneck their is a limit

to scale: add entire new computers, you network, create another server

each server is running an independent copy of node

tends to scale better with web services

event driven programming is not just node, you can see it in Node.JS

you can see it in Python asyncio, Python frameworks like Twisted, Ruby event machine

you can use as a web server, you can write anything in node as a web server

____
### Webserver

as you try to scale web server needs to deal with lots of user requests and requests from other servers

event driven programming allows to see how new actions happen from the outside world

because node is written in javascript it means the javascript version which is an immense hasle with web browsers javascript version issues are less

- you are in charge of the server you can specify which javascript engine you want the browser to use
- fewer portable problems with javascript that runs on the server side
- simplifies browser usage, done on server side
leads into an issue

### issue:
___
if you are building a client server application you have to run some sever on the client and the server side

sometimes you have code that can run on either and you have to decide

where to draw the line with code on client on server

Node.JS gives some freedom for where to put the code

code can be pushed around to different places many design decisions to make

____
another issue is which libraries do you use or packages 

a lot of application in this area, what you r application is gluing existing code similar to writing from scratch

what packages will you rely on the server side and client side

file called app.js

in that file you can have a bunch of Javascript code

let's make `const = http require('http')
`const ipaddr = '127.88`
`const port = 3000`


____
packets have a header and a body

in that header, sender ip address, recipient ip address, another number which is a port number which in the range 0 2^16 -1

discriminating against all the packets on the server

`const server = http.createServer(request, response)=>{response.statisCode = 200
`response.setHeader = ('Content-Type',` 
`'textplain'`
`response.end('This is a just another server'))}`

`server.listen(port, ipaddr () => {console.log('Server at http:/${ipaddr}:{port})})`

key idea here is that event handler get called when event starts up

in order to understand how node works know when each event handler runs

yes you could for example run multiple web servers in same javascript instance

each running on for say different port

quasi parallel but not true parallel

in low load environment can be load not in high load



#### callback or lambda expression
___
nameless function does a certain action

passes response object and top level event handler and calls this function when that events comes in 

the function fills in the response and takes the components specified and decides what response to send back


bytes of the response

Header

ContentType.textplain

empty lines

This is just a toy server


___
# high load environment

figure out how much one can handle with a single core, different process on same machine

or start on new machine and farm out request to new web server

one technique round robin DNS

whenever you ask the internet what is the location of www.cs.ucla.edu

the answer will come back with different numbers or different requests

multiple IP addresses

192.54.239.12
197.40.207.19
197.40.27.19

___
### Node.JS packages

versioned in certain node

maintained by people other than the node developers themselves

many by 3 rd party packages

will run into versioning issues

you may find that you have pulled in a 3rd party package that other people rely on but you nee

issues with managing packages

one of the most common commands you will use is npm (Node Package manager)

NPM tracks which packages you are using, feed packages into NPM to use

important part of NPM is called package.json

- a file in JSON format, tree of data that represents packages you need for application
- metadata about the project, including the author and other data
- also contains information about packages and apps your project needs
- one area called dependencies, packages your project needs to run
- dev dependencies your app needs to develop
- dev dependencies taken together when developing all that matters when running

when ship to other people they do not need all these developer tools

 - also contains version control informatin
-  exactly how they get the source code for this file
- when lisiting dependencies you need to list other packages and their names
- includes their version numbers
- simplest form, say what package you want say version k 19.7 or package L 10.3
NPMs job to take the package.json and make it happen

pulls packages off the web and local packages


### npm commands
___

npm init

npm install module

npm install just install everything and get it up to date

another thing you can do npm install --save Module

record the fact you need this module into the package.json folder

you can also edit package.json directly with a text editor if screw up npm breaks

when you first start using node is thinking of this as overhead

this file is a big deal you have to get it right or nothing works

treat with respect just as much as the source code

version control source code should be under source control

### how versions work
___

version 5.2.7 < 5.2.10 those three numbers 

read numbers left to right not as strings but as a comparison of each digit

has to do with package comptability

end value

- if the new version of your package just fixes bugs

middle value

- added features is incremented to the second value and set bug fix number to 0 or 1

first value
- serious upgrades and withdrawing and changing the meaning of old features


first value can make things not work anymore as they could be old features

___

careful about the practice not always clear in packages, not always the same as the theory

in the old days in python adding 1 to integer would overflow

when python made that change of unbounded length integers 

some judgement category to what the package change is

significant compatibility with users when changing certain features, expected affect on downstream users

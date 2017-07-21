===========environment=================

turn syntax on
:syntax on

To display line numbers along the left side of a window, type any one of the following command while using text editor:
:set number

or
:set nu

To turn off line number again enter the same command:
:set nu!

To turn off vims indenting when pasting 
:set paste

=======================add a word to spell file==================

Type this over the highlighted word
zg

For Vim to suggest a list of alternatives that it thinks may be correct type:
z=

==========navigation==================
G
To move to end of file just type G (press ESC and type capital G):

1G
OR
gg
You can jump back to beginning of file by typing any one of the following command

700G
You can jump to line number 700 (press ESC type 700G)

w 
Move forward to the beginning of a word.
3w 
Move forward three words.
b 
Move backward to the beginning of a word.
3b 
Move backward three words.
$ 
Move to the end of the line.
0 
Move to the beginning of the line.
^ 
Move to the first non-blank character of the line.
======================edittig==========================
dG
delete everything after coursor

==========searching====================

The :substitute command searches for a text pattern, and replaces it with a text string. 

:%s/foo/bar/g
Find each occurrence of 'foo' (in all lines), and replace it with 'bar'.
:s/foo/bar/g
Find each occurrence of 'foo' (in the current line only), and replace it with 'bar'.
:%s/foo/bar/gc
Change each 'foo' to 'bar', but ask for confirmation first.
:%s/\<foo\>/bar/gc
Change only whole words exactly matching 'foo' to 'bar'; ask for confirmation.
:%s/foo/bar/gci
Change each 'foo' (case insensitive) to 'bar'; ask for confirmation.
This may be wanted after using :set noignorecase to make searches case sensitive (the default).
:%s/foo/bar/gcI
Change each 'foo' (case sensitive) to 'bar'; ask for confirmation.
This may be wanted after using :set ignorecase to make searches case insensitive.

------------alternative syntax---------------
For url's you can use a different deliminator

#','
:%s,foo/bar/baz,foo/bar/boz,g 

OR

#':'
%s:old/old:/old/new:g

==========undo=================================================

u: undo last change (can be repeated to undo preceding commands)
Ctrl-R: Redo 

=========================REGISTERS================================

I was pleased when I discovered the 0 register. If you yank text without assigning it to a particular register, 
then it will be assigned to the 0 register, as well as being saved in the default " register. 
The difference between the 0 and " registers is that 0 is only populated with yanked text,
whereas the default register is also populated with text deleted using d/D/x/X/c/C/s/S commands.

I find this useful when I want to copy some text, delete something and replace it with the copied text. 
The following steps illustrate an example:

To access a register user the character " so th efirst register is accessed by ""
Yank the text you want to copy with y - this text is saved in " and 0 registers
Delete the text you want to replace with d - this text is saved in " register
Paste the yanked text with "0p


There are nine types of registers:
1. The unnamed register ""
2. 10 numbered registers "0 to "9
3. The small delete register "-
4. 26 named registers "a to "z or "A to "Z
5. four read-only registers ":, "., "% and "#
6. the expression register "=
7. The selection and drop registers "*, "+ and "~ 
8. The black hole register "_
9. Last search pattern register "/


Most used registers
-------------------
1. Unnamed register ""
Vim fills this register with text deleted with the "d", "c", "s", "x" commands
or copied with the yank "y" command,

2. Numbered registers "0 to "9
Vim fills these registers with text from yank and delete commands.
Numbered register 0 contains the text from the most recent yank command,
Numbered register 1 contains the text deleted by the most recent delete or change command
With each successive deletion or change, Vim shifts the previous contents of register 1 into register 2,
2 into 3, and so forth, losing the previous contents of register 9.



4. Named registers "a to "z or "A to "Z		
Vim fills these registers only when you say so.

When I'm working I sometimes find myself rearranging code, but I don't want to lose it and may want
to paste it multiple times. This is the perfect time to use a named register to store chunks of code.

Typing "ayy will yank the current line into register a. 
Typing "ap will put it somewhere else.

5. Read-only registers ":, "., "% and "#
The . register stores what I just typed
"%	Contains the name of the current file.
"#	Contains the name of the alternate file
":	Contains the most recent executed command-line

7. Selection and drop registers "*, "+ and "~ 
The * register is my system clipboard

The read-only "~ register stores the dropped text from the last drag'n'drop
operation.

Viminfo
-------
What's even more amazing about registers is they're stored in ~/.viminfo.
That means when Vim has been quit and then loaded again, all of the populated registers will still be available.

Acessing in insert mode
-----------------------
ctrl+r let go then follwed by the register lets you paste the contents of a register without leaving insert mode.

Ctrl + r then " Put from the default register

Ctrl + r then d Put from register d

Ctrl + r then 0 Put from register 0



===================ADVANCED=======================

You start recording by q<letter> and you can end it by typing q again.

Recording is a really useful feature of Vim.

It records everything you type. You can then replay it simply by typing @<letter>. 
Record search, movement, replacement...

--------------------macros------------------------

Let's say you are dealing with a tab-delimited value file as such:

    ID  Df  %Dev    Lambda
    1   0   0.000000    0.313682
    2   1   0.023113    0.304332
    3   1   0.044869    0.295261
    4   1   0.065347    0.286460
    5   1   0.084623    0.277922
    6   1   0.102767    0.269638
    7   1   0.119845    0.261601
Now you decide that you need to add a percentage sign at the end of the %Dev field.
We'll make a simple macro in the (arbitrarily selected) m register as follows:
[** Make sure you are in escaped mode and just type qm]

qm

EE: Go to the end of the 3rd column.

a: Insert mode to append to the end of this column.

%: Type the percent sign we want to add.

<ESC>: Get back into command mode.

j0: Go to beginning of next line.

press q in escaped mode to Stop recording macro

We can now just type @m to run this macro on the current line. Furthermore, we can type 100@m to do this 100 times! Life's looking pretty good.

At this point you should be saying, "BUT WAIT, WHAT THE HECK DOES THIS HAVE TO DO WITH REGISTERS?"

Excellent point. Let's investigate what is in the contents of the m register by typing "mp. We then get the following:

    EEa%<ESC>j0
At first this looks like you accidentally opened a binary file in notepad, but upon second glance, it's the exact sequence of characters in our macro!

You are a curious person, so let's do something interesting and edit this line of text to insert a "!" instead of boring old "%".

    EEa!<ESC>j0
Then let's yank this into the n register by typing B"nyE. Then, just for kicks, let's run the n macro on a line of our data using @n....

OMG, IT ADDED A "!"

Essentially, running a macro is like pressing the exact sequence of keys in that macro's register. If that isn't a cool register trick, I'll eat my hat.


================A neat macro trick=============
I want to paste a string that contains a number like "value1". 

Each time I paste I want to increment the number. 
The ultimate goal is something like 50p to do this 50 times. 
(value1\nvalue2\nvalue3...)


SOLUTION:


:let i=1 
qm 
ivalue<Ctrl-R>=i<Enter><Enter><Escape> 
:let i+= 1 
q

================ Now for some real crazy vim ========


so i have 100 json objects and copy pasted from one object, now i want to change the name from a list of random names
i will only show the first 3


```
{
    "model": "recons.cashier",
    "pk": 11,
    "fields": {
        "name": "Tim",     # <-------     START
        "surname":"Cythia",
        "employee_number": "1000",
        "id_number": "89242545211",
        "site": 1
    }
},
{
    "model": "recons.cashier",
    "pk": 11,
    "fields": {
        "name": "Tim",
        "surname":"Cythia",
        "employee_number": "1000",
        "id_number": "89242545211",
        "site": 1
    }
},
{
    "model": "recons.cashier",
    "pk": 11,
    "fields": {
        "name": "Tim",
        "surname":"Cythia",
        "employee_number": "1000",
        "id_number": "89242545211",
        "site": 1
    }
}
```


So lets take our cursor to the start, but before we do anything we have to define our variables so go into command mode

:let i=0
:let l=['Shena', 'Laronda', 'Diann']

now lets record our macro into the m register (remember be in normal 'escaped' mode)

qm
shift-^  # go to the start of the line
f <space> # this will find the first white space now 
dw  # delete the name basically
dw
x
"" # enter the two "" where you want to paste the new name in and put your cursor in the middle
<Ctrl-R>=l[i]<Enter><Enter><Escape>  # Now we paste from the command line using ctrl-r, Note we are indexing the list using l[i] just like in python

:let i+= 1  # we increment i for the next use
11j    # very important part move 11 lines down to the next name that needs replacing
q     # stop recording the macro


now prepare to be amazed and type @m on the line you are on and on the next neat!



===============  Vim 101: CTRL-R ============

Inserting Registers in Insert Mode

When writing in insert mode, it’s possible to paste a register at the current location without leaving insert mode. To do this, press CTRL-R then type the name of a register. For example, CTRL-R a will insert the contents of a.
The text is inserted as if it were typed: that means autoindent will be respected. Pressing CTRL-R twice will insert the text literally.

Expression Register

Typing CTRL-R = allows an expression to be entered -- press return to insert the result. This could be any expression, but an easy to learn trick is to use it for simple mathematics.

Let’s say I wanted to insert the number of milliseconds in a day. Rather than leaving Vim and using a calculator or REPL, I could type CTRL-R =1000 * 60 * 60 * 24 and "86400000" will be inserted at the current position.

Command Mode

Typing CTRL-R in command mode (not normal mode) will also insert the contents of a register.

Further Reading

For more on using CTRL-R in insert mode, see :help i_CTRL-R. The command mode equivalent is also documented in :help c_CTRL-R.



===============   MULTIPLE WINDOWS  ==============

### File Explorer ###
:E  # Open File explorer :q will close vim. 
:bd # (delete current buffer) To close only the file explorer int the scenario above

:Texplore   # tabbed full screen explorer, typing :q will always close that tab, but not vim.


### Multiple Tabs ###

:tab split            # copy the current window to a new tab of its own
:tabs                 # list all tabs including their displayed windows
:tabclose             # close current tab
:tabclose {i}         # close i-th tab
:tabonly              # close all other tabs (show only the current tab)
1gt                   # go to tab 1
gt                    # go to next tab
gT                    # go to previous tab

### Multiple windows in a Tab ###

CTRL-w c   # Close current window,  If that window is the last window visible in a tab, the tab is also closed 
CTRL-W s   # Open horizontal split window
CTRL-W v   # Open vertical split window
CTRL-w T   # Move current window to a new tab

:n new_file.txt              # create a new file inside the current window.
:new new_file.txt            # New file in horizontal split
:vnew  new_file.txt          # New file in vertical split
CTRL-w n                     # to create a new file inside a horizontal split

:hide            # hide current window (where does it go?)
:close           # close current window
:only            # keep only this window open
:ls              # show current buffers
:b 2             # open buffer #2 in this window
 
 
### Switching windows ###

CTRL-w h = Switch to the window to the left
CTRL-w j = Switch to the window below
CTRL-w k = Switch to the window above
CTRL-w l = Switch to the window to the right

#### Resizing ###

ctrl-w _  # maximizes a windown
ctrl-w =  # restores a window

ctl-w 'then' -  # reduces current window by on line horizantally
ctl-w 'then' +  # encreases current window by one line horizontally


CTRL-w =            # Resize the windows equally

CTRL-w >            # Incrementally increase the window to the right
Takes a parameter, e.g. CTRL-w 20 >

CTRL-w <            # Incrementally increase the window to the left
Takes a parameter, e.g. CTRL-w 20 <

CTRL-w -            # Incrementally decrease the window's height
Takes a parameter, e.g. CTRL-w 10 -

CTRL-w +            # Incrementally increase the window's height
Takes a parameter, e.g. CTRL-w 10 +


=========== SAVING SESSIONS ========

# To save a session

:mksession ~/mysession.vim

# Then later you can source that vim file and you'll have your old session back:

:source ~/mysession.vim


# or open vim with the -S option:

$ vim -S ~/mysession.vim






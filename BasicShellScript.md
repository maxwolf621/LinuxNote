# BASIC Shell Script

[TOC]

[basic shell script](https://blog.techbridge.cc/2019/11/15/linux-shell-script-tutorial/)

###### tags: `Linux`

using `;` to put the Commands in one line in the terminal;
```bash
$ date ; who
```

For advoiding the troublesome to type the commands in the terminal every time instead we can write a shell script  
First step create a script
```bash
$ vim script.sh
```

```bash 
#!/bin/bash
# This scirpt display the date and who's logged on
date
who
```
- To tell the system which shell can execute this shell script we need to add `#!/bin/bash` in the first line 
- `#` is a comment


Using PATH env helps SHELL finding the command 
```bash
#echo $PATH
```

Some linux systems put $HOME/bin into PATH enviroment, it supplys every user a file that SHELL can find the command that to be executed in the /HOME/.. directory

set up the priority and executable of the file after creating the script
```bash
chmod u+x test1
```


#### `$`
In linux `$abc` presents abc is a variable, but if we want to show up `$15` dollars , add `\` before `$numbers`
```bash
echo "it cost \$15 usa dollars"
printf "%s\n" "it cost \$15 usa dollar"
```

#### Declaration of local variable in the script `variable=value`
for example
```bash
var1=10
var2=-57
var3=testing
var4="more than one words"
```

#### Operator `varibale` and $(variable)

By giving command `date` to a local variable
```bash
# assign `replace` to present the command `date`
replace=$(date) 
echo "The date and time in the system is :" $replace

# copy the /usr/bin directory listing to a log file
# format as %y%m%d
today=$(date+%y%m%d)
ls /usr/bin -al > log.$today
```

## to execute the shell script via `./shellname.sh` in the terminal
```bash
./test1.sh
```
- this creates a child shell and the child shell cant use the variable in the script from father shell


## RE-define ouput and input

To GIVE the output of `command` to other script in the specific file using `>`  
sytanx
```bash
command > outputfile
```

if the script test6 doesn't exist then create one else it would cover the old one
```bash
pi@Ji$ date > test6
pi@Ji$ ls - l test6
-rw-r--r-- 1 user user 29 Feb 10 17:56 test6
pi@Ji$ cat test6
Thu Feb 10 17:56:58 EDT 2014
```

#### using `>>` to avoid ==overwriting(überschriben)==
```bash
$who >> test6
$cat test6
Thu Feb 10 18:02:14 EDT 2014 #this is date > test6
user pts/0 Feb 10 17:55      #this is who >> test6
```

#### `<` redirect backward 
sytanx
```bash
command < inputfile
```
using redirec backward to find out the information of inputfile via command  
for example  
```bash
date < inputfile
```
- To show up inpufile date and time


## piping

Without piping..
```bash
user$ rpm -qa > rmp.list # store the command `rpm -qa` outut information into `rmp.list`
user$ sort < rmp.list    # get information of `rmp.list` that would display sorted by order of the character
```
- `-qa` to show up installed package list
- `sort` to help rmp.list sort by order of character

Now we can use `piping` to make it simpler 
```bash
command1 | command2
```

To simplize the above commands
```bash
$ rpm -qa | sort
$ -qa | sort | more         #with pages
$ rmp -qa| sort > rmp.list  # To output (store) the information to the file rmp.list
$ more rmp.list
```

## Mathmatical Operator
#### command `expr`
```bash
a1 | a2 
a1 & a2 
a1 < a2
a1 % a2
STRING : REGEXP     # if STRING and REGEXP match
match STRING REGEXP # if STRING and REGECP match
substr STRING POS LENGTH # a LENGTH of substr in STRING
index STRING CHARS #find char in the string
length STRING # LENGTH of STRING
+ TOKEN # keyword
(EXPRESSION) # return value of EXPRESSION
```

### ERROR of `*` 
To handle such problem add `\` bfore `*`
```bash
$expr 5 * 2
expr: syntax error: unexpected argument \u201e0_rUDkeFO-5GS-t5Vb.png\u201c
$expr 5 \* 2
10
```

## status of Execution (Ausführung)

if the command return 0 => then it quicks successfully else return postive nums => it fails
for example 
```cpp
$asdf
bash: sasdf: Kommando nicht gefunden.
$echo $?
127
$
```

0 : successful  
1 : unkonwn ERROR  
2 : not a shell command  
126: the command cant not be executed  
127: I cant not find the command   
128: Unvalid quit argument 
228+x : linux has fatal error related with signal x  
130: exit by ctrl+c  

You can make your own exit code of the shell script
```bash
#!/bin/bash
...
...
exit 5
```
- or using `exit $variable`
- The range of exit code number is btw 0 ~ 255 so if there is exit 300 it would actually means 256-300=44 

## if then 

sytanx 
```bash
if command
then
  commands
fi
```

```
$ cat test3.sh 
#!/bin/bash 
#testing multiple commands in the then section 
# 
testuser=Christine 
# 
if grep $testuser /etc/passwd 
then 
  echo "This is my first command" 
  echo "This is my second command" 
  echo "I can even put in other commands besides echo:"
  ls -a /home/$testuser/.b* 
fi
$
```
- if Christine is online then ...


## if ... then ... else ...

```bash
if command 
then 
  commands 
else 
  commands 
fi
```

## nested if .. then .. else

```bash
$ ls -d /home/NoSuchUser/ 
/home/NoSuchUser/ 
$ 
$ cat test5.sh 
#!/bin/bash 
#Testing nested ifs 
# 
testuser=NoSuchUser 
#if grep $testuser /etc/passwd 
then 
  echo "The user $testuser  exists on this system." 
else 
  echo "The user $testuser does not exist on this system." ​
  if ls -d /home/$testuser/ 
  then echo "However, $testuser has a directory." 
  fi 
fi
```

you can also use if .. then ..  elif

```bash
if command1 
then 
  commands 
elif 
  command2 
then 
  more commands 
fi
```


## test command 
sytanx
```bash
test condition
```

### if then with test

```bash
variable=""
if test $variable
then
  ...
else
  ...
fi
```

or `[]` instead of `test`  
`[` and `]` must seprate with space between `condition` as the following  
```bash
if[ condition ]
then
  command
fi
```

What commad `test` can handle?
1. compare values (only integer)
2. compare strings (!=,<,>,-n,-z)
3. compare files

Operator
n1 -eq n2
n1 -ge n2
n1 -gt n2
n1 -le n2
n1 -lt n2
n1 -ne n2

```bash
#!/bin/bash
value1=10
balue2=11
if [ $value1 -eq $value2 ]
then 
  ...
else
  ...
fi
```



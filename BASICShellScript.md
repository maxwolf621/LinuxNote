# BASIC Shell Script

[TOC]


###### tags: `Linux`

Two Commandline together in the terminal;
```bash
$ data ; who
```
Using shell script to customize them
```bash
$ vim script.txt
```
firstline of shell script always with 
```
#!/bin/bash
```
- `#` is a comment

```bash
#!/bin/bash
# This scirpt display the date and who's logged on
date
who
```

Using PATH env helps SHELL finding the command 
```bash
#echo $PATH
```

Some linux systems put $HOME/bin into PATH enviroment, it supplys every user a file that SHELL can find the command that to be executed in the /HOME/.. directory

After creating the script, 

Do remember to set up the priority using
```bash
chmod u+x test1
```


`$` presents a variable

but if you want to show up $15 dollars 
```
echo "it cost \$15 usa dollars"
```

example declaration of local variable in the script
```bash
var1=10
var2=-57
var3=testing
var4="more than one words"
```

operator `varibale` and $(variable)
By giving command `date` to a local variable
```bash
# using replace to present the command date
replace=$(date) 
echo "The date and time in the system is :" $replace
# copy the /usr/bin directory listing to a log file
# format as %y%m%d
today=$(date+%y%m%d)
ls /usr/bin -al > log.$today
```


## `./` in the terminal

```bash
./test1.sh
```
this would create a child shell
and the child shell cant use the variable in the script from father shell


## RE-define ouput and input

To GIVE the output of command to other script in the directory  using `>`

sytanx
```bash
command > outputfile
```

if test 6 doesn't exist then create one
else it would cover the old one
```
$ date > test6
$ ls - l test6
-rw-r--r-- 1 user user 29 Feb 10 17:56 test6
$cat test6
Thu Feb 10 17:56:58 EDT 2014
$
```
- By writing the output of the command to script test6
using `>>` to avoid ==covering==
 ```bash
$who>>test6
$cat test6
Thu Feb 10 18:02:14 EDT 2014
user pts/0 Feb 10 17:55
```



`<` redirect backward 
sytanx
```bash
command < inputfile
```
- The command would execute it's function to help inputfile
such as
```bash
date < inputfile
```
- To show up inpufile date and time


## piping


Without piping..
```bash
$rpm -qa > rmp.list
$sort < rmp.list
[..]
```
- `-qa` to show up installed package list
- `sort` to help rmp.list by order of character

Now we can use piping to make it simpler 
```bash
command1 | command2
```

To simplize the above commands
```bash
$rpm -qa | sort
#with pages
$prm -qa | sort | more
# To output (store) the information to the file
$ rmp -qa| sort > rmp.list
$ more rmp.list
```


## Mathmatical Operator


### command `expr`

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

### ERROR

```bash
$expr 5*2
expr : sytanx error
$expr 5\*2
10
$
```



## quit the script

if a the command return 0 => then it quicks successfully
else return postive nums => it fails
```cpp
$asdf
-bash :asdf: command not found
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


You can make your own exit code
```bash
#!/bin/bash
...
...
exit 5
```
or using `exit $variable`


The range of exit number is btw 0 ~ 255
so if there is exit 300 it would actually means 256-300=44 


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


## if then else

```bash
if command 
then 
  commands 
else 
  commands 
fi
```

## nested if then else

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
  echo "The user $testuser exists on this system." 
else 
  echo "The user $testuser does not exist on this system." 
  if ls -d /home/$testuser/ 
  then echo "However, $testuser has a directory." 
  fi 
fi
```

you can also use elif

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

```

variable=""

if test $variable
then
  ...
else
  ...
fi
```
or to make a simpler
```bash
if[condition]
then
  command
fi
```

What `test` can handle?

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
if [$value1 -gt 5]
then
  ...
fi
#
if [$value1 -eq $value2]
then 
  ...
else
  ...
fi
```



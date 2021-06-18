# Environment Variable
###### tags: `Linux`

[export Path](https://askubuntu.com/questions/833922/what-does-the-export-path-line-in-bashrc-do)  
[soruceAndexport](https://askubuntu.com/questions/862236/source-vs-export-vs-export-ld-library-path)  
[exort meaning](https://askubuntu.com/questions/720678/what-does-export-path-somethingpath-mean)  

## `export` 
- It's a Bash builtin
The `export` means that the variable that you declare after **it will be accessible to child processes.**
In other words, processes will be able to access the variable declared after the `export` keyword through the shell's environment.  
For example  
`export FOO="BAR"` and then sourced the changes in the shell environment
now `echo $Foo` to get BAR

In short
`export` only means to make that PATH variable also available for other programs you run from bash.

## `PATH`
The PATH variable is an environment variable that contains an ordered list of paths that Linux will search for executables when running a command. 
Using these paths means that we do not have to specify an absolute path when running a command. 

**This lets you type custom commands for scripts without typing the full directory.**
- `PATH` is marked for `export` by default, so this line doesn't have to be rewritten. It doesn't hurt, though.

## `$HOME` in the PATH variable
At the beginning of the path that is assigned to the PATH variable, $HOME is declared. 
This means that the computer will pretty much grab the value stored in `HOME` and copy-paste it in front of the rest of the line when reading it.

## The : in between both paths
It just separates the three directories. 
- Without those three directories, the console would not recognize the commands it receives. 
Those three places are the three directories that are most commonly used for scripts/command files to be stored and therefore should be accessible by the terminal without having to write out the full path to the file.

For example
```bash
export PATH=$HOME/.local/bin:$HOME/.local/usr/bin:$PATH
```

 Is it exporting the data to be available for Bash?

`export` sets the environment variable on the left side of the assignment to the value on the right side of the assignment;   
such environment variable is visible to the process that sets it and to all the subprocesses spawned in the same environment,   
i.e. in this case to the Bash instance that sources ~/.profile and to all the subprocesses spawned in the same environment   
(which may include e.g. also other shells, which will in turn be able to access it).

### What is the first PATH and what is the second $PATH, and why do we need two?

The first PATH as explained above is the environment variable to be set using export.  
Since PATH normally contains something when `~/.profile` is sourced  
(by default it contains /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games), simply setting PATH to ~/.composer/vendor/bin would make PATH contain only ~/.composer/vendor/bin.

So since references to a variable in a command are replaced with (or "expanded" to) the variable's value by Bash at the time of the command's evaluation,   
:$PATH is put at the end of the value to be assigned to PATH so that PATH ends up containing ~/.composer/vendor/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games (i.e. what PATH contains already plus ~/.composer/vendor/bin: at the start).


## `source`

When you call a shell script the normal way (say with `./myscript.sh` or `sh myscript.sh`), **it will be executed in its own process context (a new process environment)**, so all variables set in the script will not be available in the calling shell.  
When executing a script using the source command, it will be executed within the context of the calling script.  
This way you are able to set environment variables by calling source myscript.


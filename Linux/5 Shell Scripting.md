# Shell Scripting

https://www.freecodecamp.org/news/shell-scripting-crash-course-how-to-write-bash-scripts-in-linux/

Shell scripting is an important part of process automation in Linux. Scripting helps you write a sequence of commands in a file and then execute them.



## Bash Shell

The Linux command line is provided by a program called the shell.

The default shell for many Linux distros is the GNU Bourne-Again Shell (bash). 

When you first launch the shell, it uses a startup script located in the `.bashrc` or `.bash_profile` file which allows you to customize the behavior of the shell.



## Bash Script

A bash script is a series of commands written in a file. These are read and executed by the bash program. 

By naming conventions, bash scripts end with a `.sh`. However, bash scripts can run perfectly fine without the `sh` extension.

Scripts are also identified with a `shebang` as first line of the script. Shebang is a combination of `bash #` and `bang !` followed the the bash shell path. Shebang tells the shell to execute it via bash shell. 

```
#! /bin/bash
```



Executable scripts appear in a different colour from rest of the files and folders.

Scripts have execution rights for the user executing them. An execution right is represented by `x`. 



### Provide execution rights to your user.

Modify the file permissions and allow execution of the script by using the command below:

```bash
chmod u+x hello_world.sh
```

`chmod` modifies the existing rights of a file for a particular user. We are adding `+x` to user `u`.



Find the path to your bash shell.

```bash
which bash
```

and I will include this in the shebang.



### Run the script.

You can run the script in the following ways:

```
./hello_world.sh
```



### How to define variables

We can define a variable by using the syntax `variable_name=value`. To get the value of the variable, add `$` before the variable.

```
#!/bin/bash
# A simple variable example
greeting=Hello
name=Tux
echo $greeting $name
```


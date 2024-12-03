Linux and Mac OS systems have Unix Shells that provide us a command line interface. e.g. Bash Shells or Zsh 

Windows uses Power Shell by default, which has a different command language. If you are on Windows you will need a Bash shell installed for the following commands. 


Much of working on the command line involves navigating around your system without the use of a Graphical User Interface (GUI). Below we have a graphical representation of a folder hierarchy with a plain text .txt file:

![[directoryStructures.png]]

root/
├─ directoryname/
       ├─ anotherdirectory/
                  ├─ aFileName.txt

In reality when working on the command line your root folder for your user account on your system will looking something like below, the root folder is always denoted by a tilde `~` 

![[commandLineRoot.png]]
in this example the user is called "pubdoc" 


##### A few terms we need to be comfortable with before moving forward are:

- __directory__ : also known as a folder
- __root directory__ :  "highest" directory in the hierarchy. You can also think of it as the start of a particular folder structure,  denoted by a tilde `~` on the command line.
- __directory path__ :  in the example above, there would be a directory called "directoryname", which has a folder called "anotherdirectory" inside of it, the path would be: `/directoryname/anotherdirectory`
- __file__ : a file, e.g. file.txt or index.html 


### Basic Commands
The following commands will help us get started, but are just the tip of the [iceberg](https://www.geeksforgeeks.org/linux-commands/?ref=lbp)

Show the name of the currently logged in user:

```shell
whoami
```

Show the name of the machine is given on the network: 

```shell
hostname
```


`sudo` stands for [Super User DO](https://www.geeksforgeeks.org/sudo-command-in-linux-with-examples/). This command is used to allow you to run commands that only super users are allowed to run, and is often followed by the command you want to run. When you use `sudo` you will be prompted to enter your super user password to execute the command:

```shell
 sudo <command>
```

If you want to enter into sudo mode to run all commands as a super user, run:

```shell
 sudo su
```
(this will then prompt you to enter your password and after which you can run commands as a super user)




Print working directory that you are currently in:

```shell
pwd
```


Change directory to a directory using the "path" to that folder/directory:

```shell
cd /directoryname/anotherdirectory
```

Change directory to root :

```shell
cd 
```

Change directory to the directory above the one you are currently in:

```shell
cd ..
```


List current directory contents:

``` shell
ls
```



Make new directory. When working on the command line it is customary for anything that is variable, i.e. in the example below `mkdir` is our command, and you would then write the name of the folder:

 ```shell
mkdir directoryname
```

However, in instructions you might see something like this:

 ```shell
mkdir <directoryname>
```


Make new file:

```shell
touch <example.txt>
```


A quick way to print contents of a file to your terminal:

```shell
cat <example.txt>
```


Copy a text file called `example.txt` which is in a sub directory "exampleFolder" to a new location  `/path/to/folder`:

```shell
cp exampleFolder/example.txt /path/to/folder
```


Remove a file, be careful with removing files!

```shell
rm <file.txt>
```


Remove an empty directory:

```shell
rmdir <directoryname>
```

Remove a directory and all the items inside of it:

```shell
rm -r <directoryname>
```




###  Command Line Text Editors

##### Nano

Edit a file using [nano](https://www.nano-editor.org/) command line text editor:

``` shell
nano <example.txt>
```


-------
Simple quick start for editing a text file:

- You will see the nano editor open. At the bottom of the window, you will find some shortcuts to use with the Nano editor. The `^` (caret) means that you must press **CTRL** (Windows) or **control** (macOS) to use the chosen command.
- You should be able to edit and enter text in your file.
- Press **CTRL + O** to save the changes made in the file and continue editing.
- To exit from the editor, press **CTRL + X**. If there are changes, it will ask you whether to save them or not. Input **Y** for **Yes**, or **N** for **No**, then press **Enter**. If there are no changes, you will exit the editor right away.

[getting started with nano](https://itsfoss.com/nano-editor-guide/), dive deeper into [nano](https://www.nano-editor.org/) 

----------------

##### Vim

Edit a file using [vim](https://vimdoc.sourceforge.net/htmldoc/usr_01.html) command line text editor:

``` shell
vim <example.txt>
```

-------
Simple quick start for editing a text file: 

- Press `i` to insert, then you can make changes to your file. 
- Once you are finished changing your file press escape `esc` to exit out of insert mode. 
- Then you should type `:wq` and hit enter to write and quit (save your file and quit).
- The command to then exit Vim is `:q` 
 
vim is less beginner friendly, dive deeper into [vim](https://vimdoc.sourceforge.net/htmldoc/usr_01.html)
 
----------------





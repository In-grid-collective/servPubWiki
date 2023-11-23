---
theme: moon
---

<style>

    
	.tiny-font{
		font-size: 0.5em;
	}

	.markdown-preview-view code{
	         font-size: 0.5em;
	}
	
	pre{
	     font-size: 0.7em!important;
	}
	p{
		font-size: 0.8em!important;
	}
	code{
	     padding: 0.5em 1em!important;
	}

</style>




## Network Anatomy 


---


## Intro




---
physical & data link layer


---
public network layer


---
virtual private network


---
server software with Nginx


---
traffic 


---


## Local Server on the Pi

---
Local vs. Remote


---
SSH

To access the pi we will use ssh [[01-Set up Pi for local Network Access#Security via SSH Keys]].

---
## Terminal

Linux and Mac OS systems have Unix Shells that provide us a command line interface. e.g. Bash Shells or Zsh 

Windows uses Power Shell by default, which has a different command language. If you are on Windows you will need a Bash shell installed for the following commands. 

--

## Who are you?

Show the name of the currently logged in user:

```shell
whoami
```

Show the name of the machine is given on the network: 

```shell
hostname
```


--
## Folder Hierarchies

Much of working on the command line involves navigating around your system without the use of a Graphical User Interface (GUI). 

However, we will essentially be navigating around folder hierarchies like below:

<pre style="background-color:black; padding:20px">
root/
├─ directoryname/
       ├─ anotherdirectory/
                  ├─ aFileName.txt
</pre>


--

- __directory__ : also known as a folder
- __root directory__ :  "highest" directory in the hierarchy. You can also think of it as the start of a particular folder structure,  denoted by a tilde `~` on the command line.
- __directory path__ :  in the example above, there would be a directory called "directoryname", which has a folder called "anotherdirectory" inside of it, the path would be: `/directoryname/anotherdirectory`
- __file__ : a file, e.g. file.txt or index.html 

--



Print working directory that you are currently in:

```shell
pwd
```


List current directory contents:

``` shell
ls
```

--

Change directory to a directory using the "path" to that folder/directory, if the path starts with `/`  it will start at the root directory:

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

---

# Server Software 


We will be using [NginX](https://www.nginx.com/)  which is a web serving software. You can use Nginx to create various types of servers. We have created a simple static server using Nginx on our both pis. 

- We have installed Nginx
- And configured a simple static server
- It is serving up html


--

We create Nginx configuration files for each server.

All Nginx configuration files should be created here:

```shell
cd /etc/nginx/sites-enabled
```

Do `ls` to list all files. We can edit our config using nano:

```shell
nano wiki2print.conf
```

--

The server block is where we define our server set up:

```nginx

server {
		# 80 the default port for listening for http traffic
        listen 80 default_server;
        listen [::]:80 default_server;

        # the root folder that your website will be served out of
        root /var/www/wiki2print;

		# Allowed file formats for the index / home page
        index index.html index.htm;

		# The server name that you would set
        server_name wiki2print.local;

		# A location directive for any incoming URI requests
        location / {

                # First attempt to serve request as file, then as directory,
                # then fall back to displaying a 404 not found error
                try_files $uri $uri/ =404;
        }
        
 }
```

Take note of the root folder path, that is where our website is being served out of. 

--

#### Using Systemctl for Nginx

We will be using [systemd](https://en.wikipedia.org/wiki/Systemd) the Linux system and service manager. We can use systemctl to manage systemd services. 

Check status of Nginx:

```shell
sudo systemctl status nginx
```

Reload any Nginx configuration files that have changed (in sites-enabled). This is a safer process as it does not shut down the Nginx process entirely, if you have any errors in your configuration file it will keep the service running on the old configuration file:

```shell
sudo systemctl reload nginx
```


--

Inside of www we will see our wiki2print folder which is serving up our html:

<pre style="background-color:black; padding:20px">
var/
├─ www/
       ├─ wiki2print/
                  ├─ index.html
</pre>

Change directory to the root:

```shell
cd /var/www/wiki2print
```


--

Print out the contents of the index.html to terminal:

```shell
cat index.html
```

Let's open it up in Nano text editor to make some changes:

```shell
nano index.html
```

--

Nano quick start for editing a text file:

- You will see the nano editor open. At the bottom of the window, you will find some shortcuts to use with the Nano editor. The `^` (caret) means that you must press **CTRL** (Windows) or **control** (macOS) to use the chosen command.
- You should be able to edit and enter text in your file.
- Press **CTRL + O** to save the changes made in the file and continue editing.
- To exit from the editor, press **CTRL + X**. If there are changes, it will ask you whether to save them or not. Input **Y** for **Yes**, or **N** for **No**, then press **Enter**. If there are no changes, you will exit the editor right away.


Every time we save we should be able to refresh our browsers to see the change!

--

## More Terminal Commands

Make new directory:

 ```shell
mkdir directoryname
```


Make new file:

```shell
touch example.txt
```

--

Copy a text file called `example.txt` which is in a sub directory "exampleFolder" to a new location  `/path/to/folder`:

```shell
cp exampleFolder/example.txt /path/to/folder
```

--

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



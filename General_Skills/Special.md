## Description

>Don't power users get tired of making spelling mistakes in the shell? Not anymore! Enter Special, the Spell Checked Interface for Affecting Linux. Now, every word is properly spelled and capitalized... automatically and behind-the-scenes! Be the first to test Special in beta, and feel free to tell us all about how Special streamlines every development process that you face. When your co-workers see your amazing shell interface, just tell them: That's Special (TM) Start your instance to see connection details.
>
>Additional details will be available after launching your challenge instance.

----
When we first enter, we might be inclined to check what is in our home:
```
Special$ ls  
Is    
sh: 1: Is: not found
```

Our ls command is converted to Is, and then fails to execute. Checking a few other commands leads all syntax being altered to similar legitimate words, and all of them starting with capitilization. With our syntax ruined, we cannot execute commands properly! If we attempt to enter a different shell terminal, we receive an interesting rejection:
```
Special$ bash  
Why go back to an inferior shell?
```
My first thought is if my input is being altered after input but before bash processing, perhaps I can use characters designated to escaping metacharacter interpretation. In bash, the backslash `\` character is used to escape regular interpretations, and generally treats the character as raw text.
```
Special$ \ls  
Als    
sh: 1: Als: not found
```
Unfortunately, trying to escape the l in `ls`, which was previously converted to I, produces something completely different. Trying again with other commands, I manage to execute `whoami`. Perhaps I am on to something?
```
Special$ \whoami  
\whoami    
ctf-player
```

As it stands currently, we are escaping only 1 character, but the others are still treated as regular components of a string. What if we escape every character?
```
Special$ \e\c\h\o test  
\e\c\h\o test    
test
```
We can successfully perform `echo`! However, for some strange reason `ls` is not working:
```
Special$ \l\s  
Plus    
sh: 1: Plus: not found
```
Perhaps the problem is caused by ls being only 2 characters? Regardless, we have other ways to look around:
```
Special$ \d\i\r  
\d\i\r    
blargh  
  
Special$ \c\a\t \b\l\a\r\g\h  
\c\a\t \b\l\a\r\g\h    
cat: blargh: Is a directory

Special$ \d\i\r \b\l\a\r\g\h  
\d\i\r \b\l\a\r\g\h    
flag.txt  

Special$ \c\a\t \b\l\a\r\g\h\/\f\l\a\g\.\t\x\t  
\c\a\t \b\l\a\r\g\h\/\f\l\a\g\.\t\x\t    
picoCTF{5p311ch3ck_15_7h3_w0r57_6a2763f6}
```
While this approached works, it is rather tedious and ugly. We can escape this special shell by invoking bash:
```
Special$ \b\a\s\h  
\b\a\s\h    
ctf-player@challenge:~$ whoami  
ctf-player
```
Now we are free!

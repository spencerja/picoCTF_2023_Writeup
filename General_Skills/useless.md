## Description

>There's an interesting script in the user's home directory
>
>Additional details will be available after launching your challenge instance.

-----
For this challenge we are given a simple script, and told to explore the script to obtain the flag.

Firstly, we are required to connect to the challenge machine via ssh:

```
$ ssh picoplayer@saturn.picoctf.net -p PORT
```

The port number is customized to your own launched instances.

we see the script in our home directory, with executable premissions:
```
picoplayer@challenge:~$ ls -al  
total 16  
drwxr-xr-x 1 picoplayer picoplayer   20 Mar 17 18:38 .  
drwxr-xr-x 1 root       root         24 Mar 16 02:30 ..  
-rw-r--r-- 1 picoplayer picoplayer  220 Feb 25  2020 .bash_logout  
-rw-r--r-- 1 picoplayer picoplayer 3771 Feb 25  2020 .bashrc  
drwx------ 2 picoplayer picoplayer   34 Mar 17 18:38 .cache  
-rw-r--r-- 1 picoplayer picoplayer  807 Feb 25  2020 .profile  
-rwxr-xr-x 1 root       root        517 Mar 16 01:30 useless
```

First, we check the kind of file we are working with:
```
picoplayer@challenge:~$ file useless    
useless: Bourne-Again shell script, ASCII text executable
```

This is a bash script, written in plaintext. We will be able to easily view the contents with a simple `cat` command:
```
picoplayer@challenge:~$ cat useless    
#!/bin/bash  
# Basic mathematical operations via command-line arguments  
  
if [ $# != 3 ]  
then  
 echo "Read the code first"  
else  
       if [[ "$1" == "add" ]]  
       then    
         sum=$(( $2 + $3 ))  
         echo "The Sum is: $sum"     
  
       elif [[ "$1" == "sub" ]]  
       then    
         sub=$(( $2 - $3 ))  
         echo "The Substract is: $sub"    
  
       elif [[ "$1" == "div" ]]  
       then    
         div=$(( $2 / $3 ))  
         echo "The quotient is: $div"    
  
       elif [[ "$1" == "mul" ]]  
       then  
         mul=$(( $2 * $3 ))  
         echo "The product is: $mul"    
  
       else  
         echo "Read the manual"  
           
       fi  
fi
```

It appears the script is performing basic math, based on whatever operation you tell it to perform. For example, a division problem

```
picoplayer@challenge:~$ ./useless div 10 2  
The quotient is: 5
```

If we don't supply 3 arguments, we are told to read the code. If we don't supply a valid math operation, we are told to read the manual. There is not much to go on with this program here. We can see if a manual exists for this by using `man`:
```
man useless    
  
useless  
    useless, -- This is a simple calculator script  
  
SYNOPSIS  
    useless, [add sub mul div] number1 number2  
  
DESCRIPTION  
    Use the useless, macro to make simple calulations like addition,subtraction, multi-  
    plication and division.  
  
Examples  
    ./useless add 1 2  
      This will add 1 and 2 and return 3  
  
    ./useless mul 2 3  
      This will return 6 as a product of 2 and 3  
  
    ./useless div 6 3  
      This will return 2 as a quotient of 6 and 3  
  
    ./useless sub 6 5  
      This will return 1 as a remainder of substraction of 5 from 6  
  
Authors  
    This script was designed and developed by Cylab Africa  
  
    picoCTF{us3l3ss_ch4ll3ng3_3xpl0it3d_5562}
```

Turns out a man page exists, and our flag was placed here as well. Great..

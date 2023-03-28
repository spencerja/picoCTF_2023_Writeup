## Description

>Reception of Special has been cool to say the least. That's why we made an exclusive version of Special, called Secure Comprehensive Interface for Affecting Linux Empirically Rad, or just 'Specialer'. With Specialer, we really tried to remove the distractions from using a shell. Yes, we took out spell checker because of everybody's complaining. But we think you will be excited about our new, reduced feature set for keeping you focused on what needs it the most. Please start an instance to test your very own copy of Specialer.
>
>Additional details will be available after launching your challenge instance.

------
Upon entering, we might want to look around. However, we instantly come up with a problem:
```
Specialer$ ls  
-bash: ls: command not found
```
From the looks of it, ls is not accessible in our PATH. Specifying the full path does not help, and we soon find that we may not have access to any binaries:
```
Specialer$ /usr/bin/ls  
-bash: /usr/bin/ls: No such file or directory  
Specialer$ locate ls  
-bash: locate: command not found
```

Using tab auto-complete to navigate, we can see we are in a very restricted environment, and the only binary available is bash:
```
Specialer$ cd /  
bin/   home/  lib/   lib64/
Specialer$ cd /bin/bash
```
Note: Tabbing at / suggests bin, home, lib and lib64. Tabbing at /bin/ auto suggessts bash. This must be the only file in the directory.

At least with bash, we still have access to built-ins. Since our terminal commands are so limited, we can actually list them all by pressing tab twice on an empty line:
```
Specialer$    
!          builtin    dirs       exit       history    pushd      suspend    unalias  
./         caller     disown     export     if         pwd        test       unset  
:          case       do         false      in         read       then       until  
[          cd         done       fc         jobs       readarray  time       wait  
[[         command    echo       fg         kill       readonly   times      while  
]]         compgen    elif       fi         let        return     trap       {  
alias      complete   else       for        local      select     true       }  
bash       compopt    enable     function   logout     set        type          
bg         continue   esac       getopts    mapfile    shift      typeset       
bind       coproc     eval       hash       popd       shopt      ulimit        
break      declare    exec       help       printf     source     umask
```

We can utilize `echo` to see one directory layer at a time:
```
Specialer$ echo *  
abra ala sim

Specialer$ echo */*  
abra/cadabra.txt abra/cadaniel.txt ala/kazam.txt ala/mode.txt sim/city.txt sim/salabim.txt
```
We have several txt files here to view, but unfortunately no common binary to read them with. Bash also comes with a read built-in, combined with echo, we can output contents within files

```
Specialer$ read IFR < abra/cadabra.txt; echo $IFR  
Nothing up my sleeve!
```
Now that the contents are displayed, perhaps one option can give us a flag?
```
Specialer$ read IFR < ala/kazam.txt; echo $IFR  
return 0 picoCTF{REDACTED}
```

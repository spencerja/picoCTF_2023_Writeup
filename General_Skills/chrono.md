## Description

>How to automate tasks to run at intervals on linux servers?
>Additional details will be available after launching your challenge instance

-----
For this challenge, we are given a hint toward how linux servers might automate tasks.

Firstly, we are required to connect to the challenge machine via ssh:

```
$ ssh picoplayer@saturn.picoctf.net -p PORT
```

The port number is customized to your own launched instances.

Linux systems are able to automate tasks on regular intervals through the usage of `cron`. More info can be found on the [Linux manual page](https://man7.org/linux/man-pages/man8/cron.8.html). The customizable settings for cron jobs can be found in a file called `crontab`, which is always located in the /etc/ directory. We can check for a crontab file using `ls`:

```sh
picoplayer@challenge:~$ ls /etc/crontab  
/etc/crontab
```

Now that we have confirmed it is there, we can output the contents in plaintext using `cat`:

```
picoplayer@challenge:~$ cat /etc/crontab    
# picoCTF{REDACTED}
```

## Description

>Can you make sense of this file? Download the file [here](https://artifacts.picoctf.net/c/475/enc_flag).

------
Here we are tasked with making sense of a file. Following the link prompts us to download a file called `enc_flag`. Enc is almost certainly shorthand for encoded, meaning our flag will probably need to go through some decoding processes.

```
$ cat enc_flag
VmpGU1EyRXlUWGxTYmxKVVYwZFNWbGxyV21GV1JteDBUbFpPYWxKdFVsaFpWVlUxWVZaS1ZWWnVh  
RmRXZWtab1dWWmtSMk5yTlZWWApiVVpUVm10d1VWZFdVa2RpYlZaWFZtNVdVZ3BpU0VKeldWUkNk  
MlZXVlhoWGJYQk9VbFJXU0ZkcVRuTldaM0JZVWpGS2VWWkdaSGRXCk1sWnpWV3hhVm1KRk5XOVVW  
VkpEVGxaYVdFMVhSbFZrTTBKVVZXcE9VazFXV2toT1dHUllDbUY2UWpSWk1GWlhWa2RHZEdWRlZs  
aGkKYlRrelZERldUMkpzUWxWTlJYTkxDZz09Cg==
```

Dumping the contents in plaintext, we can see that it is clearly not human-readable. A string such as this, ending with =, is very likely encoded with base64. We can use an online decoding tool such as CyberChef to get through this quickly, or we can use the linux command `base64` to do this as well:

```
$ cat enc_flag | base64 -d  
VjFSQ2EyTXlSblJUV0dSVllrWmFWRmx0TlZOalJtUlhZVVU1YVZKVVZuaFdWekZoWVZkR2NrNVVX  
bUZTVmtwUVdWUkdibVZXVm5WUgpiSEJzWVRCd2VWVXhXbXBOUlRWSFdqTnNWZ3BYUjFKeVZGZHdW  
MlZzVWxaVmJFNW9UVVJDTlZaWE1XRlVkM0JUVWpOUk1WWkhOWGRYCmF6QjRZMFZXVkdGdGVFVlhi  
bTkzVDFWT2JsQlVNRXNLCg==
```
From our cat output we use | to redirect straight to another command, `base64`. Setting -d means the data will be base64 decoded. Despite this, the data is still unreadable. And we still see it ending in =? It appears the flag has been through multiple layers of encoding. We can decode for multiple rounds through the same method as before:

```
$ cat enc_flag | base64 -d | base64 -d | base64 -d | base64 -d | base64 -d | base64 -d  
picoCTF{REDACTED}
```

Finally after 6 rounds of decoding, the flag appears in plaintext. 
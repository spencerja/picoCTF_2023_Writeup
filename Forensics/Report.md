# hideme
## Description

>Every file gets a flag. The SOC analyst saw one image been sent back and forth between two people. They decided to investigate and found out that there was more than what meets the eye [here](https://artifacts.picoctf.net/c/493/flag.png).

-------------------

Downloading the file shows flag.png. Exiftool tells me there's more to this image than first shown:
```
exiftool flag.png
ExifTool Version Number         : 12.57
File Name                       : flag.png
Directory                       : .
File Size                       : 43 kB
File Modification Date/Time     : 2023:03:14 17:08:24-04:00
File Access Date/Time           : 2023:03:14 17:08:50-04:00
File Inode Change Date/Time     : 2023:03:14 17:08:30-04:00
File Permissions                : -rw-r--r--
File Type                       : PNG
File Type Extension             : png
MIME Type                       : image/png
Image Width                     : 512
Image Height                    : 504
Bit Depth                       : 8
Color Type                      : RGB with Alpha
Compression                     : Deflate/Inflate
Filter                          : Adaptive
Interlace                       : Noninterlaced
Warning                         : [minor] Trailer data after PNG IEND chunk
Image Size                      : 512x504
Megapixels                      : 0.258
```

The warning tells us there is extra data after the image information is done, the PNG IEND chunk. 
We can verify this ourselves with xxd:
```
xxd flag.png
<...snip...>
00009b20: 0448 2004 02ff 0fe1 93e1 23be c327 2900  .H .......#..').
00009b30: 0000 0049 454e 44ae 4260 8250 4b03 040a  ...IEND.B`.PK...
00009b40: 0000 0000 001c 136c 5600 0000 0000 0000  .......lV.......
00009b50: 0000 0000 0007 001c 0073 6563 7265 742f  .........secret/
00009b60: 5554 0900 03f7 370d 64f7 370d 6475 780b  UT....7.d.7.dux.
00009b70: 0001 0400 0000 0004 0000 0000 504b 0304  ............PK..
00009b80: 1400 0000 0800 1c13 6c56 656c 5c74 810b  ........lVel\t..
00009b90: 0000 1b0c 0000 0f00 1c00 7365 6372 6574  ..........secret
<...snip...>
```

Using binwalk, we can see the segmented data:
```
$ binwalk ./flag.png        

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PNG image, 512 x 504, 8-bit/color RGBA, non-interlaced
41            0x29            Zlib compressed data, compressed
39739         0x9B3B          Zip archive data, at least v1.0 to extract, name: secret/
39804         0x9B7C          Zip archive data, at least v2.0 to extract, compressed size: 2945, uncompressed size: 3099, name: secret/flag.png
42984         0xA7E8          End of Zip archive, footer length: 22
```

This can also be separated with binwalk:
```
$ binwalk -D=".*" ./flag.png 
```

The output file is a folder titled `_flag.png.extracted`

```
$ ls -al
total 108
drwxr-xr-x 3 kali kali  4096 Mar 14 17:37 .
drwxr-xr-x 3 kali kali  4096 Mar 14 17:28 ..
-rw-r--r-- 1 kali kali 43006 Mar 14 17:28 0
-rw-r--r-- 1 kali kali     0 Mar 14 17:28 29
-rw-r--r-- 1 kali kali 42965 Mar 14 17:28 29-0
-rw-r--r-- 1 kali kali  3267 Mar 14 17:28 9B3B
-rw-r--r-- 1 kali kali    22 Mar 14 17:28 A7E8
```

After the bulk of data, 9B3B, it appears to be compressed information:
```
$ file 9B3B           
9B3B: Zip archive data, at least v1.0 to extract, compression method=store
```

Rename it as zip file and unzip:
```
$ mv 9B3B 9B3B.zip && unzip 9B3B.zip
```

Resulting output is folder secret with yet another flag.png:
```
$ file flag.png 
flag.png: PNG image data, 600 x 50, 16-bit grayscale, non-interlaced
```

Opening this image shows our flag.
![image](https://github.com/spencerja/picoCTF_2023_Writeup/blob/main/Forensics/Images/Pasted%20image%2020230314175651.png)

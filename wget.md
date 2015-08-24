# Wget

The following example downloads a single file from internet and stores in the current directory.

```
$ wget http://www.openss7.org/repos/tarballs/strx25-
0.9.2.1.tar.bz2
```

While downloading it will show a progress bar with the following information:
* %age of download completion (for e.g. 31% as shown below)
* Total amount of bytes downloaded so far (for e.g. 1,213,592
bytes as shown below)
* Current download speed (for e.g. 68.2K/s as shown below)
* Remaining time to download (for e.g. eta 34 seconds as shown below)

Download in progress:
```
$ wget http://www.openss7.org/repos/tarballs/strx25-
0.9.2.1.tar.bz2
Saving to: `strx25-0.9.2.1.tar.bz2.1'
31% [=================> 1,213,592   68.2K/s  eta 34s
```
Download completed:
```
$ wget http://www.openss7.org/repos/tarballs/strx25-
0.9.2.1.tar.bz2
Saving to: `strx25-0.9.2.1.tar.bz2'
100%[======================>] 3,852,374   76.8K/s   in 55s
2009-09-25 11:15:30 (68.7 KB/s) - `strx25-0.9.2.1.tar.bz2'
saved [3852374/3852374]
```

Download and Store With a Different File name Using wget -O
By default wget will pick the filename from the last word after last forward slash, which may not be appropriate always.
```
$ wget -O taglist.zip
http://www.vim.org/scripts/download_script.php?src_id=7701
```

Continue the Incomplete Download Using wget -c
Restart a download which got stopped in the middle using wget -c option as shown below.

```
$ wget -c http://www.openss7.org/repos/tarballs/strx25-
0.9.2.1.tar.bz2
```

This is very helpful when you have initiated a very big file download which got interrupted in the middle. Instead of starting the whole download again, you can start the download from where it got interrupted using option -c

Note: If a download is stopped in middle, when you restart the download again without the option -c, wget will append .1 to the filename automatically as a file with the previous name already exist. If a file with .1 already exist, it will download the file with .2 at the end.
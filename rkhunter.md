# RKhunter

rkhunter "debugging" howto
1. Don't be afraid of the RKhunter warnings in the terminal.
2. Using RKhunter is always a work in progress.
3. To install RKhunter:

`sudo apt-get install rkhunter`

1. Before running RKhunter you will need to fill the file properties database by running the following command: rkhunter --propupd Do no forget to set rkhunter in sysconfig to run the --propupd every time new software is installed or else you will get "false positives" after every software and system update.

`sudo rkhunter --propupd`



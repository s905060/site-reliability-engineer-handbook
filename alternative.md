# Alternative 

Issue the following commands :-
Code:
```
alternatives --install /etc/alternatives/editor myeditor /usr/bin/kwrite 90
alternatives --install /etc/alternatives/editor myeditor /usr/bin/gedit 90
alternatives --install /etc/alternatives/editor myeditor /usr/bin/emacs 90
```

The first command installs a link editor under /etc/alternatives directory, links it to a generic name of myeditor, which in turn is linked to the kwrite application with a priority of 90. The next two commands do the same thing for gedit and emacs.

Now, Issue the following command

```
alternatives --config myeditor
```

Your output will be as follows :-

There are 3 programs which provide 'myeditor'.
```
  Selection    Command
-----------------------------------------------
*+ 1           /usr/bin/kwrite
     2           /usr/bin/gedit
     3           /usr/bin/emacs
```
Enter to keep the current selection[+], or type selection number:

The option with the + sign is the default application. As you see, the above command also expects you to specify another choice if you wish to. Now, we have successfully created our alternative and have associated it with the above 3 applications.

Now, double click on the Home icon on your desktop. I am assuming that you are currently working on KDE. Right click on any text file and then select Open With -> Other. In the window that is displayed, type /etc/alternatives/myeditor in the Open With Text Box and then tick the checkbox at the bottom that says Remember Application Association for this type of file and then click on the OK button. That's it. As you see in the above output, kwrite is the default editor. That is, whenever you double click on a text file, it will be opened in kwrite. Run the previous command again

Code:
```
alternatives --config myeditor
```

You will get the following output. Just type 2 as the selection number and press enter :-

Code:
There are 3 programs which provide 'myeditor'.
```
  Selection    Command
-----------------------------------------------
*+   1           /usr/bin/kwrite
     2           /usr/bin/gedit
     3           /usr/bin/emacs
```
Enter to keep the current selection[+], or type selection number: 2
Now, if you double click on any text file, it will open in gedit. Likewise, you can specify emacs as your default text editor
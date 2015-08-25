# Raw_input

raw_input() function reads a line from input (i.e. the user) and returns a string by stripping a trailing newline. The syntax is:
```
mydata = raw_input('Prompt :')
print (mydata)
```
In the above example, a string called mydata stores users data. Please note that if you want to compare mydata, then convert mydata to a numeric variable using int().

Examples

In this example, read the user name using raw_input() and display back on the screen using print():

```
#!/usr/bin/python
name=raw_input('Enter your name : ')
print ("Hi %s, Let us be friends!" % name);
```

Sample outputs:
````
Enter your name : nixCraft
Hi nixCraft, Let us be friends!
```

In this following example, a string called choice converted to a numeric variable:
```
#!/usr/bin/python
# Version 1
## Show menu ##
print (30 * '-')
print ("   M A I N - M E N U")
print (30 * '-')
print ("1. Backup")
print ("2. User management")
print ("3. Reboot the server")
print (30 * '-')

## Get input ###
choice = raw_input('Enter your choice [1-3] : ')
 
### Convert string to int type ##
choice = int(choice)
 
### Take action as per selected menu-option ###
if choice == 1:
        print ("Starting backup...")
elif choice == 2:
        print ("Starting user management...")
elif choice == 3:
        print ("Rebooting the server...")
else:    ## default ##
        print ("Invalid number. Try again...")
```

OR

``` 
#!/usr/bin/python
# Version 2
print (30 * '-')
print ("   M A I N - M E N U")
print (30 * '-')
print ("1. Backup")
print ("2. User management")
print ("3. Reboot the server")
print (30 * '-')
 
###########################
## Robust error handling ##
## only accept int       ##
###########################
## Wait for valid input in while...not ###
is_valid=0
 
while not is_valid :
        try :
                choice = int ( raw_input('Enter your choice [1-3] : ') )
                is_valid = 1 ## set it to 1 to validate input and to terminate the while..not loop
        except ValueError, e :
                print ("'%s' is not a valid integer." % e.args[0].split(": ")[1])
 
### Take action as per selected menu-option ###
if choice == 1:
        print ("Starting backup...")
elif choice == 2:
        print ("Starting user management...")
elif choice == 3:
        print ("Rebooting the server...")
else:
        print ("Invalid number. Try again...")
```

Sample outputs (note down invalid inputs are detected on fly):

```
------------------------------
   M A I N - M E N U
------------------------------
1. Backup
2. User management
3. Reboot the server
------------------------------
Enter your choice [1-3] : x
''x'' is not a valid integer.
Enter your choice [1-3] :
'''' is not a valid integer.
Enter your choice [1-3] : 1
Starting backup...
```
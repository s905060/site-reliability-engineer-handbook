# Salt use notes

##Real-time management
###cmd.run way

Excuting an order

`sudo Salt '*' cmd . run 'uptime'`

###System Module

System self-built module reference salt documentation http://docs.saltstack.com/en/latest/ref/modules/all/index.html

such as viewing minion of disk usage, use disk module usage function

`sudo Salt '*' Disk . usage`

Use sys.doc module queries related to the use of salt module. sys.doc equivalent system of man, can query the salt module online doc

`sudo Salt '*'   sys . doc Disk`

###Custom Module

Custom modules directory / SRV / Salt / _modules / , custom module paths generally /srv/salt/_modules/custom.py . Examples:

```$ Cat / SRV / Salt / _modules / custom . PY
 def Test (): 
  return  'i am Test'
```

Manual synchronization module to the minion

`sudo Salt '*' saltutil . sync_modules`

Execution module

`sudo Salt '*' custom . Test`

###Targetting
Matching simple way: Shell Style & Perl regular match

|mode |meaning
|--   |--|
|*	  |Match all minion
|web1	|Match web1
|web *	|Match the beginning of the web's minion
|web?	|Match 4 character ID web beginning of minion
|web [1-5]	|Match web1 to web5
|web [1,3]	|Match web1 and web3
|web1- (prod or devel)	|Match web1-prod and web1-devel
```

A list of matching

`sudo Salt - L 'web1, web2' Test . ping`

Grains Match

```
sudo Salt - G 'virtual: physical'   Test . ping   # Match all physical machine 

sudo Salt - G 'virtual: PHY *'   Test . ping   #grains match mode can also be used Shell Style & Perl regular mode
```

Pillar Match

`sudo Salt - I 'master: ipv6: False' Test . ping`

SLS file matching

```
'Virtual: physical' : 
    - match : Grain
```

###States

minion states for implementing state management, the official reference documentation http://docs.saltstack.com/ref/states/all/index.html
States define the path / src / Salt file_roots (in / etc / salt / master of variable definition), states use YAML file format definition 
states the file suffix is sls (Salt State), sls document preparation needs attention: When you want to reserve a space, otherwise it will cause a parse error

Manual mode execution state to modify the admin account bashrc Case Prepare /src/salt/bashrc.sls , which reads as follows

```
/ Home / admin /. bashrc : 
  File . managed : 
      - Source : Salt : // Files / bashrc 
      - user : admin
       -  group : admin
       - mode :  644
```

Ready for distribution bash file, Salt: // Files / bashrc correspondence / srv / salt / files / bashrc
make bash.sls take effect

`sudo Salt '*' State . sls 'bashrc'`

Highstate way. In fact, as the state is using top.sls entrance file /src/salt/top.sls following documents, top.sls reference bashrc.sls

```
Base : 
    '*' : 
        - bashrc
```

Manually perform highstate into force

`sudo Salt '*' State . highstate`

Let minion performed automatically using a schedule highstate

Defined /srv/pillar/top.sls
```
 Base : 
    '*' : 
        - Schedule
```

Defined /srv/pillar/schedule.sls (30 minutes)

```
Schedule : 
    highstate : 
        function : State . highstate
            minutes :  30
```

###Pillar

Official documents http://docs.saltstack.com/topics/tutorials/pillar.html
pillar data definition path / SRV / pillar , entrance file: /srv/pillar/top.sls
View pillar information

`sudo Salt '*' pillar . Data`

Grains
Official documents http://docs.saltstack.com/topics/targeting/grains.html
View grains classification

`sudo Salt '*' Grains . LS`

See all the information grains

`sudo Salt '*' Grains . items`

View grains a message

`sudo Salt '*' Grains . Item osrelease`

Custom grains

grains custom directory / SRV / Salt / _grains / , custom path /srv/salt/_grains/grans_test.py , example:

```
def grans_test (): 
  Grains =  {} 
  Grains [ 'grans_test' ]  =  'this is a grans Test!' 
  return Grains
 if __name__ ==  '__main__' : 
  Print grans_test ()
```

Synchronization grains

sudo Salt '*' saltutil . sync_grains
Check machine grains information

sudo Salt '*' Grains . Item grans_test
Job management
Salt real-time management of tasks are performed as Job
View Job being executed

sudo Salt - Jobs run . Active
View Job list (including implementation over)

sudo Salt - Jobs run . list_jobs
Displays the Job Status

sudo Salt - Jobs run . lookup_jid 20140408112045976162
Minion state management
View minion of the state up or down

sudo Salt - run manage . status   # See all state 
sudo Salt - run manage . up         # Look up the 
sudo Salt - run manage . down   # just look down the
View minion version, the command will prompt what you need to upgrade the version minion

sudo Salt - run manage . versions

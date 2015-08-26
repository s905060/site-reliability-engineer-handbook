# Why is it better to use “#!/usr/bin/env NAME” instead of “#!/path/to/NAME” as my shebang?

It isn't necessarily better.

The advantage of #!/usr/bin/env python is that it will use whatever python executable appears first in the user's $PATH.

The disadvantage of #!/usr/bin/env python is that it will use whatever python executable appears first in the user's $PATH.

That means that the script could behave differently depending on who runs it. For one user, it might use the /usr/bin/python that was installed with the OS. For another, it might use an experimental /home/phred/bin/python that doesn't quite work correctly.

And if python is only installed in /usr/local/bin, a user who doesn't have /usr/local/bin in $PATH won't even be able to run the script. (That's probably not too likely on modern systems, but it could easily happen for a more obscure interpreter.)

By specifying #!/usr/bin/python you specify exactly which interpreter will be used to run the script on a particular system.

Another potential problem is that the #!/usr/bin/env trick doesn't let you pass arguments to the intrepreter (other than the name of the script, which is passed implicitly). This usually isn't an issue, but it can be. Many Perl scripts are written with #!/usr/bin/perl -w, but use warnings; is the recommended replacement these days. Csh scripts should use #!/bin/csh -f -- but csh scripts are not recommended in the first place. But there could be other examples.

I have a number of Perl scripts in a personal source control system that I install when I set up an account on a new system. I use an installer script that modifies the #! line of each script as it installs it in my $HOME/bin. (I haven't had to use anything other than #!/usr/bin/perl lately; it goes back to times when Perl often wasn't installed by default.)

A minor point: the #!/usr/bin/env trick is arguably an abuse of the env command, which was originally intended (as the name implies) to invoke a command with an altered environment. Furthermore, some older systems (including SunOS 4, if I recall correctly) didn't have the env command in /usr/bin. Neither of these is likely to be a significant concern.  env does work this way, a lot of scripts do use the #!/usr/bin/env trick, and OS providers aren't likely to do anything to break it. It might be an issue if you want your script to run on a really old system, but then you're likely to need to modify it anyway.
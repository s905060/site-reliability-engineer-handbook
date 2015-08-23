# Tempfiles


The use of tempfiles as communications drops between cooperating programs is the oldest IPC technique there is. Despite drawbacks, it’s still useful in shellscripts, and in one-off programs where a more elaborate and coordinated method of communication would be overkill.

The most obvious problem with using tempfiles as an IPC technique is that it tends to leave garbage lying around if processing is interrupted before the tempfile can be deleted. A less obvious risk is that of collisions between multiple instances of a program using the same name for a tempfile. This is why it is conventional for shellscripts that make tempfiles to include $$ in their names; this shell variable expands to the process-ID of the enclosing shell and effectively guarantees that the filename will be unique (the same trick is supported in Perl).

Finally, if an attacker knows the location to which a tempfile will be written, it can overwrite on that name and possibly either read the producer’s data or spoof the consumer process by inserting modified or spurious data into the file.74 This is a security risk. If the processes involved have root privileges, this is a very serious risk. It can be mitigated by setting the permissions on the tempfile directory carefully, but such arrangements are notoriously likely to spring leaks.

All these problems aside, tempfiles still have a niche because they’re easy to set up, they’re flexible, and they’re less vulnerable to deadlocks or race conditions than more elaborate methods. And sometimes, nothing else will do. The calling conventions of your child process may require that it be handed a file to operate on. Our first example of a shellout to an editor demonstrates this perfectly.
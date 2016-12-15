#What are the common run levels in linux

|Run Level |	Mode	| Action |
| -------- | ------ | ------ |
| 0	| Halt |	Shuts down system |
| 1	| Single-User Mode	| Does not configure network interfaces, start daemons, or allow non-root logins |
| 2	| Multi-User Mode |	Does not configure network interfaces or start daemons. |
| 3	| Multi-User Mode with Networking	| Starts the system normally. |
| 4	| Undefined	| Not used/User-definable |
| 5	| X11	| As runlevel 3 + display manager(X) |
| 6	| Reboot	| Reboots the system |

Ref: https://www.liquidweb.com/kb/linux-runlevels-explained/

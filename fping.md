# fping

fping is one my favorite network profiling / scripting tool. It uses the Internet Control Message Protocol (ICMP) echo request to determine if a target host is responding or not.

Unlike ping , fping is meant to be used in scripts, so its output is designed to be easy to parse.

You can easily write perl / shell script to check a list of hosts and send mail if any are unreachable.

fping command example

Just type the following command to see if we can reach to router:

`$ fping router`

Output:

`router is alive`

You can read list of targets (hosts / servers) from a file. The -f option can only be used by the root user. Regular users should pipe in the file via
I/O redirectors (stdin). For example read all host names from ~/.ping.conf file

`$ fping < ~/.ping.conf`
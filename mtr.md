### Check for packet loss in intermediate hops
- [diagnosing-network-issues-with-mtr](https://www.linode.com/docs/networking/diagnostics/diagnosing-network-issues-with-mtr/)
- [diagnosing-network-issues-with-mtr-2](http://www.weirdkiwi.com/2014/02/diagnosing-internet-issues-part-two/)

# Introduction to MTR:

MTR (or also known as My Traceroute) Is a very popular and effective tool that combines Traceroute and Ping into one tool, but also does a continuous Traceroute Ping, which will come up with more accurate results, and is a great tool for measuring the packet losss a customer has as it will average it out per each second.
For windows there is WinMTR. While it does carry the MTR name, it technically runs very differently and it also has less options. this is only worth using if the customer has no linux instances. Win MTR uses only ICMP, so this also makes it not as desireable as MTR.

## Requirements:
Here are the downloads for MTR:
- Windows: This is an .exe file that requires no installation.
```http
http://winmtr.net/
```
- Linux: If using Amazon Linux, using sudo yum install mtr should install MTR. Otherwise, the source is below.
```http 
http://www.bitwizard.nl/mtr/
```
## Process/Summary:
We will start with WinMTR. It is the easier of two to use, however, it also lacks most of the features that MTR itself uses.
WinMTR works entirely off of a graphical interface. It is pretty straight forward as you will see below. You will just entire the IP of the service you want to Ping and trace route, and click start.

The columns go as ordered. Hostname of the router it his hitting, the Number of hope, the Packet less, packets sent, packets received, the best ping, average, and worst, and then the last one. With Win MTR, our biggest concerns to look at are the Loss% as well as Hostname to identify where the packet loss is happening.

The benefit of WinMTR is how easy it is to export. You have the option to Export as a .txt file or a .html file. In terms of the actual options menu, the only options you can set are Interval, Ping Size, and Most hosts in LRU list. You will generally leave these to default for the best results.

Linux, MTR is much more feature rich and gives us a lot more flexibility.
Be careful when using mtr 0.75 in Linux has a bug that shows a 10% packet loss on the last hop when using the --report / -r flag (destination). https://bugs.launchpad.net/ubuntu/+source/mtr/+bug/966065
The bug is fixed in 0.88

With MTR, you have the option to export it, do a UDP traceroute as opposed to just ICMP, it also allows you to specify how many you want to do for your reports, and many other custom options.
IMPORTANT: All MTR commands need to be performed as sudo or as su. If you get the error mtr: unable to get raw sockets. then you are not running as sudo.
The standard MTR command is simple. Its just "sudo mtr hostname". If you wish to do a udp trace, do "sudo mtr -u hostname" However, this will generate a non stop MTR that will run until you end it.  You do have some additional options at this screen.


 Help   Display mode   Restart statistics   Order of fields   quit
Help is Self explaining. Display Mode will switch between standard MTR read outs, and showing the last 50 pings only,. Restart Statics just resets it back to 0. And Order of Fields lets you select

The best format to run for a customer output is to do a -report, or -r, which will generate an output that is easier for the customer to copy and paste to us. You will want to use -c to specify how many pings it does before it generates the report. Please note, this may take a little bit based on what you set the count to.


Example: 
```bash 
mtr -r -c 10 8.8.8.8
```
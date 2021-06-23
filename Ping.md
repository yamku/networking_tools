# Ping Syntax

## Notes

```bash
ping [-t] [-a] [-n count] [-l size] [-f] [-i TTL] [-v TOS] [-r count] [-s count] [-w timeout] [-R] [-S srcaddr] [-p] [-4] [-6] target [/?]
```
### Ping Switch
```bash
-t Using this option will ping the target until you force it to stop using Ctrl-C.

-a This ping command option will resolve, if possible, the hostname of an IP address target.

-n count This option sets the number of ICMP Echo Requests to send, from 1 to 4294967295. The ping command will send 4 by default if -n isn't used.

-l size Use this option to set the size, in bytes, of the echo request packet from 32 to 65,527. The ping command will send a 32-byte echo request if you don't use the -l option.

-f Use this ping command option to prevent ICMP Echo Requests from being fragmented by routers between you and the target. The -f option is most often used to troubleshoot Path Maximum Transmission Unit (PMTU) issues.

-i TTL This option sets the Time to Live (TTL) value, the maximum of which is 255.

-v TOS This option allows you to set a Type of Service (TOS) value. Beginning in Windows 7, this option no longer functions but still exists for compatibility reasons.

-r count Use this ping command option to specify the number of hops between your computer and the target computer or device that you'd like to be recorded and displayed. The maximum value for count is 9, so use the tracert command instead if you're interested in viewing all the hops between two devices.

-s count Use this option to report the time, in Internet Timestamp format, that each echo request is received and echo reply is sent. The maximum value for count is 4, meaning that only the first four hops can be time stamped.

-w timeout Specifying a timeout value when executing the ping command adjusts the amount of time, in milliseconds, that ping waits for each reply. If you don't use the -w option, the default timeout value of 4000 is used, which is 4 seconds.

-R This option tells the ping command to trace the round trip path.

-S srcaddr Use this option to specify the source address.

-p Use this switch to ping a Hyper-V Network Virtualization provider address.

-4 This forces the ping command to use IPv4 only but is only necessary if target is a hostname and not an IP address.

-6 This forces the ping command to use IPv6 only but as with the -4 option, is only necessary when pinging a hostname. target This is the destination you wish to ping, either an IP address or a hostname.

/? Use the help switch with the ping command to show detailed help about the command's several options.
```

### Note: The -f, -v, -r, -s, -j, and -k options work when pinging IPv4 addresses only. The -R and -S options only work with IPv6.

Other less commonly used switches for the ping command exist including ```[-j host-list], [-k host-list], and [-c compartment]```. Execute ping /? from the Command Prompt for more information on these two options.

Tip: Save all of the ping command output to a file using a redirection operator. See How to Redirect Command Output to a File for instructions or see our Command Prompt Tricks list for more tips.
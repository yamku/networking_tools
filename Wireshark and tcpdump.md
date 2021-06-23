# Wireshark
- Wireshark is the most commonly used network protocol analyzer tool. It is used to inspect hundreds of protocols in order to troubleshoot various network related issues.
  
[Download Link](http://www.wireshark.org/download.html)

---

# TCPDUMP

Another commonly used packet analyzer is ```tcpdump```, that runs under the command line and is similar to Wireshark, however, doesn’t have a Graphical User Interface (GUI) as Wireshark.

‘Tcpdump’ works on most Linux operating systems. Packet captures can be done using ```tcpdump```, it displays the contents of network packets which read from a network interface card. ```Tcpdump``` can write packets to standard output or a file.

- ```-n```: This options is used when DNS lookup is not needed. This option don’t convert host addresses to names.

- ```-i```: This options is used to select the network interface to listen on the machine.

- ```-v```: This option provides verbose output.

For more information about _tcpdump_ options, run the command: 
```sh 
man tcpdump
```
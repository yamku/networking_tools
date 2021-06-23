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

Also, tcpdump output can be saved in ‘*.pcap’ file and can be analyzed using Wireshark. To save tcpdump output to a file, use the following command:
```bash
sudo tcpdump –ni any –w /<directory>/<filename>.pcap
```

- Rotating capture window:
```bash
sudo tcpdump -i eth0 -w /var/tmp/rotate.pcap -W 3 -C 10 -s 150 &
```
---

# Some Important Terms:

From [Wireshark Wiki](http://wiki.wireshark.org/TCP_Analyze_Sequence_Numbers)

- ### TCP ZeroWindow:
  Occurs when a receiver advertises a receive window size of zero. This effectively tells the sender to stop sending because the receiver's buffer is full. Indicates a resource issue on the receiver, as the application is not retrieving data from the TCP buffer in a timely manner.

- ### TCP ZerowindowProbe:
  The sender is testing to see if the receiver's zero window condition still exists by sending the next byte of data to elicit an ACK from the receiver. If the window is still zero, the sender will double his persist timer before probing again.

- ### TCP ZeroWindowViolation:
  The sender has ignored the zero window condition of the receiver and sent additional bytes of data.

- ### TCP WindowUpdate:
  This indicates that the segment was a pure WindowUpdate segment. A WindowUpdate occurs when the application on the receiving side has consumed already received data from the RX buffer causing the TCP layer to send aWindowUpdate to the other side to indicate that there is now more space available in the buffer. Typically seen after a TCP ZeroWindowcondition has occurred. Once the application on the receiver retrieves data from the TCP buffer, thereby freeing up space, the receiver should notify the sender that the TCP ZeroWindow condition no longer exists by sending a TCP WindowUpdate that advertises the current window size.

- TCP WindowFull - This flag is set on segments where the payload data in the segment will completely fill the RX buffer on the host on the other side of the TCP session. The sender, knowing that it has sent enough data to fill the last known RX window size, must now stop sending until at least some of the data is acknowledged (or until the acknowledgement timer for the oldest unacknowledged packet expires). This causes delays in the flow of data between sender and receiver and lowers throughput. When this event occurs, aZeroWindow condition might occur on the other host and we might see TCP ZeroWindow segments coming back. Do note that this can occur even if no ZeroWindow condition is ever triggered. For example, if the TCP WindowSize is too small to accommodate a high end-to-end latency this will be indicated by TCP WindowFull and in that case there will not be any TCP ZeroWindow indications at all.

---
# MSS vs MTU
- The relationship between the value of the maximum IP datagram size and the maximum TCP segment size is obscure.  The problem is that both the IP header and the TCP header may vary in length.  The TCP Maximum Segment Size option (MSS) is defined to specify the maximum number of data octets in a TCP segment exclusive of TCP (or IP) header.
- To notify the data sender of the largest TCP segment it is possible to receive the calculation of the MSS value to send is:
```MSS = MTU - sizeof(TCPHDR) - sizeof(IPHDR)```
- On receipt of the MSS option the calculation of the size of segment that can be sent is:
```SndMaxSegSiz = MIN((MTU - sizeof(TCPHDR) - sizeof(IPHDR)), MSS)```
This begs the question, though.  What value should be used for then "sizeof(TCPHDR)" and for the "sizeof(IPHDR)"?
- There are three reasonable positions to take: the conservative, the moderate, and the liberal.
The conservative or pessimistic position assumes the worst -- that both the IP header and the TCP header are maximum size, that is, 60 octets each.
- ```MSS = MTU - 60 - 60 = MTU - 120```
If MTU is 576 then MSS = 456
The moderate position assumes the that the IP is maximum size (60 octets) and the TCP header is minimum size (20 octets), because there are no TCP header options currently defined that would normally be sent at the same time as data segments.
- ```MSS = MTU - 60 - 20 = MTU - 80```
If MTU is 576 then MSS = 496
The liberal or optimistic position assumes the best -- that both the IP header and the TCP header are minimum size, that is, 20 octets each.
- ```MSS = MTU - 20 - 20 = MTU - 40```
If MTU is 576 then MSS = 536
# TCP Slow Start
TCP employs four critical congestion control mechanisms in order to function efficiently under constantly changing network conditions such as those found on the global Internet. These mechanisms are defined in [RFC 5681](http://tools.ietf.org/html/rfc5681) (and previously in RFCs 2001 and 2581) as slow start, congestion avoidance, fast retransmit, and fast recovery. Today we'll look at how the slow start mechanism is used to increase the initial throughput rate of a TCP connection immediately upon establishment.

---
# Transmission Windows
Before digging into slow start, it is necessary to understand how TCP places limits, called windows, on the amount of data which can be in transit between two endpoints at a given time. Because of the reliable nature of TCP, a TCP sender can transmit only a limited amount of data before it must receive an acknowledgement from the receiver; this is to ensure that any lost segments can be retransmitted efficiently.

There are two variables which affect how much unacknowledged data a sender can send: the receiver window (RWND) advertised by the TCP peer and the sender's own congestion window (CWND).

The sender's congestion window, however, is known only to the sender and does not appear on the wire. The lower of the two window values becomes the maximum amount of unacknowledged data the sender can transmit.

So how is the CWND determined? 
- RFC 5681 mandates that the initial CWND value for a connection must be set relative to the sender's maximum segment size (SMSS) for the connection:
```
If SMSS > 2190 bytes:
IW = 2 * SMSS bytes and MUST NOT be more than 2 segments
If (SMSS > 1095 bytes) and (SMSS <= 2190 bytes):
IW = 3 * SMSS bytes and MUST NOT be more than 3 segments
If SMSS <= 1095 bytes:
IW = 4 * SMSS bytes and MUST NOT be more than 4 segments
```
- The MSS for either side of a TCP connection is advertised as a TCP option in the SYN packets, and both sides use the lower of the two advertised values. 
- An MSS of 1460 bytes is common on the Internet, being derived from a layer two MTU of 1500 bytes (1500 - 20 bytes for IP - 20 bytes for TCP = 1460). 
- According to RFC 5681, an SMSS of 1460 bytes would give us an initial CWND of 4380 bytes (3 * 1460 = 4380). 
- However, in practice the initial CWND size will vary among TCP/IP stack implementations.

Again, remember that the sender's effective transmission window is always the lower of CWND and RWND. As we'll see, the slow start (and later, congestion avoidance) mechanisms are used to dynamically increase (and lower) the sender's transmission window throughout the duration of a TCP connection.

---
# Slow Start
The slow start algorithm can simplified as this: for every acknowledgment received, increase the CWND by one MSS. For example, if our MSS is 1460 bytes and our initial CWND is twice that (2920 bytes), we can initially send up to two full segments immediately after the connection is established, but then we have to wait for our segments to be acknowledged by the recipient. For each of the two acknowledgments we then receive, we can increase our CWND by one MSS (1460 bytes). So, after we receive two acknowledgments back, our CWND becomes 5840 bytes (2930 + 1460 + 1460). Now we can send up to four full segments before we have to wait for another acknowledgment.

# Some Reasons for Latency/Slow Network
## Packet Loss: 
Packet loss cuts down the congestion in &frac12; of the previous size in TCP communication. Using the TCP Slow Start algorithm, the congestion window drops to 50% of its previous size and then goes through the congestion avoidance state, where the congestion window grows slowly. If there is again packet loss experienced, then the window again drops to 50% ** of its previous size. The congestion window is the number of outstanding bytes that is unacknowledged.

In UDP based communications, we are reliant on the application to recover. The application needs to be set to re-request something before it gives up.

  This will vary based on the congestion algorithm used.

## Client, Server and Wire Latency:
-  If time between the syn and the syn ack is high, then the round trip time is high and this is caused by wire latency.

- If the time between the syn ack and ack is slow then there must be an issue with the client.

- If the client takes a longer time in sending out the GET request, then the issue is with the client application. Probably processing time.

- Once the GET request reaches the server and then the ACK from the server takes time then the issue is with wire latency.

- If the ACK for the GET request was sent very fast, but there is delay in data coming from the server then the issue is with the server.

- Using wireshark, set the time as “Seconds since previous displayed packet”.

## Window Scaling Issues [RFC 1323](http://tools.ietf.org/html/rfc1323)
There is a receive window, which is the TCP receive buffer space, which is a value upto 65535 bytes. As we send request to server, server sends back data to the client. The data fills the bucket on the client side. The application needs to pick up this data from the receive window bucket before it fills up. If the application is slow in picking up this data, the receive window size reduces and the client may stop the communication notifying it has no receive buffer space available. The receive window size can be set in the Operating System.
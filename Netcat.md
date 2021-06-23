# Netcat or nc
---
- _Netcat_ or _nc_ is a networking utility for debugging and investigating the network.

This utility can be used for creating TCP/UDP connections and investigating them. The biggest use of this utility is in the scripts where we need to deal with TCP/UDP sockets.
In this article we will learn about the _netcat_ command by some practical examples.

---

* The _netcat_ utility can be run in the server mode on a specified port listening for incoming connections.
```bash nc -l 2389```

* Also, it can be used in client mode trying to connect on the port(2389) just opened
```bash nc localhost 2389```

Now, if we write some text at the client side, it reaches the server side. Here is the proof :
```bash nc localhost 2389 HI, server```

On the terminal where server is running :
```bash nc -l 2389 HI, server```

So we see that _netcat_ utility can be used in the client server socket communication.
---
# Use Netcat to transfer files

- The _netcat_ utility can also be used to transfer files.

At the client side, suppose we have a file named ‘testfile’ containing :
```bash cat testfile hello test```

and at the server side we have an empty file ‘test’

Now, we run the server as :
```bash nc -l 2389 > test```
and run the client as :
```bash cat testfile | nc localhost 2389```

Now, when we see the ‘test’ file at the server end, we see :
```bash cat test hello test```

So we see that the file data was transferred from client to server.

---
# Supports Timeouts

- There are cases when we do not want a connection to remain open forever. In that case, through ‘-w’ switch we can specify the timeout in a connection. So after the seconds specified along with -w flag, the connection between the client and server is terminated.

Server :
```bash nc -l 2389```
Client :
```bash nc -w 10 localhost 2389```
The connection above would be terminated after 10 seconds.

NOTE : Do not use the -w flag with -l flag at the server side as in that case -w flag causes no effect and hence the connection remains open forever.

---
# Netcat Supports IPv6

- The flag -4 or -6 specifies that netcat utility should use which type of addresses. -4 forces nc to use IPV4 address while -6 forces nc to use IPV6 address.

Server :
```bash nc -4 -l 2389```
Client :
```bash nc -4 localhost 2389```

Now, if we run the netstat command, we see :
```bash netstat | grep 2389 tcp 0 0 localhost:2389 localhost:50851 ESTABLISHED tcp 0 0 localhost:50851 localhost:2389 ESTABLISHED```

The first field in the above output would contain a postfix ‘6’ in case the IPV6 addresses are being used. Since in this case it is not, so a connection between server and client is established using IPV4 addresses.

Now, If we force nc to use IPV6 addresses

Server :
```bash nc -6 -l 2389```
Client :
```bash nc -6 localhost 2389```

Now, if we run the netstat command, we see :
```bash netstat | grep 2389 tcp6 0 0 localhost:2389 localhost:33234 ESTABLISHED tcp6 0 0 localhost:33234 localhost:2389 ESTABLISHED```

So now a postfix ‘6’ with ‘tcp’ shows that nc is now using IPV6 addresses.

---
# Disable Reading from STDIN in Netcat

- This functionality can be achieved by using the flag ```-d```. In the following example, we used this flag at the client side.

Server :
```bash nc -l 2389``` 

Client :
```bash nc -d localhost 2389 Hi```

The text ‘Hi’ will not be sent to the server end as using ``` -d``` option the read from stdin has been disabled.

---
# Force Netcat Server to Stay Up

- If the netcat client is connected to the server and then after sometime the client is disconnected then normally netcat server also terminates.

Server :
```bash nc -l 2389```

Client :
```bash nc localhost 2389 ^C```

Server :
```bash nc -l 2389 $```

So, in the above example we see that as soon as the client got disconnected the server was also terminated.

This behavior can be controlled by using the -k flag at the server side to force the server to stay up even after the client has disconnected.

Server :
```bash nc -k -l 2389```

Client :
```bash nc localhost 2389 ^C```

Server :
```bash nc -k -l 2389```

So we see that by using the -k option the server remains up even if the client got disconnected.

---
# Configure Netcat Client to Stay Up after EOF

- Netcat client can be configured to stay up after EOF is received. In a normal scenario, if the ```nc``` client receives an EOF character then it terminates immediately but this behavior can also be controlled if the ```-q``` flag is used. This flag expects a number which depicts number of seconds to wait before client terminates (after receiving EOF)

Client should be started like :
```bash nc -q 5 localhost 2389```
Now if the client ever receives an EOF then it will wait for 5 seconds before terminating.
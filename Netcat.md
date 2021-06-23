# Netcat or nc
---
_Netcat_ or _nc_ is a networking utility for debugging and investigating the network.

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

The _netcat_ utility can also be used to transfer files.

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
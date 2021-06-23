# Netcat or nc
---
_Netcat_ or _nc_ is a networking utility for debugging and investigating the network.

This utility can be used for creating TCP/UDP connections and investigating them. The biggest use of this utility is in the scripts where we need to deal with TCP/UDP sockets.
In this article we will learn about the _netcat_ command by some practical examples.

---

The _netcat_ utility can be run in the server mode on a specified port listening for incoming connections.
```bash nc -l 2389```

Also, it can be used in client mode trying to connect on the port(2389) just opened
```bash nc localhost 2389```

Now, if we write some text at the client side, it reaches the server side. Here is the proof :
```bash nc localhost 2389 HI, server```

On the terminal where server is running :
```bash nc -l 2389 HI, server```

So we see that _netcat_ utility can be used in the client server socket communication.
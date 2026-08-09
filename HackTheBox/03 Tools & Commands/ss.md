socket statistics
can be used to check which ports are listening locally on a given machine
-l: Display only listening sockets.
-t: Display TCP sockets.
-n: Do not try to resolve service names.

E.g.:
![[Pasted image 20260413163948.png]]
 first column indicates the state that the socket is in; since we specified the -l flag, we will only see sockets that are actively listening for a connection. Moving along horizontally, the Recv-Q column is not of much concern at this point, it simply displays the number of queued received packets for that given port; Send-Q does the same but for the amount of sent packets. The crucial column is the fourth, which displays the local address on which a service listens, as well as its port. 127.0.0.1 is synonymous with localhost , and essentially means that the specified port is only listening locally on the machine and cannot be accessed externally. This also explains why we did not discover such ports in our initial Nmap scan. On the other hand, the addresses 0.0.0.0 , * ,
and [::] indicate that a port is listening on all intefaces, meaning that it is accessible externally, as well as locally, which is why we were able to detect both the FTP service on port 21 , as well as the SSH service on port 22 . 
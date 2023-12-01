0x08. Networking basics #1

LocalHost
In computer networking, localhost is a hostname that refers to the current computer used to access it. The name localhost is reserved for loopback purposes.[1] It is used to access the network services that are running on the host via the loopback network interface. Using the loopback interface bypasses any local network interface hardware.

Loopback
The local loopback mechanism may be used to run a network service on a host without requiring a physical network interface, or without making the service accessible from the networks the computer may be connected to. For example, a locally installed website may be accessed from a Web browser by the URL http://localhost to display its home page.

IPv4 network standards reserve the entire address block 127.0.0.0/8 (more than 16 million addresses) for loopback purposes.[2] That means any packet sent to any of those addresses is looped back. The address 127.0.0.1 is the standard address for IPv4 loopback traffic; the rest are not supported by all operating systems. However, they can be used to set up multiple server applications on the host, all listening on the same port number. In the IPv6 addressing architecture[3] there is only a single address assigned for loopback: ::1. The standard precludes the assignment of that address to any physical interface, as well as its use as the source or destination address in any packet sent to remote hosts.

0.0.0.0

Official meaning and use
IANA, who allocate IP addresses globally, have allocated the single IP address 0.0.0.0[1] to RFC 1122 section 3.2.1.3. It is named as "This host on this network".

RFC 1122 refers to 0.0.0.0 using the notation {0,0}. It prohibits this as a destination address in IPv4 and only allows it as a source address under specific circumstances.

A host may use 0.0.0.0 as its own source address in IP when it has not yet been assigned an address, such as when sending the initial DHCPDISCOVER packet when using DHCP.

Operating system specific uses
Some operating systems have attributed special internal meanings to the address. These uses do not result in IPv4 packets containing 0.0.0.0 and so are not governed by RFC 1122.[2] These meanings may not be consistent between OS.

In both Windows and Linux, when selecting which of a host's IP address to use as a source IP, a program may specify INADDR_ANY (0.0.0.0).[3][4]

In Linux a program may specify 0.0.0.0 as the remote address to connect to the current host (AKA localhost).[citation needed]

Other non-standard uses
Besides the use by operating systems internally, other uses have been attributed to the address.[5][6]

A non-routable meta-address used to designate an invalid, unknown or non applicable target
The address a host assigns to itself when address request via DHCP has failed, provided the host's IP stack supports this. This usage has been replaced with the APIPA mechanism in modern operating systems.
A way to explicitly specify that the target is unavailable.[7]
A way to route a request to a nonexistent target instead of the original target. Often used for adblocking purposes. This can conflict with OS specific behaviour.[8]
Routing
In routing tables, 0.0.0.0 can also appear in the gateway column. This indicates that the gateway to reach the corresponding destination subnet is unspecified. This generally means that no intermediate routing hops are necessary because the system is directly connected to the destination.[9]

The CIDR notation 0.0.0.0/0 defines an IP block containing all possible IP addresses. It is commonly used in routing to depict the default route as a destination subnet. It matches all addresses in the IPv4 address space and is present on most hosts, directed towards a local router.

In IPv6
In IPv6, the all-zeros address is typically represented by :: (two colons), which is the short notation of 0000:0000:0000:0000:0000:0000:0000:0000.[10] The IPv6 variant serves the same purpose as its IPv4 counterpart.

8 Practical Linux Netcat NC Command Examples
by HIMANSHU ARORA on APRIL 23, 2012
Netcat or nc is a networking utility for debugging and investigating the network.

This utility can be used for creating TCP/UDP connections and investigating them. The biggest use of this utility is in the scripts where we need to deal with TCP/UDP sockets.

In this article we will learn about the netcat command by some practical examples.

1. Netcat in a Server-Client Architecture
The netcat utility can be run in the server mode on a specified port listening for incoming connections.

$ nc -l 2389
Also, it can be used in client mode trying to connect on the port(2389) just opened

$ nc localhost 2389
Now, if we write some text at the client side, it reaches the server side. Here is the proof :

$ nc localhost 2389
HI, server
On the terminal where server is running :


$ nc -l 2389
HI, server
So we see that netcat utility can be used in the client server socket communication.

2. Use Netcat to Transfer Files
The netcat utility can also be used to transfer files. At the client side, suppose we have a file named ‘testfile’ containing :

$ cat testfile
hello test
and at the server side we have an empty file ‘test’

Now, we run the server as :

$ nc -l 2389 > test
and run the client as :

cat testfile | nc localhost 2389
Now, when we see the ‘test’ file at the server end, we see :

$ cat test
hello test
So we see that the file data was transfered from client to server.

3. Netcat Supports Timeouts
There are cases when we do not want a connection to remain open forever. In that case, through ‘-w’ switch we can specify the timeout in a connection. So after the seconds specified along with -w flag, the connection between the client and server is terminated.

Server :

nc -l 2389
Client :

$ nc -w 10 localhost 2389
The connection above would be terminated after 10 seconds.

NOTE : Do not use the -w flag with -l flag at the server side as in that case -w flag causes no effect and hence the connection remains open forever.

4. Netcat Supports IPV6 Connectivity
The flag -4 or -6 specifies that netcat utility should use which type of addresses. -4 forces nc to use IPV4 address while -6 forces nc to use IPV6 address.

Server :

$ nc -4 -l 2389
Client :

$ nc -4 localhost 2389
Now, if we run the netstat command, we see :

$ netstat | grep 2389
tcp        0      0 localhost:2389          localhost:50851         ESTABLISHED
tcp        0      0 localhost:50851         localhost:2389          ESTABLISHED
The first field in the above output would contain a postfix ‘6’ in case the IPV6 addresses are being used. Since in this case it is not, so a connection between server and client is established using IPV4 addresses.

Now, If we force nc to use IPV6 addresses

Server :

$ nc -6 -l 2389
Client :

$ nc -6 localhost 2389
Now, if we run the netstat command, we see :

$ netstat | grep 2389
tcp6       0      0 localhost:2389          localhost:33234         ESTABLISHED
tcp6       0      0 localhost:33234         localhost:2389          ESTABLISHED
So now a postfix ‘6’ with ‘tcp’ shows that nc is now using IPV6 addresses.

5. Disable Reading from STDIN in Netcat
This functionality can be achieved by using the flag -d. In the following example, we used this flag at the client side.

Server :

$ nc -l 2389
Client :

$ nc -d localhost 2389
Hi
The text ‘Hi’ will not be sent to the server end as using -d option the read from stdin has been disabled.

6. Force Netcat Server to Stay Up
If the netcat client is connected to the server and then after sometime the client is disconnected then normally netcat server also terminates.

Server :

$ nc -l 2389
Client :

$ nc localhost 2389
^C
Server :

$ nc -l 2389
$
So, in the above example we see that as soon as the client got disconnected the server was also terminated.

This behavior can be controlled by using the -k flag at the server side to force the server to stay up even after the client has disconnected.

Server :

$ nc -k -l 2389
Client :

$ nc localhost 2389
^C
Server :

$ nc -k -l 2389
So we see that by using the -k option the server remains up even if the client got disconnected.

7. Configure Netcat Client to Stay Up after EOF
Netcat client can be configured to stay up after EOF is received. In a normal scenario, if the nc client receives an EOF character then it terminates immediately but this behavior can also be controlled if the -q flag is used. This flag expects a number which depicts number of seconds to wait before client terminates (after receiving EOF)

Client should be started like :

nc  -q 5  localhost 2389
Now if the client ever receives an EOF then it will wait for 5 seconds before terminating.

8. Use Netcat with UDP Protocol
By default all the sockets that nc utility creates are TCP protocols but this utility also works with UDP protocol. To enable UDP protocol the -u flag is used.

Server :

$ nc -4 -u -l 2389
Client :

$ nc -4 -u localhost 2389
Now, both the server and client are configured to use UDP protocol. This can be confirmed by the following netstat command. So we see that this connection is now using the UDP protocol.

$ netstat | grep 2389
udp        0      0 localhost:42634         localhost:2389          ESTABLISH

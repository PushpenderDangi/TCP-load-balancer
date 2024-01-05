

## Document on how to setup caddy and balance load



**Submitted By** :

**Pushpender**


 ## Table of Content 
 
[Task requirement](#task-requirement)

[Environment details](#environment-details)

[List of tools and technologies](#list-of-tools-and-technologies)

[For VM1 caddy installation](#vm1-caddy-installation)

[For VM2 caddy installation](#for-vm2-caddy-installation)

[Curl on virtual machines](#curl-on-virtual-machines)

[Curl on base machine](#curl-on-base-machine)

[Command to be run on VM1](#command-to-be-run-on-virtual-machine-1)

[Command to be run on Virtual Machine 2](#command-to-be-run-on-virtual-machine-2)

[Command to be run on Virtual Machine 1](#command-to-be-run-on-virtual-machine-1)

[Load testing on base machine](#load-testing-on-base-machine)




**Transform iptables into a TCP load balancer**

# Task requirement

To create two new Caddy or two Nginx server and after creating, it needs to forward the 50% of Load to another server.


#  Environment details

UbuntuNAME="Ubuntu"

VERSION="20.04.5

Codename: Focal Fosa   

# List of tools and technologies

- Caddy is most often used as an HTTPS server, but it is suitable for any long-running Go program. First and foremost, it is a platform to run Go applications.


- Caddy:- CADDY_VERSION=v2.7.5


- I have made two virtual machines so that they can work like a web server.





**TCP Load Balancing**

Load balancing ensures high system availability through the distribution of workload across multiple components. Using multiple components with load balancing, instead of a single component, may increase reliability through redundancy.

TCP load balancing component receives a connection request from a client application through a network socket. This component decides which node in the environment receives the request. A TCP load balancer is a type of load balancer that uses transmission control protocol (TCP), which operates at layer 4 — the transport layer — in the open systems interconnection (OSI) model.

For this requests distribution, the platform uses Round Robin Algorithm.

Round‑robin load balancing is one of the simplest methods for distributing client requests across a group of servers. Going down the list of servers in the group, the round‑robin load balancer forwards a client request to each server in turn.
When it reaches the end of the list, the load balancer loops back and goes down the list again (sends the next request to the first listed server, the one after that to the second server, and so on).

Image showing load balancing.

![Alt text](https://github.com/PushpenderDangi/TCP-load-balancer/blob/main/load%20balancer.png)


**What are iptables?**

Iptables is basically a powerful firewall, which can allow a user to set specific rules to control incoming and outgoing traffic. You can use it to block specific port, IP addresses and much more. 

**The iptables rules can be specified with 3 blocks**

**INPUT** – All packets destined for the host computer.

**OUTPUT** – All packets originating from the host computer.

**FORWARD** – All packets neither destined for nor originating from the host computer, but passing through (routed by) the host computer.


**Nginx and Caddy Web servers.**

Both are web servers but they are used according to need.
Nginx web server is ideal for handling heavy traffic volumes and serving large amounts of static content, but Caddy web servers are suitable for small sites and focus on performance and ease of use.


# VM1 caddy installation

**VM1 :- 192.168.122.19**

**Update and upgrade system commands.**

sudo apt update

sudo apt upgrade

**sudo** - performs the following command with super-user (root) capabilities. Many actions that require modifying system files or installing applications require extra permissions to go through.

**apt** - is a command-line tool which Ubuntu uses to install, remove, and manage software packages.

**update**,**upgrade** - The “sudo apt upgrade” command is used to upgrade the installed packages on your system to their latest versions. On the contrary, the “sudo apt update” command ensures that the system has the latest information about the available packages. But it doesn't install or upgrade any software packages.


**Download and Install Caddy: Download and install Caddy, a web server, using the given below commands.**

**These commands are to be run in the virtual machine that we have created to run the caddy.**


**1. To install caddy first we have to install the curl package on our server. To do this, run this command.**

**If you are in a root directory then run this command.**

apt install curl -y

**apt** - is a command-line tool which Ubuntu uses to install, remove, and manage software packages.

**install** - The install command installs a specified file in a specific place within a file system.


**Or if you are not in a root directory then run this command.**


sudo apt install curl -y

**sudo** - performs the following command with super-user (root) capabilities. Many actions that require modifying system files or installing applications require extra permissions to go through.

**apt** - is a command-line tool which Ubuntu uses to install, remove, and manage software packages.

**install** - The install command installs a specified file in a specific place within a file system.

**curl** - Client URL is a command line tool that enables data exchange between a device and a server through a terminal.

-y flag means "yes" to any prompts, so it automatically agrees to the upgrades without asking for confirmation.






**2. After that install the necessary dependencies to install the caddy web server.**

**Import the official GPG key for caddy. The keys are imported using these commands.**


sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https

curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg

curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-stable.list




**Update your system then install the caddy.**


sudo apt install caddy 

**sudo** - performs the following command with super-user (root) capabilities. Many actions that require modifying system files or installing applications require extra permissions to go through.

**apt** - is a command-line tool which Ubuntu uses to install, remove, and manage software packages.

**install** - The install command installs a specified file in a specific place within a file system.

**Caddy** - It is the name of the file or the resource we want to install in our system.



**It automatically starts caddy as a system service name caddy. The status of which you can check with the following command.**


systemctl status caddy

**systemctl** - Systemctl provides a means to check the status of any systemd service running on dedicated server hosting.

**status** - By issuing the status command, you can gather information about a particular service, including its current state and details.

**Caddy** - The server which we want to check.



**Verify Caddy Installation: Confirm the installation of Caddy by checking its version.**

caddy version

**Caddy** - The server

**version** - This will show the version of the caddy server.




**If everything works for you then it's time to check it by going to the server IP.**

**Go to the web browser of the same virtual machine in which you have installed caddy and in the url bar type:-**

localhost:80



**To install vim in a virtual machine.**


sudo apt install vim

**sudo** - performs the following command with super-user (root) capabilities. Many actions that require modifying system files or installing applications require extra permissions to go through.

**apt** - is a command-line tool which Ubuntu uses to install, remove, and manage software packages.

**install** - The install command installs a specified file in a specific place within a file system.

**Vim** is a text editor in linux like OS. It is used to create and edit text files.


**Then go to the terminal of the virtual machine 1.**

cd /usr/share/caddy

**cd** - cd command is used to change the directory.

**/usr/share/caddy** - It is the path of the file

ls

**ls** - command is used to show a list of files.

sudo vim index.html

**sudo** - performs the following command with super-user (root) capabilities. Many actions that require modifying system files or installing applications require extra permissions to go through.

**Vim** - it s a text editor in linux like OS. It is used to create and edit text files.

**index.html** - It is the file we have created inside the above directory.






**Add below content:-**

<!DOCTYPE html>
<html lang="en">
<head>
    	<meta charset="UTF-8">
    	<meta name="viewport" content="width=device-width, initial-scale=1.0">
    	<title>Hello rohit</title>
</head>
<body>
    	<h1>Hello rohit</h1>
</body>


# For VM2 caddy installation

**VM2 >>>> 192.168.122.103**

**Update and upgrade system commands.**

sudo apt update

sudo apt upgrade

**sudo** - performs the following command with super-user (root) capabilities. Many actions that require modifying system files or installing applications require extra permissions to go through.

**apt** - is a command-line tool which Ubuntu uses to install, remove, and manage software packages.

**update**,**upgrade** - The “sudo apt upgrade” command is used to upgrade the installed packages on your system to their latest versions. On the contrary, the “sudo apt update” command ensures that the system has the latest information about the available packages. But it doesn't install or upgrade any software packages.


**Download and Install Caddy: Download and install Caddy, a web server, using the given below commands.**

**These commands are to be run in the virtual machine that we have created to run the caddy.**


1.  To install caddy first we have to install the curl package on our server. To do this, run this command.

**If you are in a root directory then run this command.**

apt install curl -y

**apt** - is a command-line tool which Ubuntu uses to install, remove, and manage software packages.

**install** - The install command installs a specified file in a specific place within a file system.


**Or if you are not in a root directory then run this command.**


sudo apt install curl -y

**sudo** - performs the following command with super-user (root) capabilities. Many actions that require modifying system files or installing applications require extra permissions to go through.

**apt** - is a command-line tool which Ubuntu uses to install, remove, and manage software packages.

**install** - The install command installs a specified file in a specific place within a file system.

**curl** - Client URL is a command line tool that enables data exchange between a device and a server through a terminal.

-y flag means "yes" to any prompts, so it automatically agrees to the upgrades without asking for confirmation.






**2. After that install the necessary dependencies to install the caddy web server.**

**Import the official GPG key for caddy. The keys are imported using these commands.**


sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https

curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg

curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-stable.list




**Update your system then install the caddy.**


sudo apt install caddy 

**sudo** - performs the following command with super-user (root) capabilities. Many actions that require modifying system files or installing applications require extra permissions to go through.

**apt** - is a command-line tool which Ubuntu uses to install, remove, and manage software packages.

**install** - The install command installs a specified file in a specific place within a file system.

**Caddy** - It is the name of the file or the resource we want to install in our system.



**It automatically starts caddy as a system service name caddy. The status of which you can check with the following command.**


systemctl status caddy

**systemctl** - Systemctl provides a means to check the status of any systemd service running on dedicated server hosting.

**status** - By issuing the status command, you can gather information about a particular service, including its current state and details.

**Caddy** - The server which we want to check.



**Verify Caddy Installation: Confirm the installation of Caddy by checking its version.**

caddy version

**Caddy** - The server

**version** - This will show the version of the caddy server.




**If everything works for you then it's time to check it by going to the server IP.**

**Go to the web browser of the same virtual machine in which you have installed caddy and in the url bar type:-**

localhost:80



**To install vim in a virtual machine.**


sudo apt install vim

**sudo** - performs the following command with super-user (root) capabilities. Many actions that require modifying system files or installing applications require extra permissions to go through.

**apt** - is a command-line tool which Ubuntu uses to install, remove, and manage software packages.

**install** - The install command installs a specified file in a specific place within a file system.

**Vim**i s a text editor in linux like OS. It is used to create and edit text files.


**Then go to the terminal of the virtual machine 1.**

cd /usr/share/caddy

**cd** - cd command is used to change the directory.

**/usr/share/caddy** - It is the path of the file

ls

**ls** - command is used to show a list of files.

sudo vim index.html

**sudo** - performs the following command with super-user (root) capabilities. Many actions that require modifying system files or installing applications require extra permissions to go through.

**Vim** - it s a text editor in linux like OS. It is used to create and edit text files.

**index.html** - It is the file we have created inside the above directory.

**Add below content:-** 

<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>Hello pushpender</title>
</head>
<body>
	<h1>Hello pushpender</h1>
</body>
</html>



# Curl on virtual machines

**I am able to curl pages from both virtual machines.**

Curl command is used to get data or exchange of data between a device and server through a terminal.

**curl http://192.168.122.19**

<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>Hello pushpender</title>
</head>
<body>
	<h1>Hello pushpender</h1>
</body>
</html>


    	



**curl http://192.168.122.103**

<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>Hello pushpender</title>
</head>
<body>
	<h1>Hello pushpender</h1>
</body>
</html>


# Curl on base machine

**I am able to curl from Base Machine.**


**curl http://192.168.122.19**

<!DOCTYPE html>
<html lang="en">
<head>
    	<meta charset="UTF-8">
    	<meta name="viewport" content="width=device-width, initial-scale=1.0">
    	<title>Hello rohit</title>
</head>
<body>
    	<h1>Hello rohit</h1>
</body>



**curl http://192.168.122.103**

<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>Hello pushpender</title>
</head>
<body>
	<h1>Hello pushpender</h1>
</body>
</html>

**Commands that to be run on VM1**

**Now work on Virtual machine 1** 

**a) Enable IP Forwarding:**

**Activate IP forwarding to facilitate traffic routing between interfaces:** 


ubuntu@ubuntu-Standard-PC-Q35-ICH9-2009:~$ sudo vim /etc/sysctl.conf


**uncomment in the file below line.**

 net.ipv4.ip_forward=1

 net.ipv4.conf.all.accept_redirects = 0

 net.ipv4.conf.all.accept_source_route = 0

 net.ipv4.conf.all.log_martians = 1

ubuntu@ubuntu-Standard-PC-Q35-ICH9-2009:~$ sudo sysctl -p
password for ubuntu:
net.ipv4.ip_forward = 1
net.ipv4.conf.all.accept_redirects = 0 
net.ipv6.conf.all.accept_redirects = 0
net.ipv4.conf.all.secure_redirects = 1 
net.ipv4.conf.all.accept_source_route = 0 net.ipv6.conf.all.accept_source_route = 0
net.ipv4.conf.all.log_martians = 1




**Modify source IP for forwarding: this command run on vm1 source**

ubuntu@ubuntu-Standard-PC-Q35-ICH9-2009:~$ sudo iptables -A POSTROUTING  -t nat -p tcp  -d 192.168.122.103 --dport 80 -j SNAT --to-source 192.168.122.19

**Set default connection drop vm1**
This implies that by default, any incoming traffic that doesn't match specific predefined rules will be dropped or rejected. In other words, if the firewall doesn't have a rule that explicitly allows the traffic to pass through, it will block it as a security measure. This approach follows the principle of allowing only known and authorised traffic, enhancing the network's security.

ubuntu@ubuntu-Standard-PC-Q35-ICH9-2009:~$ sudo iptables -t filter -P FORWARD DROP

iptables: This is a command-line utility in Linux used to configure the netfilter firewall (which is built into the Linux kernel). It allows you to set up rules to filter or manipulate network packets.
-t filter: Specifies the table to work with. In this case, it's the 'filter' table, which is the default table used by iptables for packet filtering.
-P FORWARD DROP: This part of the command sets the default policy for the FORWARD chain to DROP. The FORWARD chain is responsible for packets that are being forwarded through the system from one network interface to another. When the policy is set to DROP, it means that by default, any packets that don't match any specific rules in the FORWARD chain will be dropped (i.e., not forwarded).




**Traffic to the Server:**
This involves creating rules that explicitly allow certain types of traffic to reach a designated server. For instance, if you have a server at IP address 192.168.122.44 and it's listening on port 80, you can configure the firewall to accept incoming traffic destined for that server's IP and port. This is done using firewall rules that specify the source, destination, protocol, and port of the allowed traffic.
Allow specific traffic from particular sources and to specific destinations (servers) on specific ports (--dport for destination port and --sport for source port). This creates a controlled and secure network environment where only the specified traffic is permitted to traverse the firewall.

**This command run on Virtual Machine 1**

ubuntu@ubuntu-Standard-PC-Q35-ICH9-2009:~$ sudo iptables -t filter -A FORWARD -d 192.168.122.103 -p tcp --dport 80 -j ACCEPT

iptables: The command-line utility used for managing the netfilter firewall in Linux.

-t filter: Specifies the table to operate on. In this case, it's the 'filter' table, which is the default table for packet filtering.

-A FORWARD: This option appends a rule to the FORWARD chain. The FORWARD chain is responsible for packets being forwarded through the system from one network interface to another.

-d 192.168.122.103: Specifies the destination IP address as 192.168.122.103. This rule will match packets whose destination address is this specific IP.

-p tcp: Specifies the protocol. In this case, it's TCP, which is used for many common types of internet connections.

--dport 80: Specifies the destination port. This rule matches packets whose destination port is 80. Port 80 is commonly used for HTTP web traffic.

-j ACCEPT: This part of the command determines the action to take if a packet matches all the criteria mentioned above. In this case, it's ACCEPT, which means the packets meeting the conditions will be allowed to pass through the firewall.





ubuntu@ubuntu-Standard-PC-Q35-ICH9-2009:~$ sudo iptables -t filter -A FORWARD -s 192.168.122.103 -p tcp --sport 80 -j ACCEPT

# Command to be run on Virtual Machine 2

ubuntu@ubuntu-Standard-PC-Q35-ICH9-2009:~$ sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT


iptables: The command-line utility used for managing the netfilter firewall in Linux.

-A INPUT: This option appends a rule to the INPUT chain. The INPUT chain is responsible for handling packets destined for the local system itself.

-p tcp: Specifies the protocol. In this case, it's TCP, which is used for many common types of internet connections.

--dport 80: Specifies the destination port. This rule matches packets whose destination port is 80. Port 80 is commonly used for HTTP web traffic.

-j ACCEPT: This part of the command determines the action to take if a packet matches all the criteria mentioned above. In this case, it's ACCEPT, which means the packets meeting the conditions will be allowed to pass through the firewall.





**Apply  Random Load Balancing and then  Round Robin:-**

A technique used in network environments to distribute incoming traffic across multiple servers in a random manner. This method is employed to optimise resource utilisation, prevent overload on individual servers, and enhance the overall performance and reliability of a system.
In this case, incoming traffic destined for the IP address 192.168.122.19 on port 80 is subject to random distribution between one destinations (192.168.122.103:80)  The --mode random option ensures that the traffic is distributed based on a randomised algorithm, with different probabilities assigned to each destination (--probability 0.33 and --probability 0.5 in this case).
The goal of applying random load balancing is to distribute traffic unpredictably, achieving a fair distribution of incoming requests among the specified destinations. This way, the servers can collectively handle the load more efficiently, minimising the risk of overloading any single server and contributing to improved performance and fault tolerance.


# Command to be run on Virtual Machine 1


ubuntu@ubuntu-Standard-PC-Q35-ICH9-2009:~$ sudo iptables -A PREROUTING -t nat -p tcp -d 192.168.122.19 --dport 80 \
	-m statistic --mode random --probability 0.33 \
	-j DNAT --to-destination 192.168.122.103:80

-A PREROUTING: This option appends a rule to the PREROUTING chain in the nat table. The PREROUTING chain is used to modify packets as soon as they come in, before any routing decisions are made.

-t nat: Specifies the table to operate on. In this case, it's the 'nat' table, used for Network Address Translation.

-p tcp: Specifies the protocol. In this case, it's TCP.

-d 192.168.122.19 --dport 80: Specifies the destination IP address (192.168.122.19) and the destination port (80). This rule will match incoming TCP packets destined for port 80 on the specified IP address.

-m statistic --mode random --probability 0.33: This part of the command uses the 'statistic' module to introduce randomness into the rule. It sets a probability of 33% (0.33) using the '--probability' option. Essentially, this rule will only match packets randomly with a 33% chance.

-j DNAT --to-destination 192.168.122.103:80: If a packet matches the criteria mentioned above (based on the destination IP and port and the probability condition), this part of the command specifies the action to take. It uses Destination Network Address Translation (DNAT) to redirect the packets to a different destination address and port. In this case, it redirects the packets to 192.168.122.103 on port 80.







**Capture Specific Port Traffic:**

Use tcpdump to monitor network traffic on a specific port:-

Tcpdump command is used to capture and analyze network traffic

sudo tcpdump -i enp1s0 port 80 -n 


**Output of TCP Dump**

**1st curl hit:-**

﻿
ubuntu@ubuntu-Standard-PC-Q35-ICH9-2009:~$ sudo tcpdump -i enp1s0 port 80 -n -v
tcpdump: listening on enp1s0, link-type EN10MB (Ethernet), capture size 262144 bytes
15:14:22.700156 IP (tos 0x0, ttl 64, id 45394, offset 0, flags [DF], proto TCP (6), length 60)
192.168.122.19.57652 > 185.125.190.17.80: Flags [S], cksum 0xb279 (incorrect -> 0x83c4), seq 540801299, win 64240, options [mss 1460, sackOK, TS val 2565047112 ecr 0,nop,wscale 7], length Ⓒ
15:14:22.878938 IP (tos 0x0, ttl 57, id 0, offset 0, flags [DF], proto TCP (6), length 60)
185.125.190.17.80 > 192.168.122.19.57652: Flags [S.], cksum Oxfb8f (correct), seq 274400043, ack 540801300, win 65160, options [mss 1412,sackOK, TS val 4275466846 ecr 2565047112, nop, wscale 7], l
15:14:22.879095 IP (tos 0x0, ttl 64, id 45395, offset 0, flags [DF], proto TCP (6), length 52)
192.168.122.19.57652 > 185.125.190.17.80: Flags [.], cksum Oxb271 (incorrect -> 0x260c), ack 1, win 502, options [nop, nop, TS val 2565047291 ecr 4275466846], length 0
15:14:22.879403 IP (tos 0x0, ttl 64, id 45396, offset 0, flags [DF], proto TCP (6), length 139)
192.168.122.19.57652 > 185.125.190.17.80: Flags [P.], cksum 0xb2c8 (incorrect -> 0x6465), seq 1:88, ack 1, win 502, options [nop, nop, TS val 2565047291 ecr 4275466846], length 87: HTTP, length:
GET/ HTTP/1.1
Host: connectivity-check.ubuntu.com
Accept: */*
Connection: close
15:14:23.086111 IP (tos 0x0, ttl 57, id 2322, offset 0, flags [DF], proto TCP (6), length 241)
185.125.190.17.80 > 192.168.122.19.57652: Flags [P.], cksum 0x6720 (correct), seq 1:190, ack 88, win 508, options [nop, nop, TS val 4275467035 ecr 2565047291], length 189: HTTP, length: 189
HTTP/1.1 204 No Content
server: nginx/1.14.0 (Ubuntu)
date: Tue, 07 Nov 2023 09:44:23 GMT
x-cache-status: from content-cache-il3/0
x-networkmanager-status: online
connection: close
15:14:23.086135 IP (tos 0x0, ttl 57, id 2323, offset 0, flags [DF], proto TCP (6), length 52)
185.125.190.17.80 > 192.168.122.19.57652: Flags [F.], cksum 0x2434 (correct), seq 190, ack 88, win 508, options [nop, nop, TS val 4275467035 ecr 2565047291], length 0
15:14:23.086231 IP (tos 0x0, ttl 64, id 45397, offset 0, flags [DF], proto TCP (6), length 52)
192.168.122.19.57652 > 185.125.190.17.80: Flags [.], cksum 0xb271 (incorrect -> 0x236d), ack 190, win 501, options [nop, nop, TS val 2565047498 ecr 4275467035], length 0
15:14:23.086419 IP (tos 0x0, ttl 64, id 45398, offset 0, flags [DF], proto TCP (6), length 52)
192.168.122.19.57652 > 185.125.190.17.80: Flags [F.], cksum 0xb271 (incorrect -> 0x236b), seq 88, ack 191, win 501, options [nop, nop, TS val 2565047498 ecr 4275467035], length 0
15:14:23.295849 IP (tos 0x0, ttl 57, id 2324, offset 0, flags [DF], proto TCP (6), length 52)
185.125.190.17.80 > 192.168.122.19.57652: Flags [.], cksum 0x2295 (correct), ack 89, win 508, options [nop, nop, TS val 4275467242 ecr 2565047498], length 0

9 packets captured
9 packets received by filter
0 packets dropped by kernel












**2nd curl Hit:-**

ubuntu@ubuntu-Standard-PC-Q35-ICH9-2009:~$ sudo tcpdump -i enp1s0 port 80 -n -v
tcpdump: listening on enp1s0, link-type EN10MB (Ethernet), capture size 262144 bytes
15:14:22.700156 IP (tos 0x0, ttl 64, id 45394, offset 0, flags [DF], proto TCP (6), length 60)
192.168.122.19.57652 > 185.125.190.17.80: Flags [S], cksum 0xb279 (incorrect -> 0x83c4), seq 540801299, win 64240, options [mss 1460, sackOK, TS val 2565047112 ecr 0,nop,wscale 7], length Ⓒ
15:14:22.878938 IP (tos 0x0, ttl 57, id 0, offset 0, flags [DF], proto TCP (6), length 60)
185.125.190.17.80 > 192.168.122.19.57652: Flags [S.], cksum Oxfb8f (correct), seq 274400043, ack 540801300, win 65160, options [mss 1412,sackOK, TS val 4275466846 ecr 2565047112, nop, wscale 7], l
15:14:22.879095 IP (tos 0x0, ttl 64, id 45395, offset 0, flags [DF], proto TCP (6), length 52)
192.168.122.19.57652 > 185.125.190.17.80: Flags [.], cksum Oxb271 (incorrect -> 0x260c), ack 1, win 502, options [nop, nop, TS val 2565047291 ecr 4275466846], length 0
15:14:22.879403 IP (tos 0x0, ttl 64, id 45396, offset 0, flags [DF], proto TCP (6), length 139)
192.168.122.19.57652 > 185.125.190.17.80: Flags [P.], cksum 0xb2c8 (incorrect -> 0x6465), seq 1:88, ack 1, win 502, options [nop, nop, TS val 2565047291 ecr 4275466846], length 87: HTTP, length:
GET/ HTTP/1.1
Host: connectivity-check.ubuntu.com
Accept: */*
Connection: close
15:14:23.086111 IP (tos 0x0, ttl 57, id 2322, offset 0, flags [DF], proto TCP (6), length 241)
185.125.190.17.80 > 192.168.122.19.57652: Flags [P.], cksum 0x6720 (correct), seq 1:190, ack 88, win 508, options [nop, nop, TS val 4275467035 ecr 2565047291], length 189: HTTP, length: 189
HTTP/1.1 204 No Content
server: nginx/1.14.0 (Ubuntu)
date: Tue, 07 Nov 2023 09:44:23 GMT
x-cache-status: from content-cache-il3/0
x-networkmanager-status: online
connection: close
15:14:23.086135 IP (tos 0x0, ttl 57, id 2323, offset 0, flags [DF], proto TCP (6), length 52)
185.125.190.17.80 > 192.168.122.19.57652: Flags [F.], cksum 0x2434 (correct), seq 190, ack 88, win 508, options [nop, nop, TS val 4275467035 ecr 2565047291], length 0
15:14:23.086231 IP (tos 0x0, ttl 64, id 45397, offset 0, flags [DF], proto TCP (6), length 52)
192.168.122.19.57652 > 185.125.190.17.80: Flags [.], cksum 0xb271 (incorrect -> 0x236d), ack 190, win 501, options [nop, nop, TS val 2565047498 ecr 4275467035], length 0
15:14:23.086419 IP (tos 0x0, ttl 64, id 45398, offset 0, flags [DF], proto TCP (6), length 52)
192.168.122.19.57652 > 185.125.190.17.80: Flags [F.], cksum 0xb271 (incorrect -> 0x236b), seq 88, ack 191, win 501, options [nop, nop, TS val 2565047498 ecr 4275467035], length 0
15:14:23.295849 IP (tos 0x0, ttl 57, id 2324, offset 0, flags [DF], proto TCP (6), length 52)
185.125.190.17.80 > 192.168.122.19.57652: Flags [.], cksum 0x2295 (correct), ack 89, win 508, options [nop, nop, TS val 4275467242 ecr 2565047498], length 0

9 packets captured
9 packets received by filter
0 packets dropped by kernel



# Load Testing on base machine

**This command is used to install set of utility program for web servers**

sudo apt install apache2-utils 


**Install ab command at local for load testing**

Install apache2-utils for ab command for Load Testing
run load test using ab command :- 









ubuntu@ubuntu-Standard-PC-Q35-ICH9-2009:~$  ab -n 1000 -c 100 http://192.168.122.19:80/

pushpender@pushpender:~$ sudo ab -n 1000 -c 100 http://192.168.122.19:80/

This is ApacheBench, Version 2.3 <$Revision: 1879490 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking 192.168.122.19 (be patient)
Completed 100 requests
Completed 200 requests
Completed 300 requests
Completed 400 requests
Completed 500 requests
Completed 600 requests
Completed 700 requests
Completed 800 requests
Completed 900 requests
Completed 1000 requests
Finished 1000 requests


Server Software:    	Caddy
Server Hostname:    	192.168.122.19
Server Port:        	80

Document Path:      	/
Document Length:    	234 bytes

Concurrency Level:  	100
Time taken for tests:   1.078 seconds
Complete requests:  	1000
Failed requests:    	638
   (Connect: 0, Receive: 0, Length: 638, Exceptions: 0)
Total transferred:  	459018 bytes
HTML transferred:   	241018 bytes
Requests per second:	927.81 [#/sec] (mean)
Time per request:   	107.781 [ms] (mean)
Time per request:   	1.078 [ms] (mean, across all concurrent requests)
Transfer rate:      	415.90 [Kbytes/sec] received

Connection Times (ms)
          	min  mean[+/-sd] median   max
Connect:    	0	3   3.1  	2  	13
Processing: 	1   25  24.8 	13 	109
Waiting:    	0   24  24.3 	12  	93
Total:      	1   28  24.6 	20 	111

Percentage of the requests served within a certain time (ms)
  50% 	4
  66% 	6
  75% 	7
  80% 	8
  90% 	11
  95% 	1015
  98% 	1026
  99% 	1029
 100%	1034 (longest request)



## curl  Output :- 

## curl http://192.168.122.19
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hello rohit</title>
</head>
<body>
    <h1>Hello rohit!</h1>
</body>
</html>



# curl http://192.168.122.19
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hello pushpender</title>
</head>
<body>
    <h1>Hello pushpender!</h1>
</body>
</html>

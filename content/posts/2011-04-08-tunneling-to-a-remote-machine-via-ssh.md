---
date: 2011-04-08 13:57:00
title: Tunneling to a remote machine via ssh
tags:
- ssh
- linux
---



Update: For the most part, I want to tunnel from the server to the local machine. This only works with the localhostname set to localhost, for example to tunnel a mysql server to the current machine port 10101:

``` bash
ssh -L 10101:localhost:3306 user@remoteserver.com -N
```

The localhost is important, and in this case the mysql client (on the local machine) has to have the hostname set to 127.0.0.1 (for some reason localhost doesnt work). 
Ive been trying to access my works MySQL database from outside the universities firewall and was having issues. Im still having issues but thats another story: Ill get to that.
Some definitions: the local machine is my machine accessing the server from outside the firewall, and the server machine is the one running the server and is behind the firewall.
My initial test was to run a rails server (which runs on port 3000) on the server machine. It serves a basic test page just to show it works. 
To set up a tunnel, I used the following command:

``` bash
ssh -L localport:localhostname:remoteport user@remoteserver -N
```

The -L means set up a tunnel, and -N means to not actually log in to the server. For this example I mapped port 3000 on the server machine to port 10101 on the local machine by running:

``` bash
ssh -L 10101:hostname:3000 user@remoteserver -N
```

Then went to my web browser and typed localhost:10101 and got a simple rails start page. Et voila!

To access my MySQL server, this worked ok but the permissions for my username had to be altered as me@localhost (from the tunnel) could not access the databases owned by me@remoteserver due to the changing username. This is a separate issue so I will sort this out later!

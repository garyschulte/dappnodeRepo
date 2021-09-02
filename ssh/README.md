# openssh dappnode package

![ak-47]( https://memegenerator.net/img/instances/56811901/ak-47-the-very-best-there-is-when-you-absolutely-positively-got-to-kill-every-motherfucker-in-the-ro.jpg)

## The nuclear option for remote access to a dappnode

If you have a remote dappnode, and do not have access to ssh, you can install this incredibly dangerous package to allow you to use dappnode as a bastion host to get to the rest of your remote network and to port forward.

## Build and Install

tl/dr:
* connect to your [dappnode vpn](https://docs.dappnode.io/user-guide/ui/access/vpn/)
* clone this repo, `git clone https://github.com/garyschulte/dappnoderepo`
* `cd dappnodeRepo/ssh`
* `npm install -g @dappnode/dappnodesdk`
* `dappnodesdk build`
* follow the http link at the end of the build to install your package


## Connecting to the node

You will be able to connect to a docker instance that is running openssh.  The ssh port is exposed as `2222`, so for example:
```
xx  ssh unsafe@ssh.dappnode -p 2222
unsafe@172.33.0.14's password:
Welcome to OpenSSH Server
:w

2d4b927201e3:~$
````

Once you are there, you should be able to ssh to any other host on your network, e.g.:
```
2d4b927201e3:~$ ssh dappnode@192.168.0.110
dappnode@192.168.0.110's password:
Linux dappnode 5.10.0-8-amd64 #1 SMP Debian 5.10.46-4 (2021-08-03) x86_64
 ___   _             _  _         _
|   \ /_\  _ __ _ __| \| |___  __| |___
| |) / _ \| '_ \ '_ \ .  / _ \/ _  / -_)
|___/_/ \_\ .__/ .__/_|\_\___/\__,_\___|
          |_|  |_|
Last login: Wed Sep  1 22:13:32 2021 from 172.33.0.14
Wait until DAppNode initializes (press ctrl+c to stop)
Connect to DAppNode through Wi-Fi using the following credentials:
      - SSID=some_DAppNodeWIFI_ssid
      - WPA_PASSPHRASE=some_example_password

Access your DAppNode at http://my.dappnode

Discover more ways to connect to your DAppNode at http://welcome.dappnode

dappnode@dappnode:~$```


### port forwarding to a different host in your network

If you want to forward a local port over to a different host in your network (like say a router web server at 192.168.1.1) connect with ssh thusly:
```ssh unsafe@ssh.dappnode -p 2222 -L 8080:192.168.1.1:80```

and then in your browser you can hit [http://localhost:8080](http://localhost:8080)


## Do Not Leave This Package Installed
It is bad security hygiene at best.  But handy in a pinch.

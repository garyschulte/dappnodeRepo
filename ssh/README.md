# openssh dappnode package

## The nuclear option for remote access to a dappnode

If you have a remote dappnode and a VPN connecetion to it but do not have access to ssh or other necessary remote network resources, you can install this incredibly dangerous package to allow you to use dappnode as a bastion host to get to the rest of your remote network.

## Build and Install

if you have decided you need this, here is a quick crib sheet to build and install this package.  You will need to have docker installed, a vpn connection to your dappnode, and git:

* connect to your [dappnode vpn](https://docs.dappnode.io/user-guide/ui/access/vpn/)
* clone this repo, `git clone https://github.com/garyschulte/dappnoderepo`
* `cd dappnodeRepo/ssh`
* `npm install -g @dappnode/dappnodesdk`
* `dappnodesdk build`
* follow the http link at the end of the build to install your package


### example build output:

```
➜  ssh git:(main) ✗ dappnodesdk build
  ✔ Verify connection
  ✔ Create release dir
  ✔ Copy files and validate
  ✔ Build docker image
  ✔ Save and compress image
  ✔ Upload release to IPFS node
  ✔ Save upload results

  DNP (DAppNode Package) built and uploaded
  Release hash : /ipfs/QmW3zUxqQe4UYsntBLwo3ESYvb5XNgnMPFkmwKNCkw4G5e
  http://my.dappnode/#/installer/%2Fipfs%2FQmW3zUxqQe4UYsntBLwo3ESYvb5XNgnMPFkmwKNCkw4G5e
```


## Connecting to the node

You will be able to connect to a docker instance that is running openssh.  This is the information you will need to connect to the running container:

* hostname: ssh.dappnode
* ssh port: 2222
* username: unsafe
* password: password 

So for example:

```
➜  ssh git:(main) ✗ ssh unsafe@ssh.dappnode -p 2222
The authenticity of host '[ssh.dappnode]:2222 ([172.33.0.14]:2222)' can't be established.
ECDSA key fingerprint is SHA256:0YFanY9tcWtT4DL9iOt7fMcudMw11s4XBnI8zdAZ/jk.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[ssh.dappnode]:2222,[172.33.0.14]:2222' (ECDSA) to the list of known hosts.
unsafe@ssh.dappnode's password:
Welcome to OpenSSH Server

337cb2cacf64:~$
```

et voila, you are in.  Lets call this the "bastion host"

### Connecting to the primary dappnode ssh daemon

First off, [ensure ssh is enabled in your admin console](http://my.dappnode/#/system/advanced).  The "bastion host" we are connecting to is a containerized ssh daemon.  There isn't much happening in the container, it is just there as a way for us to get onto the host.  In order to get onto the dappnode host "proper", you will need to ssh AGAIN to the dappnode ssh daemon (or to any other host on your network) e.g.:

```
2d4b927201e3:~$ ssh dappnode@192.168.1.100
dappnode@192.168.1.100's password:
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

dappnode@dappnode:~$
```


### Port forwarding to a different host in your network

The reason I embarked on this journey was so that I could get a web browser interface on the router where I was hosting my dappnode machine.  This is actually pretty easy to do once we have the ssh "bastion host" running on dappnode.  We can simply forward a local port over to the target host and port (in this case a router web admin console at http://192.168.1.1).  So when we connect to the bastion host we pass an additional param to tell it what machine we want to forward to, thusly:

```ssh unsafe@ssh.dappnode -p 2222 -L 8080:192.168.1.1:80```

and then FINALLY, in your local browser you can hit [http://localhost:8080](http://localhost:8080) and do the router fiddling you wanted to do to begin with.


## Do Not Leave This Package Installed

It is bad security hygiene at best.  A backdoor into your network and your dappnode at worst.  Be sure to remove the package when you are done doing what you need to do.  Caveat Emptor!

![hold onto your butts](https://memegenerator.net/img/instances/74885411/hold-on-to-your-butts.jpg)

### ssh_jupyter_port_tunneling_example

The other day I wanted to connect to a jupyter server in my work computer, but first I had to go through the server, and then make another ssh to our local ip.

In these cases, it was not clear to me how to start a jupyter server for example on my computer and then access to it from outside the network. Specially because I needed to connect to a specific port of the localhost to be able to connect to the jupyter server.

In order to do this we need to forward ports.

#### Example

- our_server--> user: human
- random_local_pc -->  user: duman

we are going to forward the port 9999 of the local_pc to the port 7777 in the pc from where we are connecting to random_local_pc

```
ssh -L 7777:[::1]:1234 human@ssh.our_server.com.ar -t ssh -L 1234:[::1]:9999 duman@random_local_pc
```

In case we do not know the domain of the server or we want to use ip addresses we can do the same just replacing the ips

- our_server (200.20.20.20) --> user: human
- random_local_pc (192.168.1.25)-->  user: duman

```
ssh -L 7777:[::1]:1234 human@200.20.20.20 -t ssh -L 1234:[::1]:9999 duman@192.168.1.25
```

In both cases we are going to be ask to prompt our passwords, and thats it, now in the pc where you have executed the command you can go to your explorer and go to `localhost:7777` and you will be connecting to the server in random_local_pc.


Additionally, if you do this frequently, you just can create a file containing this line, give to it execution permission.

So we generate a file:
```
 echo "ssh -L 7777:[::1]:1234 human@ssh.our_server.com.ar -t ssh -L 1234:[::1]:9999 duman@random_local_pc" > random_local_pc_port_tunneling
```
and then we provide execution permission,
```
chmod +x ./random_local_pc_port_tunneling
```
and that's it, we now can simply run `./random_local_pc_port_tunneling`  to forward the localhost.

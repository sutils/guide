NFS Security By SSH Tunnel
===

## Requirement

* nfs>=4
* autossh
* openssh

## Configure Export On NFS Server

* add export folder and change access

```.sh
mkdir /nfs/testnfs
chmod -R 600 /nfs/testnfs
```

* add export on `/etc/exports`

```.sh
/nfs/testnfs    127.0.0.1(rw,fsid=0,sync,insecure_locks,insecure,no_root_squash)
```

* execute `exportfs -a`

* add autologin user

```.sh
useradd autologin
```

## Configure NFS Client

* create autologin key

```.sh
ssh-keygen -t rsa  -P '' -f ~/.ssh/<host>_rsa
```

* copy public key to nfs server

```.sh
scp ~/.ssh/<host>_rsa.pub <host>:~/.ssh/authorized_keys
```

* add auto login configure on nfs client to `~/.ssh/conf`

```.txt
Host <host>
  IdentityFile ~/.ssh/<host>_rsa
  User autologin
```

* testing autologin by `ssh <host>`

* running port forward by ssh tunnel

```.sh
autossh -fNv -L 3049:localhost:2049 <host>
```

note: must run again when server is rebooted.

* mount the nfs export

```.sh
mount -t nfs4 -o port=3049 <host>:/ /home/nfs
```

* enjoy it

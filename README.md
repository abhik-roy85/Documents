# Linux Containers(lxc)

## Installations

```
sudo apt install lxc

sudo apt install lxd
```

## Initialization

```
lxd init
```
Choose all defaults

## List all available images:

```
lxc image alias list ubuntu:
```

### Search for specific image:
```
lxc image alias list ubuntu: '18.04'

lxc image alias list images: 'alpine'
```

## Launch a specific image:

```
lxc launch <Image name> <Container name>

lxc launch images:alpine/edge/default alpine1
or
lxc launch ubuntu:18.04 ubuntu1
```

## List of available containers

```
lxc list
```

**Detailed list:**
```
lxc list -c n,s,4,image.description:image
```

## Get info of a launched container:

```
lxc info ubuntu1
```

## Run a command on a container:

```
lxc exec <container name> <command>

lxc exec ubuntu1 hostname
```

## Entering shell environment of a container

```
lxc exec ubuntu1 bash

lxc exec ubuntu1 sh
```

## Starting a stopped container:

```
lxc start <container name>

lxc start ubuntu2
```

## Stopping a container:

```
lxc stop <container name>

lxc stop alpine1
```

## Deleting a container:

```
lxc delete <container name>

lxc delete alpine1
```

## Making a copy of a container:

```
lxc copy <src Container> <new dst container>

lxc copy ubuntu1 ubuntu2
```

## Pushing a file to a container:

```
lxc file push <Source file> <Container name>/<Destination location>

lxc file push /home/abroy/.ssh/authorized_keys ubuntu1/root/.ssh/authorized_keys
```
Note: Push the authorized_keys file to a container to make ssh work.

## ssh into a container:
```
ssh root@<container ip address>
```

## Pull a file from a container:

```
lxc file pull <Container name>/<Source file> <Destination location>

lxc file pull ubuntu1/root/.ssh/authorized_keys /home/abroy/.ssh/authorized_keys
```

## Edit a file in a container without sshing into it:

```
lxc file edit <Container name>/<File path>

lxc file edit ubuntu1/root/.ssh/authorized_keys
```

## LXC Snapshots:

### Taking a snapshot of a container:
```
lxc snapshot ubuntu1
```

'lxc list' shows snapshot of a container.

### Get details of the snapshot from:
```
lxc info ubuntu1
```

### Restoring to the snapshot:
```
lxc restore <container name> <snapshot name from 'lxc info'>

lxc restore ubuntu1 snap0
```

### Deleting a snapshot:
```
lxc delete ubuntu1/snap0
```



## Connecting LXC network interface to home LAN so that it connects to the home router:

```
lxc config device add ubuntu1 <container interface name> nic nictype=bridged parent=<local bridged adapter name> name=<local interface name>

lxc config device add ubuntu1 eth0 nic nictype=bridged parent=br0 name=eth0
```

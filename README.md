# Network-Namespace-Vagrant
Two namespace connected via virtual ether cable

## Install Virtualbox

```bash
sudo apt update
sudo apt install virtualbox
```

## Install Vagrant

```bash
curl -O https://releases.hashicorp.com/vagrant/2.2.9/vagrant_2.2.9_x86_64.deb
sudo apt install ./vagrant_2.2.9_x86_64.deb
vagrant --version
```

### Getting Started with Vagrant

```bash
mkdir ~/my-vagrant-project
cd ~/my-vagrant-project
```

```bash
vagrant init centos/8
vagrant up
```

```bash
vagrant ssh
vagrant suspend
vagrant resume
vagrant reload
vagrant destroy
```

# Network Namespace

**Create Network Namespace**

```bash
ip netns add red
ip netns add green
ip netns list
```

**Create virtual cable**

```bash
ip link add reth type veth peer name geth
```

**Set namespace with virtual cable**

```bash
ip link set reth netns red
ip link set geth netns green
```

**Get inside the netns**

```bash
ip netns exec red bash
```

**Check reth has ip or not**

```bash
ip addr
```

**Add IP address to each interface**

```bash
ip link set dev reth up
ip addr add 10.10.0.2/32 dev reth
```

**Add the interface to route**

```bash
ip route add 10.10.0.3/32 dev reth
```

---

_Similar for green netns with geth_

```bash
ip netns exec green bash
ip addr
ip link set dev geth up
ip addr add 10.10.0.3/32 dev geth
ip route add 10.10.0.2/32 dev geth
exit
```

### PING

**Ping from red**

```bash
ip netns exec red bash
ping 10.10.0.3
exit
```

**Ping from green**

```bash
ip netns exec green bash
ping 10.10.0.2
exit
```

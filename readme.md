# Home Lab

## Prerequisites

### Networking

Network interfaces need to be set up before connecting via ssh.  The following configured the quad-port Intel NIC.  These settings should be placed in `/ect/network/interface`

```
# interfaces(5) file used by ifup(8) and ifdown(8)
auto lo
iface lo inet loopback

auto enp4s0f0
iface enp4s0f0 inet manual
  bond-master bond0

auto enp4s0f1
iface enp4s0f1 inet manual
  bond-master bond0

auto enp4s0f2
iface enp4s0f2 inet manual
  bond-master bond0

auto enp4s0f3
iface enp4s0f3 inet manual
  bond-master bond0

auto bond0
iface bond0 inet manual
  bond-mode 4
  bond-miimon 100
  bond-lacp-rate fast
  bond-slaves enp4s0f0 enp4s0f1 enp4s0f2 enp4s0f3

auto bond0.100
iface bond0.100 inet static
  address 192.168.81.14
  netmask 255.255.255.240
  vlan-raw-device bond0

auto bond0.101
iface bond0.101 inet static
  address 192.168.81.18
  netmask 255.255.255.240
  gateway 192.168.81.17
  dns 8.8.8.8
  vlan-raw-device bond0

```

### Ansible Requirements

Ansible requires python:

```
sudo apt install -y python
```


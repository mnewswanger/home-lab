# Home Lab

## Prerequisites

### Disk Encryption

#### Setup

For non-system drives, encryption can be set up using luks.

```
sudo cryptsetup luksFormat -c aes-xts-plain64 -s 512 -h sha512 /dev/<disk>
```

Generate a key:

```
sudo dd if=/dev/urandom of=/etc/luks-keys/<key_name> bs=512 count=8
```

Add the key to the disk:

```
sudo cryptsetup -v luksAddKey /dev/<disk> /etc/luks-keys/<key_name>
```

#### Automount

Get the device id and add to `/etc/crypttab` in the format `<name> UUID=<uuid> /etc/luks-keys/<key-name> luks`

```
sudo cryptsetup luksDump /dev/<disk> | grep "UUID"
```

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


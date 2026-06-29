# Basic Linux Tasks

## Change Hostname

1. Change the hostname using `hostnamectl` command.
2. Then update the `/etc/hosts` file.

This example shows to change the hostname to `ns1`.

```sh
sudo hostnamectl set-hostname ns1
sudo nano /etc/hosts
```

Use `hostnamectl` command to view the status.
```sh
hostnamectl
```

```yaml
   Static hostname: ns1
         Icon name: computer-vm
           Chassis: vm
        Machine ID: 62447195dfa24f14b70431654bceefbd
           Boot ID: 48f9e70f60dd4a7da20ca1fbf7332320
    Virtualization: oracle
  Operating System: Ubuntu 20.04 LTS
            Kernel: Linux 5.4.0-26-generic
      Architecture: x86-64
```

```sh
cat /etc/hosts
```
```vim
127.0.0.1 localhost
127.0.1.1 ns1
#The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```

```sh
cat /etc/hostname
```
```vim
ns1
```

---

## Change Machine ID

After cloning from an existing you may find that the machines ID of the new image is same as the old one. Change the machine ID. It&#39;s stored in `/etc/machine-id` file.

```sh
rm /etc/machine-id
systemd-machine-id-setup
```

If this does not create unique ID, just manually change the value (e.g. using nano).

---

## IP Setting: Static and Dynamic

1. Create/Modify the .yaml file in `/etc/netplan` directory to configure IP addresses in the interfaces. The exact name of the .yaml file is fixed. There may be more then one file in this directory. Example configuration is given below. Here the renderer is **networkd**.

```sh
vi /etc/netplan/01-netcfg.yaml
```
```yaml
# This file describes the network interfaces available on your system
# For more information, see netplan(5).
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      dhcp4: no
      addresses: [192.168.144.12/24]
      gateway4: 192.168.144.1
      nameservers:
        addresses: [192.168.144.1, 8.8.8.8]
    enp0s8:
      dhcp4: yes
```

2. Use `sudo netplan apply` command to apply the changes. Verify the change using `ip address` command.

```sh
netplan apply
ip address
```

3. To `start` / `restart` / `stop` to check status of **networkd** service, use the following.

```sh
systemctl status system-networkd
```

---

## SSH Server Installation

To get SSH access to a linux machine, you can install `openssh-server`. You can see the status of the server with systemctl command.

```sh
apt-get install opensh-server
systemctl status ssh
```
![ssh service](images/img-ssh1.png)

## Checking Status of Services

1. You can use `service --status-all` command or `systemctl list-unit-files` command to view the status all the installed services.

```sh
service --status-all
systemctl list-unit-files
```
![service status](images/img-service1.png)
![service status](images/img-service2.png)

1. Use `systemctl status  <servicename>` command or `service <servicename>  status` command to see the status of a service. The following example shows the status of ssh service.

```sh
service ssh status
systemctl status ssh
systemctl list-unit-files | grep -i ssh.service
```
![service status](images/img-service3.png)

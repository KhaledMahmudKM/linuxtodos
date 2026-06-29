# NAT Setup

## Enable NAT in Linux

To set up a linux server to perform NAT service, follow the steps below.

## Enable IP Forwarding (Routing)

For just short term (until next boot), set the value of `/proc/sys/net/ipv4/ip_forward` file to 1.

```sh
echo 1 > /proc/sys/net/ipv4/ip_forward
```


For permanent change (persistent after reboot), edit `/etc/sysctl.conf` file. Uncomment the line containing `net.ipv4.ip_forward = 1`.

## Configure IPTABLES

1. Add a `POSTROUTING` rule to nat table to translate IP addresses coming from the given source IP range. Specify the output interface.

```sh
iptables -t nat -A POSTROUTING -s 10.99.99.0/24 -o enp0s3 -j MASQUERADE
```


2. Check the new rule.

```sh
iptables -t nat -L
```


3. Make the rule persistent with boot. First install the package called iptables-persistent. The save the rule(s) in `/etc/iptables/rules.v4` file. The iptables-persistent tool reads this file during boot.

```sh
apt install iptables-persistent
iptables-save > /etc/iptables/rules.v4
```



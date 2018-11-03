# Ubuntu
All knowledge about Ubuntu

=========== BUILD VIRTUAL NETWORK OF UBUNTU SERVER 16.04 USING VIRTUAL BOX

# http://coding4streetcred.com/blog/post/VirtualBox-Configuring-Static-IPs-for-VMs
# https://askubuntu.com/questions/293816/in-virtualbox-how-do-i-set-up-host-only-virtual-machines-that-can-access-the-in

1. ON VM:

+ Virutal Box: config network to adaper 1: Host-only.

+ RUN: ls /sys/class/net

+ RUN: sudo nano /etc/network/interfaces
  # The primary network interface
  auto enp0s3
  iface enp0s3 inet static
    address 192.168.56.102
    netmask 255.255.255.0
    gateway  192.168.56.1 # IPv4 Adress of vboxnet0 (Host-only)
    broadcast 192.168.56.255
    dns-nameservers 8.8.8.8 8.8.4.4 # Using google's DNS servers
	
+ RUN: sudo reboot

+ RUN: ping vnexpress.net # to check if VM can access Internet.

                            ==================
	
2. ON HOST:
# Setup IP Forwarding.
# https://kyrofa.com/posts/virtualbox-internet-access-with-host-only-network

$ sudo iptables -A FORWARD -o wlan0 -i vboxnet0 -s 192.168.56.0/24 -m conntrack --ctstate NEW -j ACCEPT
$ sudo iptables -A FORWARD -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
$ sudo iptables -t nat -F POSTROUTING
$ sudo iptables -t nat -A POSTROUTING -o wlan0 -j MASQUERADE
$ sudo sh -c "echo 1 > /proc/sys/net/ipv4/ip_forward"
	
$ sudo ssh baotq@server-01 # to check if HOST can access to GUEST (Configed in /etc/hosts)

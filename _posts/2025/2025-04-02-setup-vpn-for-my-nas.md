---
layout: post
title: "Setup VPN for my NAS"
date: 2025-04-02
author: CHRiSTeeNA
tags: ["NAS", "VPN"]
categories: blog
---

## Accessing my NAS from anywhere
After hitting the ground running, I want to have access to my Nas from outside my house so that I can backup my stuff anywhere, anytime.

## Install WireGuard on NAS
First, update repository and download WireGuard
```
sudo apt update && sudo apt install wireguard
```
Then, enable ip forwarding
```
echo "net.ipv4.ip_forward = 1" | sudo tee -a /etc/sysctl.conf
echo "net.ipv6.conf.all.forwarding = 1" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```
After that, generate public and private key for authentication. Just like how we normally do with setting up ssh, just via WireGuard way.
```
wg genkey | tee ~/path/to/privatekey | wg pubkey > ~/path/to/publickey
```
Save those keys for later use, and **make sure to not share the private key**!

Now, modify the config file of WireGuard
```
sudo nano /etc/wireguard/wg0.conf
```
The configuration will be something like below
```
[Interface]
Address = 10.0.0.1/24
SaveConfig = true
ListenPort = 51820
PrivateKey = <YOUR_PRIVATE_KEY>  # Replace with the key from "privatekey"
PostUp = iptables -A FORWARD -i wg0 -j ACCEPT; iptables -A FORWARD -o wg0 -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
PostDown = iptables -D FORWARD -i wg0 -j ACCEPT; iptables -D FORWARD -o wg0 -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE
```
After saving the config file, enable and start WireGuard
```
sudo systemctl start wg-quick@wg0
sudo systemctl enable wg-quick@wg0
```
## Configure port forwarding on Router
To route traffic to the NAS, it should be forwarded to port 51820
So, on the Wi-Fi router admin page (normally 192.168.10.1 or the ip written at the back of your router), look for port mapping setting, and add the required details onto it.
```
Priority: The priority to forward the traffic if there is multiple forward settings, normally number 1, 2, 3 ~ n
LAN Host: <private IP of the NAS>
Port to Forward from: 51820, if the router only provide certain range of ports, choose any port. This is for the client side who will be accessing the NAS through this router (eg: <router public ip>:<this port>)
Port to Forward to: 51820
```

## Install WireGuard on Client (Computer or phone)
Simply download from [WireGuard](https://www.wireguard.com/install/) site.
I am using Windows PC to connect to my NAS, so I just open the GUI of WireGuard, add empty tunnel, and insert the details below.
```
[Interface]
Address = 10.0.0.2/24
PrivateKey = <CLIENT_PRIVATE_KEY>  # Generate a new key pair for the client
DNS Server = 8.8.8.8 # Google's DNS server

[Peer]
PublicKey = <SERVER_PUBLIC_KEY>  # Use the public key from your NAS
Endpoint = <YOUR_PUBLIC_IP>:51820  # Replace with your router’s public IP
AllowedIPs = 0.0.0.0/0, ::/0  # Route all traffic through VPN
PersistentKeepalive = 25
```

Then, on the server side, add the client to the config file
```
sudo wg set wg0 peer <CLIENT_PUBLIC_KEY> allowed-ips 10.0.0.2/32
```

## Connect to NAS from other network
Now let's make the magic happen!
Open WireGuard, import `wg-client.conf`, and then click activate.

On the file explorer, I am able to connect to my NAS on `\\10.0.0.1/24`!

## After Thoughts
I was able to made it work the first time and I was so pumped!

Although connecting to my NAS using IP address isn't the most intuitive way, but it looks cool when I do it in front of my friends and family! I feels like I am the best hacker they know!
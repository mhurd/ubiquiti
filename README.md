# ubiquiti
Notes on Ubiquiti networking equipment (UniFi, EdgeRouter etc.)

## Enabling dnsmasq for DHCP server
This enables local DNS hostname resolution.
See: https://help.ubnt.com/hc/en-us/articles/115002673188-EdgeRouter-Using-dnsmasq-for-DHCP-Server

Enable dnsmsq:
```bash
configure
set service dhcp-server use-dnsmasq enable 
commit
save
exit
```
Set system name-server to loop back to the router itself, which will forward requests to the DNS servers set in the DNS forwarding settings by entering the following:
```bash
set system name-server 127.0.0.1
```
Set global name-servers to resolve all external resolutions.
```bash
set service dns forwarding name-server 1.1.1.1
```
Set DNS forwarding listen-on address for all LAN interfaces including VLANs (your specific interfaces may be different, I have three ports setup as a switch and no VLANs).
```bash
set service dns forwarding listen-on eth0
set service dns forwarding listen-on switch0
```
Increase DNS forwarding cache-size (optional).
```bash
set service dns forwarding cache-size 4000 
```
Set system domain-name (use your own name here).
```bash
set system domain-name mhurd.local
```
Set domain-name for dhcp-servers, use your shared-network names and subnets (you can find thes ein the config tree under the dhcp section).
```bash
set service dhcp-server shared-network-name switch subnet 192.168.2.0/24 domain-name mhurd.local
set service dhcp-server shared-network-name wired-eth0 subnet 192.168.1.0/24 domain-name mhurd.local
```
Set static host mapping for each local device, you may already have done this if you have set up static mappings, the name you assign will be the hostname.
```bash
set service dhcp-server shared-network-name wired-eth0 subnet 192.168.1.0/24 static-mapping arbus ip-address 192.168.1.177 
set service dhcp-server shared-network-name wired-eth0 subnet 192.168.1.0/24 static-mapping arbus mac-address 70:85:c2:2b:45:55 
```
Then commit and save.
```bash
commit
save
exit
```
```

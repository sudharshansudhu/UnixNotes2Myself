# Description: Unbound DNS Server Administration on Ubuntu

### Note
* Unbound [Tutorial](https://calomel.org/unbound_dns.html).
* Ubnound Hosts Without Domain Name [Troubleshooting](https://www.unbound.net/pipermail/unbound-users/2011-March/001733.html)

### Setup Root Hints
* Root key location: /var/lib/unbound/root.key
* Get root hints and place it on path /var/lib/unbound/root.hints.

```
# Place the downloaded file on the path /var/lib/unbound/root.hints
wget ftp://FTP.INTERNIC.NET/domain/named.cache -O /var/lib/unbound/root.hints

# Set the ownership
sudo chown root:root /var/lib/unbound/root.hints
```

### Install Unbound DNS Server
```
sudo apt-get update
sudo apt-get dist-upgrade
sudo apt-get install unbound
```

### Check Unbound Reference Conf File
* Check /usr/share/doc/unbound/examples/unbound.conf for a commented reference config file after installation.
* Alternativley, check unbound.conf.original in this directory.

### Configure Unbound DNS Server
```
### Take Backup of Original Conf file
ssh <machine-name>
cd /etc/unbound
sudo cp unbound.conf unbound.conf.YYYY-MM-DD

# Edit the configuration file unbound.conf
sudo vim /etc/unbound/unbound.conf

# Check unbound.conf in this directory and add entries

# Save the file 

# Restart the file
/etc/init.d/unbound restart
```

### Configure to Resolve Hostnames Without .local
```
# Simply edit /etc/resolv.conf, and add a domain or search directive, on each of the servers
# cat /etc/resolv.conf
search local 
nameserver 127.0.0.1

cat /etc/unbound.conf | grep "\.local"
# Output
# local-data: "testing.local IN A 127.0.0.5"

ping testing
# Output
# PING testing.local (127.0.0.5) 56(84) bytes of data.
# 64 bytes from 127.0.0.5: icmp_seq=1 ttl=64 time=0.055 ms
```

### Test Locally By Pointing Local DNS server to point to the New DNS Server Machine
- If you are using ubuntu 14.04, you may find you can not set DNS on your /etc/resolv.conf.
- This is because Ubuntu which uses resolvconf to manage the DNS settings, will regenerate resolv.conf file on every 
  system boot.

```bash
sudo vim /etc/resolvconf/resolv.conf.d/head  

# Put your nameserver list in
nameserver 8.8.8.8
nameserver 8.8.4.4 

nameserver 192.168.0.21

# Regenerate the resolve.conf file 
sudo resolvconf -u

# Check /etc/resolv.conf 
cat /etc/resolv.conf 
```

### Check the DNS Servers Used Locally
```bash
nmcli dev show enp0s25 | grep -i dns
```

### Miscellaneous
```bash
# Command to check the DNS server being used locally. 
nmcli dev show enp0s25 | grep -i dns

# Add the output IP of the above to DNS server cat /etc/resolv.conf on hawk
# Actually add it to the head
sudo vim /etc/resolv.conf

nameserver 202.83.21.12
nameserver 202.83.20.100
```

### Common Unbound Server Commands
```
# Start Unbound Server
sudo service unbound start

# Stop Unbound Server 
sudo service unbound stop

# Restart Unbound Server 
sudo service unbound restart
```

### TODO
* Add the log file details.jai
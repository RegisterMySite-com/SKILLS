Basic checks (do these first)
Plug your phone in via USB and turn on USB tethering on the phone.
Then run these in Konsole and paste the output:
# See if the USB network interface appears
```
ip link
ip a
```
# See what NetworkManager thinks
```
nmcli device
nmcli connection
```
Also test basic connectivity:
```
ping -c 3 8.8.8.8
ping -c 3 google.com
```
2. Restart networking (quick try)
sudo systemctl restart NetworkManager
Unplug the phone, wait 5 seconds, plug it back in, and re-enable USB tethering on the phone.
3. Re-apply the TTL “1 less hop” trick
This is the command that sets the hop limit so the phone doesn’t decrement it in a way the carrier can detect:
# IPv4
```
sudo sysctl -w net.ipv4.ip_default_ttl=65
```
# IPv6 (recommended too)
```
sudo sysctl -w net.ipv6.conf.all.hop_limit=65
sudo sysctl -w net.ipv6.conf.default.hop_limit=65
```
To make it permanent (survives reboot):
```
echo "net.ipv4.ip_default_ttl=65" | sudo tee /etc/sysctl.d/99-tether-ttl.conf
echo "net.ipv6.conf.all.hop_limit=65" | sudo tee -a /etc/sysctl.d/99-tether-ttl.conf
echo "net.ipv6.conf.default.hop_limit=65" | sudo tee -a /etc/sysctl.d/99-tether-ttl.conf
sudo sysctl --system
```
4. Load the RNDIS module if needed
Some phones need this kernel module:
```
sudo modprobe rndis_host
sudo modprobe usbnet
```
Quick tests
Run these in Konsole and tell me the results:
# Test DNS resolution
```
ping -c 2 google.com
nslookup google.com
```
# See what DNS servers NetworkManager is using
```
nmcli dev show enxa6cd8960e4de | grep -i dns
resolvectl status
```
Most common fix
Force good public DNS on the tether connection:
Open System Settings → Network
Click the wired/USB connection (should be “Wired connection 3” or similar)
Go to the IPv4 tab
Change Method to Automatic (DHCP) addresses only
Under DNS servers put:
1.1.1.1, 8.8.8.8
Click Apply
Disconnect and reconnect the USB tether (or just toggle it off/on on the phone)
Alternatively from the terminal:
```
nmcli connection modify "Wired connection 3" ipv4.dns "1.1.1.1 8.8.8.8"
nmcli connection modify "Wired connection 3" ipv4.ignore-auto-dns yes
nmcli connection down "Wired connection 3"
nmcli connection up "Wired connection 3"
```
Also try in Firefox
Open a new private window and test
Or go to about:preferences#general → Network Settings → make sure it is set to Use system proxy settings or No proxy
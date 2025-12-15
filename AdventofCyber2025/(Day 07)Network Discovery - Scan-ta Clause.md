- nmap: port scanning
- 22 -> default SSH port
- netcat: to manually interact with network services
- `dns` port: protocol that connects domain names to IP addresses
- 3306 port -> MySQL database management system  

## Nmap commands
- `nmap -sn <network>` : tells us which machines are live
- `nmap <ip>` : scans the specified host for open ports
- `nmap -p <port> <ip>`: scans specified port

## Transport protocols 
- TCP(Transmission Control Protocol): used in case of web browsing, email, file transfer, SSH; connection based 
- UDP(User Datagram Protocol): is connectionless

## Netcat
- `nc <ip> <port>` : connects to service 


# Banner Grabbing Tool

## Description
This is a **multi-threaded banner grabbing tool** written in Python. It attempts to connect to specified ports on a target IP address and retrieve service banners, which can reveal information about running services such as server versions and software types.

## Features
✅ Multi-threaded for **faster scanning**  
✅ Supports scanning **multiple ports simultaneously**  
✅ Lightweight and easy to use  

## How It Works
1. The user enters a **target IP address**.
2. The user provides a list of **target ports**, separated by commas.
3. The script **creates a new thread for each port**, allowing simultaneous scanning.
4. It **attempts to connect** to each port and retrieves any available **banner information**.
5. The results are printed, showing either the service banner or an error message.

## Installation
Ensure you have Python installed (Python 3 recommended). No additional libraries are required.

## Usage
Run the script and provide the necessary inputs:

```sh
python banner_grabber.py
```

Example:
```
Enter target IP address: 192.168.1.1
Enter target ports (comma-separated): 21,22,80,443

[+] Starting banner grab on: 192.168.1.1
[+] 21/tcp     vsFTPd 3.0.3
[+] 22/tcp     OpenSSH 7.9p1 Debian-10+deb10u2
[+] 80/tcp     Apache/2.4.41 (Ubuntu)
[-] 443/tcp    [Errno 104] Connection reset by peer
[+] Banner grabbing completed!
```

## Code
```python
import socket
import threading

# Take input from the user for target IP address
TARGET = input("Enter target IP address: ")

# Take input from the user for target ports
ports_input = input("Enter target ports (comma-separated): ")
PORTS = [int(port.strip()) for port in ports_input.split(",")]

print("[+] Starting banner grab on:", TARGET)

def grab_banner(port):
    try:
        sock = socket.socket()
        sock.connect((TARGET, port))
        banner = sock.recv(1024)
        print(f"[+]{port}/tcp\t\t {str(banner.decode().strip())}")
    except Exception as e:
        print(f"[-]{port}/tcp\t\t {str(e)}")

# Create a thread for each target port
threads = []
for port in PORTS:
    thread = threading.Thread(target=grab_banner, args=(port,))
    threads.append(thread)
    thread.start()

# Ensure all threads finish
for thread in threads:
    thread.join()

print("[+] Banner grabbing completed!")
```

## Security & Ethical Use
🔴 **Use this tool only on networks you have permission to scan. Unauthorized scanning may be illegal.** 🔴

## Future Improvements
- Add **timeout handling** for faster execution.
- Implement **logging to a file** for better analysis.
- Enhance **error handling** for better reliability.

🚀 Feel free to contribute or modify as needed!


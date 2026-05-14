# 2c.SIMULATING ARP /RARP PROTOCOLS
## NAME: RITIKA S
## REG NO: 212225220086
## AIM
To write a python program for simulating ARP protocols using TCP.
## ALGORITHM:
## Client:
1. Start the program
2. Using socket connection is established between client and server.
3. Get the IP address to be converted into MAC address.
4. Send this IP address to server.
5. Server returns the MAC address to client.
## Server:
1. Start the program
2. Accept the socket which is created by the client.
3. Server maintains the table in which IP and corresponding MAC addresses are
stored.
4. Read the IP address which is send by the client.
5. Map the IP address with its MAC address and return the MAC address to client.
P
## PROGRAM
```
import socket
import threading
import time

ARP_PORT  = 55000
RARP_PORT = 55001

# ---------- ARP ----------
def arp_server():
    s = socket.socket()
    s.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    s.bind(("127.0.0.1", ARP_PORT))
    s.listen(5)
    print("ARP Server waiting...")
    address = {
        "165.165.80.80": "6A:08:AA:C2",
        "165.165.79.1":  "8A:BC:E3:FA"
    }
    while True:
        c, addr = s.accept()
        ip = c.recv(1024).decode().strip()
        if ip.lower() == "exit":
            c.send("Closing ARP Server".encode())
            c.close()
            break
        c.send(address.get(ip, "Not Found").encode())
        c.close()
    s.close()

def arp_client():
    time.sleep(1)
    while True:
        ip = input("Enter Logical Address (or 'exit' to quit): ").strip()
        try:
            s = socket.socket()
            s.connect(("127.0.0.1", ARP_PORT))
            s.send(ip.encode())
            response = s.recv(1024).decode()
            print("MAC Address:", response)
            s.close()
        except Exception as e:
            print("Connection error:", e)
            break
        if ip.lower() == "exit":
            break

# ---------- RARP ----------
def rarp_server():
    s = socket.socket()
    s.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    s.bind(("127.0.0.1", RARP_PORT))
    s.listen(5)
    print("RARP Server waiting...")
    address = {
        "6A:08:AA:C2": "192.168.1.100",
        "8A:BC:E3:FA": "192.168.1.99"
    }
    while True:
        c, addr = s.accept()
        mac = c.recv(1024).decode().strip()
        if mac.lower() == "exit":
            c.send("Closing RARP Server".encode())
            c.close()
            break
        c.send(address.get(mac, "Not Found").encode())
        c.close()
    s.close()

def rarp_client():
    time.sleep(1)
    while True:
        mac = input("Enter MAC Address (or 'exit' to quit): ").strip()
        try:
            s = socket.socket()
            s.connect(("127.0.0.1", RARP_PORT))
            s.send(mac.encode())
            response = s.recv(1024).decode()
            print("Logical Address:", response)
            s.close()
        except Exception as e:
            print("Connection error:", e)
            break
        if mac.lower() == "exit":
            break

# ---------- MAIN ----------
def main():
    choice = input("Enter ARP or RARP: ").strip().upper()
    if choice == "ARP":
        t1 = threading.Thread(target=arp_server, daemon=True)
        t2 = threading.Thread(target=arp_client)
    elif choice == "RARP":
        t1 = threading.Thread(target=rarp_server, daemon=True)
        t2 = threading.Thread(target=rarp_client)
    else:
        print("Invalid Choice")
        return
    t1.start()
    t2.start()
    t2.join()

if __name__ == "__main__":
    main()
```
## OUTPUT
<img width="755" height="247" alt="Screenshot 2026-05-14 231153" src="https://github.com/user-attachments/assets/b62107db-e241-49ac-b540-37aa14ef2f33" />
<img width="670" height="222" alt="Screenshot 2026-05-14 231205" src="https://github.com/user-attachments/assets/46bd5a51-2ba3-49cb-8d03-a271864c36e9" />

## RESULT
Thus, the python program for simulating ARP protocols using TCP was successfully 
executed.

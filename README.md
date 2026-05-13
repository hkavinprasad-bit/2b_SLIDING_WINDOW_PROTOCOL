# 2b IMPLEMENTATION OF SLIDING WINDOW PROTOCOL
## AIM
## ALGORITHM:
1. Start the program.
2. Get the frame size from the user
3. To create the frame based on the user request.
4. To send frames to server from the client side.
5. If your frames reach the server it will send ACK signal to client
6. Stop the Program
## PROGRAM
```
import socket
import threading
import time

host = "localhost"
port = 8000

frames = [0, 1, 2, 3, 4, 5, 6, 7]
window_size = 4

def receiver():

    r = socket.socket()

    r.bind((host, port))

    r.listen(1)

    print("📡 Receiver Ready...\n")

    conn, addr = r.accept()

    print("🔗 Connected with", addr)

    expected = 0

    while True:

        msg = conn.recv(1024).decode()

        if msg == "END":
            print("\n🛑 Transmission Completed")
            break

        packet = eval(msg)

        print(f"\n📦 Window Received : {packet}")

        for frame in packet:

            print(f"✅ Frame {frame} Accepted")

            expected += 1

            time.sleep(0.5)

        ack = expected - 1

        conn.send(f"ACK {ack}".encode())

        print(f"📤 ACK {ack} Sent")

    conn.close()

    r.close()

def sender():

    time.sleep(2)

    s = socket.socket()

    s.connect((host, port))

    base = 0

    while base < len(frames):

        window = frames[base:base + window_size]

        print(f"\n📤 Sending Window : {window}")

        s.send(str(window).encode())

        ack = s.recv(1024).decode()

        print(f"📨 {ack} Received")

        base += window_size

        time.sleep(1)

    s.send("END".encode())

    s.close()

threading.Thread(target=receiver).start()

threading.Thread(target=sender).start()
```
## OUPUT
<img width="1918" height="963" alt="Screenshot 2026-05-13 153349" src="https://github.com/user-attachments/assets/be18393b-ab62-409e-ac08-63dc1489c9cb" />

## RESULT
Thus, python program to perform stop and wait protocol was successfully executed

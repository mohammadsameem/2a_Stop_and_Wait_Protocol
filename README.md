# 2a_Stop_and_Wait_Protocol

## AIM
To write a Python program to implement the Stop and Wait protocol using socket programming.

## ALGORITHM
1. Start the program.
2. Run the server (receiver) and connect the client (sender) to it.
3. Get the number of frames and the data for each frame from the user.
4. The client sends one frame with its sequence number (0 or 1) and waits for the ACK.
5. The server accepts the frame, toggles the expected sequence number and sends the ACK.
6. If the client receives the correct ACK, it sends the next frame; if the timer expires, it retransmits the same frame.
7. After all frames are sent, the client sends END and both sides close the connection.
8. Stop the program.

## PROGRAM

**Server (run first):**
```python
import socket

s = socket.socket()
s.bind(('localhost', 8000))
s.listen(1)
c, addr = s.accept()

expected = 0
while True:
    data = c.recv(1024).decode()
    if not data or data == "END":
        break
    seq, frame = data.split(":", 1)
    if int(seq) == expected:          
        print("Frame", seq, "received:", frame)
        expected = 1 - expected
    c.send(f"ACK{seq}".encode())     
c.close()
```

**Client:**
```python
import socket

s = socket.socket()
s.connect(('localhost', 8000))
s.settimeout(3)

seq = 0
for i in range(int(input("Number of frames: "))):
    frame = input(f"Frame {i} data: ")
    while True:
        s.send(f"{seq}:{frame}".encode())
        try:
            if s.recv(1024).decode() == f"ACK{seq}":
                print("ACK", seq, "received")
                break
        except socket.timeout:
            print("Timeout, resending")
    seq = 1 - seq
s.send(b"END")
s.close()
```

## OUTPUT
<img width="1917" height="247" alt="image" src="https://github.com/user-attachments/assets/11475827-c931-4768-8812-837db5c2eead" />


## RESULT
Thus, the Python program to implement the Stop and Wait protocol was successfully executed. Each frame was sent only after the previous one was acknowledged.

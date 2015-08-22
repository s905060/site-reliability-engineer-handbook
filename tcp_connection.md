# TCP connection

* Step1:
Machine 1 wants to initiate a connection with machine 2, So machine 1 sends a segment with SYN(Synchronize Sequence Number). This segment will inform the machine 2 that Machine 1 would like to start a communication with Machine 2 and informs machine 2 what sequence number it will start its segments with.

Note: Sequence Numbers are mainly used to keep data in order.

* Step2: 
Machine 2 will respond to Machine 1 with "Acknowledgment" (ACK) and SYN bits set. Now machine 2's ACK segment does two things; they are as below.

  1. It acknowledges machine 1's SYN segment.
  2. It informs Machine 1 what sequence number it will start its data with.

* Step 3: 
Now finally machine 1 Acknowledges Machine 2's initial sequence Number and its ACK signal. And then Machine 1 will start the actual data transffer.

**Note: Initial Sequence Numbers are randomly selected while initiating connections between two machines.**
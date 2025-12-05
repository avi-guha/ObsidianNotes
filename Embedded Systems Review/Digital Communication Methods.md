

| Protocol | Type         | Topology         | Typical Use           |
| :------- | :----------- | :--------------- | :-------------------- |
| UART     | Asynchronous | Point-Point      | GPS/Bluetooth         |
| SPI      | Synchronous  | Master-Slave     | Fast Sensors/Displays |
| I2C      | Synchrnous   | Multi-Master Bus | Sensors/ICs           |
| CAN      | Differential | Multi-Master     | Automotive,Robotics   |
## UART 
- Asynchronous 
- Communication between two devices 
- Agreed BAUD rate 
Wires: 
- TX: Transmit to output 
- RX: Receive to input

## SPI 
- Synchronous 
- Full duplex communication between master and one or more slaves 
- Extremely fast and simple - often used for high speed peripherals 
Wire 
- MOSI: Master out slave in (out) / Data from master to slave
- MISO: Master in slave out (in) Data from slave to master
- SCK: Serial Clock (synchronizes bit transfers) (out)
- SS: Slave select / chip select (Selects which slave is active)(selects which slave is active low)

## I2C 
- Two-wire synchronous, multi-master bus 
- Uses addressing - multiple devices can share the same two lines 
- Open-drain architecture (pulls low, resistors pulled high) 
- (SCL) Serial Clock: 
	- bidirectional carries clock pulses 
- SDA Serial data line 
	- Bidirection (open drain), carries data between all devices 

## CAN 
- Multi-master, differential bus designed for reliable communication in noisy environments 
- All nodes can transmit; arbitration ensures only one wins without data loss 
- Excellent for long cables and multiple ECU's

- CAN_H high line of differential pair (carries differential signal)
- CAN_L carries differential signals 
- 120 $\ohm$ prevents reflection 
# CAN Protocol : Insights and Applications :

## Theory and insights :

* CAN is a communication protocol used mostly in vehicles and automotive industry and that is mainly for its robustness, high accuracy and significant baudrate.

* CAN is a network composed of many nodes,Meaning that it works by broadcasting The message.(Look up to next Bullet) One good thing is that CAN Nodes can be disconnected without affecting other nodes and that is good for maintenance and debugging.

* CAN is known for its message type, So Nodes do not have adresses as in I2C or are selected as in SPI, However we Tend to identify messages for that each message have An ID, every Node expects then a certain Message ID and that's how informations are exchanged.

###  **Physical Layer** :
  * CAN is known for introducing the **twisted** differential pair : The CAN HIGH and CAN LOW, So our signal will not be compared by a common ground by the nodes how ever it is treated by making a difference between its two lines and that for if We have a perturbation in one Line the other Line will have the same perturbation.
* Logic 0, Logic 1 : the CAN WAY :
    So can basically have two states for the entire BUS : and those are recessive and dominant state, Recesive is marked by CANH ~= CANL, Dominant state is where there is a significant difference between the two lines.

###   Priorities : 

CAN have a unique way of handling priorities and that is coming from its physical layer.
Remember Recessive and Dominant ? that's exactly the awnser, In fact Recessive state is the "By default state" in The Bus and Nodes whenever they want to Write a 1 they let the bus get pulled to that state on its own and that way dominant takes over the higher priority.

### A single CAN BIT : 

A single CAN Bit is divided into four parts and those are sync, propagation, phase1 and phase2, each one of those Parts takes a certain amount of "**Time Quanta**", A time Quanta is the smallest time Unit in CAN ecosystem.
The exact moment when Phase 1 Ends and Phase 2 begins is **The Sampling Point** and where the Bus retrieves the information.

### Filters : 

As already said, CAN messages are broadcasted to all Nodes across the network and here a CAN Filter is used so that every nodes takes the message type it is expecting.
There are many types of FDCAN Filters.

 * CAN MASK FILTER : This Filter uses two seperate register in order to decide whether message can pass, First One is **IDMask** : It indicates what bit I do care about and what Bits I do not care About.
Next, and using **IDFilter** Register I check if the Bits **I Care About** are the same as is Incoming data Header.
Combining these Two Steps, CAN Decides whether Incoming Data is relevant ti this Node ot Not.

### Message Reception : 
CAN Introduces a concept of FIFOs, Deciding Where the message will be stored is the filter Job,
See there is a Struct member in the filter called : **FilterConfig**, this member is responsible for guiding the message in case the filter let it pass.
When Activation notification for FDCAN we can match FIFOx FOR FDCANx and so whenever we get a new message or out FIFO is Full we hit on the callback function.
**CubeMX** Configuration allows us to choose RxFIFO size and so we need to keep an Eye on our **HAL_FDCAN_ActivateNotification** second Param.

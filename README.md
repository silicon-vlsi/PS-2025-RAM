# 16 byte SRAM USING CMOS

- Designed a 16-byte SRAM (Static Random Access Memory) using the 130nm technology node, employing a full custom approach.
- This SRAM design allows for reading or writing 8-bit data at a time.
- Throughout the project, various Cadence tools were employed to aid in the design and verification processes.

## Table of Contents

- [Introduction](#Introduction)
- [Architecture](#Architecture)
- [Components](#Components)
    - [8T_SRAM](#8T_SRAM)
    - [Precharge_circuit](#Precharge_circuit)
    - [Row_decoder](#Row_decoder)
    - [RAM/ICM logic block](#RAM/ICM_logic_block)
    - [Sense_Amplifier](#Sense_Amplifier)
    - [Write_driver](#Write_driver)
 
- [Design and testbench](#Design_and_testbench)
- [Layout](#Layout)
- [Conclusion](#Conclusion)
 ## Introduction
  ### SRAM :  

Static Random Access Memory, commonly known as SRAM, is a fundamental type of semiconductor memory used extensively in modern digital electronic systems. Unlike dynamic RAM (DRAM), which requires periodic refreshing, SRAM is static in nature, meaning it holds data as long as power is supplied.

Applications of SRAM include serving as cache memory in microprocessors, providing high-speed storage for critical data and instructions, and acting as the primary memory in various embedded systems where fast and reliable access to data is essential. SRAM is favored for its fast read and write access times, making it a crucial component in optimizing the performance of various electronic devices.
## Architecture
- In this section we will see the architecture integrates a 16×8 SRAM array, a dual-mode RAM/ICM logic controller, and key peripheral circuits—decoder, sense amplifier, precharge, and write driver— offering a compact, energy-efficient solution for future embedded memory systems. Then we will discuss about the overall operation of this system which will give the rough idea about the working of various components simultaneously and how the data is being stored and then how we read that from SRAM cell.

<img src="https://github.com/manas60/PS_project/blob/c55684821099f65b76b476f67eef0244b86f7ff5/images/8Tarch.png" alt="Architecture_SRAM" title="Figure 1" height="500" width="4000">
<p align="center"> Figure 1: Architecture_SRAM</p>

- Figure 1 respresents the the architecture of the 16 byte SRAM along with some supporting elements like precharge circuit, write driver etc.
- We can read or write 8 byte data in the memory element and the total amount of memeory avaliable is 16 byte.
- The design of 16 byte SRAM is done by arranging the **6T SRAM cells** as 16 row and in ach row there will be 8 cells
   for each row there will a word line (WL) and for each colum there will a BL and BLB so in total there will be 16 WL and 8 BL and BLB.
- 6T SRAM cells are designed using a back to back inverter and two access transistors, the design part will be discussed 
  #### Operation

  - All the signals that are shown in the figure 1 are given parallely.
  
  - First PC will be given zero that will activate the precharge circuit the precharge circuit will basically chagrge the capacitors Cpar to vdd then if we want to write then PC will be high and ctrl signal will be turned ON 
 and rwn will be given as 0, by doing this the write driver will be actiavted and after that it will drive the data from input to the BL and BLB node. The adress data will be taken by the row decoder 
 and row decoder will select in which row to write.  Suppose the adress is 0000 then it will select the 0th  row for writing then when the control signal will be turned ON it will activate the WL signal 
 then the data will be written to SRAM and all these things will occur when the precharge signal is at high.A point to note is before each read or write there will be a precharge and when precharge is done after that only read or write operation can   be started. Now becuase of back to back inverters it will hold the data till the time the next data is not written in the same location.

   - Then if we want to read again we have to precharge and the BL and BLB node to vdd that means Cpar is fully charged. Then when pc = 0 and WL = 1 at that time the the data stored in the sram is 1 then the BLB
  node will come down and if the data stored in sram is 0 then the BL node will come down i.e the Cpar in the BL side wll discharge.Now the sense Amplifier two inputs are also connected to the BL and BLB line
  when any one node will go down it will sense the voltage difference between BL and BLB node and at the sense amplifier output we will get the data of sram that we have selected by giving its adress.

## Components

In this section, Various components of projects are explained in detail and realted equations, simulation results are mentioned.  

### 6T_SRAM
<div align = "center">
<figure>
    <img src="DOCS/6 tSRAM.png" alt="6T SRAM" title="Figure 4" height="500" width="700" align="center">
</figure>
<p></p>
<p>&nbsp;Figure 4: 6T SRAM</p>
</div>
 The above fingure i.e figure 4 shows the classic structure of a 6T sram which can store one bit data.
- These are basically two back to back inverter with access transistor i.e M3 and M4.
- Since there are two back to back inverter structure is there till the time the vdd and ground supply is there for inverter the data will not change.

#### Operation :
- There are basically 3 modes of opeartion,
     - read operation
     - write operation
     - hold operation
- In read and write operation WL (Word line) = 1 and in hold operation WL = 0.
- Word line will be controlled by a signal called control and row decoder output, which will be discussed later.
- read write operation will be controlled by  signal rwn and  control as shown in figure 1 i.e SRAM architecture.

##### Read operation :
- In read operation first the bit line (BL) and bit line bar (BLB) node will be charged to vdd.
- Then PC=1 i.e Precharge will be turned OFF, in figure 4 suppose node N1 is at vdd and N2 is at zero i.e we can say that we have stored a logic 1 in sram cell previously.
- When we will make WL = 1 then, 
  BL node is at vdd and N1 is at also at vdd then there will be no change in BL where as BLB node is at vdd but N2 is at logic 0 and M2 is also ON as N1 is input for M2 because of back to back structure
  hence BLB node will try to discharge i.e the Parasitic Cap at BLB node Cpar will discharge through that path of M2 and M4.
- In this way BL node is stable and BLB node is going down which is a indication that we are reading logic 1 similarly while reading logic 0 BLB node will be stable at logic 1 where as BL node will be  dischagred gradually.
- During read operation there is a problem that we can face that is :
   - While reading logic 0 BL node will be discharged and it will charge N1 node and if N1 node will be charged to same or more than threshold voltage then the NMOS on the other side 
  i.e M2 will be ON it may toggle the data stored even if it will not toggle there will be a unneccesary current flow since NMOS is on and that is a power loss.
   - Thats why we try to keep the node N1 voltage around 0.3 V i.e less than the threashold now how can we do that for that we have to modify the sizing of two NMOS i.e
  M1 and M3 and because SRAM is a symmetric structure M2 and M4 will also have the same size.
##### Write operation :

- In write operation, first we will precharge both the nodes to vdd .
- Suppose node N1 is at logic 0 and N2 is at logic 1, now we want to say write logic 1 to node N1.
- After precharge, we will make PC = 1 and then when Ctrl = 1 and rwn = 0 at that time the write driver will be connected to the BL and BLB line.
- Data line will be connected to BL where as Data bar line will be connected to BLB.
- As we want to write one the BL line will at logic 1 and the BLB line will be at logic 0.
- Then WL=1, so that the data in BL and BLB line can be stored in the internal node of the sram i.e N1 and N2 in this case.
  ### Precharge_circuit
<div align="center">
<!--<figcaption>Figure 2: Precharge Circuit</figcaption>-->
<img src="./precharge ckt.png" alt="Precharge circuit" title="Figure 2" height="400" width="700">
<p align="center">Figure 2: Precharge Circuit</p>
</div>

- Preharge circuit is basically used to charge the BL and BLB node to vdd before write and read operation.
- When PC = 0, at that time the PMOS will be ON and it will charge the BL and BLB to vdd.
- The Meq i.e shown in figure 2 is basically the equallizer transistor whose purpose is to equallize the BL and BLB line during precharge period.
- As discussed earlier, we have a large parasistic capacitance on BL node i.e Cpar. Now PMOS in the Precharge has to be that large so that it can charge the capacitance in less time.
- Why do we do pre-charging or pre-discharging?
- Before reading or writing from/to the SRAM, we need to make the BL and BLB of same voltage, we can do it by either ways, by pre-charging them to 1.8V or pre-discharging them to 0V. And the difference between BL and BLB, due to falling or rising of the BL and BLB, can be sensed and output can be obtained.

- why did we go for pre-charge and not pre-discharge?
- If we are going for pre-discharge then the area of the transistors increases by 11.9%. So, minimising the area and having good operative gain we went for the pre-charge.

- Can we go for pre-charge and pmos as the access transistors?
- It's technically possible to use PMOS transistors for access during pre-charge, it would be unconventional and may introduce unnecessary complexity and potential performance drawbacks. NMOS transistors are used for active pull-down operations in SRAM cells because they can discharge the storage nodes quickly when needed. And using PMOS can discharge but taking up longer time than NMOS making the device slower

  


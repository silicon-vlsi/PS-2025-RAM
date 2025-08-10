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
- 6T SRAM cells are designed using a back to back inverter and two access transistors, the design part will be discussed l

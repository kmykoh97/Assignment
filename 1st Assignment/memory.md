# Memory Technical Report

## Brief Introduction on Memory

> Memory, one of the most important part in the computer, is a bridge to communicate with CPU. All the programs run in the memory, which makes the performance of memory affect heavily on computer. Memory is also called inner memory, whose is used to store CPU data and data that is to be changed with outer storage like disk temporarily. The memory keeps the computer work steadily and it can read and store data fast.

## Some of the Memory Technologies
---
### ECC  
 >Error Detection Code, is also called ECC. The extra bits on the data bits store a code encrypted with data, called ECC code. ECC codes encode information in 8 bits, which can recover 1 bit error. Each time the data is written to memory, ECC code uses a special algorithm to calculate the data. The result is called check bits. Then the sum of all the CHECKBITS is checksum, and the checksum is stored with the data. When these data are read out from memory, the same algorithm is used to calculate the checksum again, and compared with the previous calculation results, if the results are the same, the data is correct, and vice versa, the ECC code will be decoded to determine which one of the data is incorrect. Then this error bit will be corrected without affecting the system operation.  
  
Pros:Simultaneous detect and correct single bit errors  
Cons:Can't do anything about two or more bit errors;Errors may occur in memory locations that have not yet been accessed. If these errors accumulate, they can lead to irreparable multi-bit errors, which can lead to data corruption and even system crash.
 
---
### ChipKill
 >The storage protection provided by the Chipkill Memory Controller is conceptually similar to that provided by disk arrays with verification functions: when writing data, it writes data to multiple DIMM memory chips. In this way, each DIMM plays the same role as the storage array. If any of these chips fails, it only affects one bit of a data byte, because the other bits are stored on another chip. After an error occurs, the memory controller can reconstruct the "lost" data from the failed chip so that the server can continue to work properly.  

Pros:Can detect and correct two or more bits errors  

---
### Memory ProteXion / RBS
> Memory ProteXion (memory protection, also known as Redundant Bit Migration (RBS) is a memory error correction technology of IBM, which was originally developed for IBM mainframe. It works in a way similar to the hot standby disk sector in Windows NTFS file system. If the operating system detects that there are bad sectors on the disk, it will write data to the standby sector to achieve this purpose.

Pros:It is more protective than the Chipkill memory technology described earlier. 

---
### SDDC 和DDDC
> Intel provides a similar function to ChipKill, called Single Device Data Correction (Intel x4/x8 SDDC). It can correct the multi-bit error on a single DRAM device, delete a single DRAM from the memory map, and restore the DRAM data to the new device, so as to quickly repair the failure of the whole DRAM device. After the error DRAM is deleted from the memory map, the single bit data correction can be continued.

> DDDC is a dual device data correction. Allows correction of dual-bit hardware memory errors. Make sure that when two DRAM chips on a DIMM have multi-bit errors, they can also repair the errors, delete the erroneous DRAM from the memory map, and restore the repaired data to the new device. After the error DRAM is deleted from the memory map, the single bit data correction can be continued.

---
### Memory Scrubbing
> Memory Scrubbing is Intel's memory checking technology. It introduces an embedded hardware engine to continuously monitor and repair memory errors, so as to ensure that errors do not accumulate to an irreparable extent.

---
### Memory Mirroring 
>Memory Mirroring uses two DIMMs to mirror each other to form a backup mechanism. Its working principle is similar to RAID1 hard disk. Memory Mirroring makes two copies of memory data, which are stored in main memory and mirror memory respectively.

>Normally, memory data is read only from active memory cards. Only when active memory fails, memory protection and Chipkill repair technologies can not be completely repaired, will data be read from mirror memory. If there is a malfunction in memory that can cause the system to alarm, the system will report to the system administrator, and the server will automatically switch to using the mirror memory card until the malfunctioning memory is replaced. Mirror memory allows for Hot swap and Hot-add memory.

---
### LockStep
> Access to memory channels can be divided into Independent Channel Mode and LockStep Channel Mode. Independent channel mode means that each channel runs independently, and one memory channel corresponds to a cache-line of the CPU. LockStep technology uses the same, redundant hardware components to process the same instructions at the same time, so that data on a CPU cache line can be distributed to several memory channels. LockStep technology can keep precise synchronization of multiple CPUs and memory, and execute the same instructions in the correct same clock cycle. This technology guarantees that any errors can be found, and even if the errors are short, the system can resume normal operation without losing data. LockStep is also called lock step, or advanced ECC.

## Key Indicators

  Here we talk about some indicators of server memory.
- price   
  RDIMM is moderate. Small-capacity UDIMM is cheaper, but because of the high manufacturing process requirements, the price of large-capacity UDIMM is even higher than RDIMM. The price of LRDIMM is higher than that of RDIMM and UDIMM.

- capacity  
  Only LRDIMM can support maximum total memory capacity. However, the memory capacity required by major applications can generally be achieved by deploying RDIMM.

- frequency  
  At present, only RDIMM supports 1.6 GHz, which can have larger bandwidth and smaller delay.

- delay  
  Although UDIMM has the smallest latency at the same frequency, RDIMM can support 1.6 GHz and use higher frequency to reduce the latency.

- consumption  
  LRDIMM was the highest, while RDIMM and UDIMM were lower.



The following are the maximum amount of memory, operating voltage and frequency table that each channel can support when the server uses different types of memory:

![TIM图片20191009171925.png](https://i.loli.net/2019/10/09/y8nliOS4PsbpzrM.png)

## Comment
 If the maximum memory capacity is required, LRDIMM must be used; if the maximum memory capacity is not required, RDIMM (1.6GHz) has better performance, price and scalability; if power consumption is considered, low-voltage DDR3L memory can be used and controlled to run at lower frequencies.
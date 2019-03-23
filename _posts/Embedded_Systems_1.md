# 1|computer oganization



## three abstraction in computer:

Data, Instruction, Sequential execution model

## computer components:

I/O,Memory,CPU

## interaction between the components and abstractions:

Data and instruction are stored in memory
CPU executes instruction in sequential order
Instruction can read and write memory
Instruction can perform arithmetic operations 

## Microcomputer System

# 2|CPU/Microprocessor

## Key concept

- Core->ALU, CU, register

- Clock

- ISA/Instruction architecture set

## Hardware:CPU(1)-ALU

- Arithmetic Logic Unit (ALU)

- Two inputs
- Calculation result can be temporarily stored in one of the **_registers_** 

## Hardware:CPU(2)-CU

- Control Unit works under **instructions** 

- CU contains an instructor decoder 

- CU contains a program counter 

  

## Hardware:CPU(3)_Instruction Set

- The instruction set

  All recognizable instructions by the instruction decoder

- CISC (Complex Instruction set conponents)

  eg.x86

- RISC (Reduced instruction set components)

  eg.ARM, PowerPC

# 3|Memory (Hardware)



## Memory concept:

- Bit (b): a binary digit that can have the value o or 1

- #### Byte (B): consists of 8 bits, contains $2^8$ codes 

- Nibble: is half a Byte (4bits)

- Word: the number of bits that a CPU can process at one time

  (in 8086, 1 word= 16 bit)

  depends on the width of the CPU’s registers and that of the data bus

- Double word

  

# 4|Bus (Hardware)

- #### Concept: A bus is a communication pathway connecting two or more devices

- System bus: connects major computer components (processor, memory, I/O)

  

## Structure

- #### Simple-bus Structure

- #### CPU-Central Dual-Bus Structure

- #### Memory_Central Dual-Bus Structure

  

## BUS-Data Bus

- #### Used to provide a path for moving data between system modules

## BUS-Address Bus

- #### Used to designate the source or destination of the data on the data bus that the processor intends to communicate with

## BUS-Control Bus

- #### Used to control each module and the use of data and address buses

  

# 5|I/O

## - classification:

- #### Memory-mapped I/O

- #### Isolated I/O





# 6|introduce to embedded systems

- #### Microcontrollers(MCS)

  A microcontroller has a CPU in addition to a fixed amount of RAM, ROM, I/O ports on one single chip

- #### Embedded Systems

  #### An embedded system uses a microcontroller or a microprocessor to do one task and one task only

  (Example: toys, TV remote, keyless entry, etc. )

  #### Using microcontrollers is cheap but sometimes inadequate for the task

  #### Microcontrollers differ in terms of their RAM,ROM, I/O sizes and type.

  #### - ROM (often used as program memory, like BIOS)

  OTP (One Time-Programmable) 
  UV-ROM, EEPROM
  Flash memory

  #### - RAM (can be used as both program mem and data mem)

  SRAM(static RAM):cache
  DRAM(Dynamic RAM): main memory
  SDRAM (Synchrous DRAM)
  DDR DRAM (Double Data Rate DRAM)
  DDRII
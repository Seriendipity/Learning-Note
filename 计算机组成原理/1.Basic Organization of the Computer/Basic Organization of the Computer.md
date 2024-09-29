# 一、Introduction

> * **Introduction:** for the final examination is in English and should use English to write answer, I am going to use English to record my learning experience

Now let us begin the study of the first Character of the Component of Computer

## 1.1 Hardware and Software

A computer system consists of **Hardware** and **Software**.

### 1.1.1 Hardware

> As for the **Hardware** , it is the physical part of the computer. It consists of many kinds of electronic components, such as **Motherboard** , **External Equipment** etc..

***motherboard (also called the mainboard )*** : It is the main printed circuit board. And for the laptop computers, **CPU**、**GPU** and even **memory** are integrated on the motherboard.

> > ***CPU*** : It is called Central Processing Unit , also called Central Processor or main processor. It is the electronic circuitry within a computer that carries out the **instructions** of a computer program by performing the basic **arithmetic, logic, controlling and input/output(I/O)**

> > ***GPU*** : It is called Graphics Processing Unit. It is a specialized electronic circuit designed to rapidly manipulate and alter memory to **accelerate** the creation of images in a frame buffer intended for output to a display device.

> >> Then Why GPU can accelerate the processing of the images?

> > > To answer this question, we can study their structures. For CPU , it consists of a Control Unit, four ALU(Arithmetic Logic Unit), a cache , a DRAM. But for GPU , it has **Many ALU** than CPU. As ALU is a component for calculating , the GPU can accelerate the process.

> > ***Memory*** : In computer , **main memory** refers to the computer hardware integrated circuits that store information for immediate use in a computer, it is synonymous with the term "primary memory".**external Storage** refers to storage other than computer memory and CPU/GPU cache. Common external Storage include hard disks、CD、USB flash drives.

### 1.1.2 Software

> As for the software , it is a collection of data and instructions that tell the computer how to work. Generally speaking, it includes **System Software and Application Software** 

For the ***System Software***, It includes **Operating System、Language Processing、Database Management System、Procedures for various services.**

For the ***Application Software***, It includes **Scientific Computing Class 、Engineering Design Class 、Information Management Class**.

## 1.2 Computer System Hierarchy

Now the computer System is generally using **The Five levels of hierarchy** .Begin talking about this , let us look the history of the Computer System Hierarchy.

At the beginning , there is only one level. In a word, the programmer code **through binary**. It looks very difficult and very easy to get wrong, because the programmer must be familiar with their own computer .Besides , It has poor portability for it is  difficult to execute in another computer , But the computer can directly execute the code without compiling it.

**Code by Binary and  Execute directly**



After that period , there are two levels. A new language appear, **Assembly language** , which allows programmers to use signals to represent the address of instructions and data stores in memory unit. In a word , programmers do not need to code with binary. They directly code through assembly language. Then there is a virtual machine to translate the assembly language into machine language , then the computer directly execute it.

**Code by assembly language and translate it into machine language(binary) ---> execute directly**



For people still think it is not suitable for programmers to code , **high level languages** (eg: FORTRAN、BASIC、Pascal、C、etc) then appear. Because the high level languages is easy to understand and following the habits of people , It is very convenient. The programmer just need to know the syntax and semantics of high level languages , it is portable and easy. Then came the three levels. They directly code through high level language and then a virtual machine translate it into assembly language , then another virtual machine translate it into machine language. After that the computer can execute the program.

As the virtual machine how to translate the high level language into the assembly language , it is a software that called a translator. However according to the language , there are two kinds of methods called  ***Compiler***  and  ***Interpreter***.

> **Compiler** : In a word , it translates the source program into machine language **<u>at one time</u>** and then executes it.(C、C++)
>
> **Interpreter:** In a word , it translates **<u>one statement of the source program</u> **into one statement of the corresponding machine language and **<u>executes it immediately</u>**.(Python、Matlab)

**Code by high level language  through Compile translate it into assembly language ----> translate it into machine language ---> execute it directly**



Then with the development of the software , four levels appear. Compared with the three levels , the difference is it interprets machine instructions with microinstructions. Then the computer execute the microinstructions by hardwares.

 **Code by high level language  through Compile translate it into assembly language ----> translate it into machine language --- > Interpret it with microinstructions ----> the hardware execute the microinstructions directly**



After that ,  then came the five levels of hierarchy. Compared to the Four levels of hierarchy , it use operating system to explain the machine language.

 **Code by high level language  through Compile translate it into assembly language ----> translate it into machine language --- > Explain the operating system in machine language ---> Interpret it with microinstructions ----> the hardware execute the microinstructions directly**

## 1.3 Computer Performance Indicator

the following methods are used to measure the performance of the hardware.

### 1.3.1 Machine Word-Length

It represents the bits that the CPU can process data at one time , usually related to the number of the register bits in the CPU. At present , the machine word-length is usually 64bits or 32bits.

### 1.3.2 Memory Capacity

It includes the **Main Memory Capacity** and the **External Memory Capacity**.

Main Memory Capacity represents the binary codes stored in Main Memory , Main Memory Capacity = Storage Unit Numbers * Storage Word-Length.

 Usually the MAR (Memory Address Register) reflects the number of the Storage Unit , the MDR (Memory Data Register) reflects the Storage Word-Length. For example the MAR is 16 and the MDR is 32. That is to say , it has 2^16 storage units and every storage units can store 32 bits . So the Memory Capacity is 2^16 * 32.

8 bits (Minimum Unit) = 1 Byte (Basic Unit)

1024 Byte = 1 KB

1024 KB = 1 MB

1024 M = 1 GB

1024 GB = 1 TB

1024 TB = 1 ZB

### 1.3.3 Computing Speed

MIPS (Million Instructions Per Second) to measure the Computing Speed

CPI (Cycle Per Instruction) is to execute one instruction and to see how   much time it spends.

FLOPS (Floating Point Operation Per Second)

## 1.4 von Neumann Computer

### 1.4.1 The Characteristics of von Neumann Computer

> The computer consists of **Five** main components : **Operator, Memory , Input , Output, Controller**

* Instructions and data are stored in memory in the equal status, and can be accessed by address.

* Instructions and data are represented by binary
* The instructions consists of **an opcode** and **an address code**.
* Instructions are stored sequentially in memory, and usually operated in sequential order.
* Centered on Operator(ALU)

### 1.4.2 The diagram of von Neumann Computer

![](C:\Users\MHB\Desktop\von Neumann diagram.png)

This first kind of computer is centered on ALU. 

***The Operator*** is used to finish arithmetic calculation and logic calculation , and the intermediate result of the operation is temporarily stored in it.

***The Memory*** is used to stored data and program.

***The Controller*** is used to control and command the input、operate、the processing calculating result of the program and data.

***The Input and Output*** is used to transform the machine calculating result into our familiar information.

Another kind of computer is centered on Memory , for the data and instructions are stored in it and centered on it can decrease the accessing time.

### 1.4.3 Modern computer hardware diagram

![](C:\Users\MHB\Desktop\_     Modern computer hardware diagram.png)

The five main components of computer work orderly under the instructions of controller. 

For the Operator and the Controller is related closed and always integrated in the same chip, So we always call them together as **CPU** (Central Processing Unit).Call the Input Device and the Output Device as I/O Device. Thus we can conclude that the modern computer consists of three parts: CPU、I/O Device and Main Memory. Besides, the CPU and the Main Memory together are called MainFrame. So , we can see the diagram.

The **ALU** (Arithmetic Logic Unit) is used to finish Arithmetic Logic operations.

The **CU** (Control Unit) is used to send out operation command to execute command.

Thus the modern computer components diagram is as followed:

![](C:\Users\MHB\Desktop\modern computer components diagram.png)

## 1.5 Computer Working Process

Generally, there are three periods:

* Building a mathematical model
* Determining the calculation method
* Programming problem solving procedure

Next , we are going to focus on the third period to see how the computer works.

First, assuming that an instruction format example(the machine instruction word-length is sixteen bits) is: opcode with six bits and address code with ten bits.

| Opcode(6 bits) | Address code(10 bits) |
| :------------: | :-------------------: |

if the opcode has the following meanings:

| Opcode | operation |                           Content                            |
| :----: | :-------: | :----------------------------------------------------------: |
| 000001 |   Fetch   | Fetch the operand which is represented by the instruction address code and stored in the storage Unit to the Operator accumulator(ACC) |
| 000010 |   Store   | Store the number in ACC in the Storage Unit according to  the Instruction address code |
| 000011 |    Add    | Add the number in ACC and the number in the Storage Unit that is represented by the instruction address code together. Then store the result in ACC |
| 000100 | Multiply  | Multiply the number in ACC and the number in the Storage Unit that is represented by the instruction address code together. Then store the result in ACC |
| 000101 |   Print   | Print the operand in the Storage Unit which is represented by the instruction address code |
| 000110 |   Stop    |                                                              |

Assuming that we are going to calculate 
$$
ax^2 + bx + c
$$
![](C:\Users\MHB\Desktop\Instruction example.png)

Now let us detail the working process.

> Operator components

**X** : Operand Register

**ACC**: Accumulator

**MQ**: Multiplier-Quotient Register

|      |      +      |          -          |                *                |    /     |
| :--: | :---------: | :-----------------: | :-----------------------------: | :------: |
| ACC  | Augend、Sum | Minuend、Difference |        Product high bits        | Dividend |
|  MQ  |             |                     | Multiplicator、Product low bits | Quotient |
|  X   |   Addend    |     Subtrahend      |          Multiplicand           | Divisor  |

> Controller components

**PC**: Program Counter. Store the<u> current instructions address to be executed</u> , which counting function (PC)+1 ---> PC

**IR**(Instruction Register): Holds the <u>current instruction to execute</u>

**CU**: Control Unit

In order to complete an instruction , there are two periods : ***Fetch*** and  ***Execute***

for **Fetch** : including PC(Fetch instruction) 、IR（Analysis instruction）

for **Execute** : CU(Executing instruction)

---

Then take an example of fetch instruction:

![](C:\Users\MHB\Desktop\Detail fetch Instruction.png)

First PC store the current instructions address to be executed, so the CU send the content to the MAR(PC-->MAR) and command the Memory to read.

Then the MAR find the content in the Memory Bank(MAR--->M) , and get the instruction 0000010000001000 , and use MDR to store this instruction(M ----> MDR)

Then the MDR send this instruction to IR and let it analysis the instruction(MDR ---> IR --- >CU(IR)).

After analysis , the computer knows it is a fetch instruction (opcode is 000001) , then IR send the address code to Memory and let it return the number in the address code(0000001000)(IR---->MAR) 

Then the MAR find the number in Memory Bank according to the address code , and return the number to the MDR(MAR---->M---->MDR)

After get the number , MDR send it to the ACC in Operator.

When finish above operations, the first Fetch instruction then finished. At the same time, PC update itself(PC + 1 ---> PC) . And execute the next instruction.  

----

A Very Important Conclusion !!!

> [!IMPORTANT]
>
> From the above analysis , we can conclude that **although both instructions and the data in the von Neumann computer are stored in binary form in memory, the CPU can distinguish them on the basis that the different stages of the instruction cycle.**


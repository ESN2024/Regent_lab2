# Regent_lab2
# Lab 2 : “Let’s count stuff”
## Objective 
The objective is to implement a 3-digit counter with 7-segment display, accompanied by a timer using an interrupt with a period of 1 second.

## Introduction 
This report describes the implementation of a counter progressing from 0 to 999, using BCD to C conversion, a VHDL file for binary representation on a 7-segment display, and a timer programmed to increment the counter every second using an interrupt.

## Creating a basic NIOS II system 
Platform Designer :
![image](https://github.com/ESN2024/Regent_lab2/assets/131348400/432aa07c-2f64-4f6d-8b88-5c7cb8f91a1b)

After creating a new project on Quartus, open the Platform Designer to integrate all the required components.
Components to be added include :
- RAM0: A RAM with a storage capacity of 40 MB.
- SEG: Represents the Qsys output of 12-bit counter (3*4-bit counter). Here I have the advantage to deal with only one PIO for the displays.
- Timer_0 : Set the timer period to 1 second.
We link the timer Interrupt Sender to the NIOS2 in order to pace the counter.
Quartus 

The system consists of three distinct parts:
- lab2_sys.qsys
- bin_to_7seg.vhd: This VHDL file takes a 4-bit (counter) signal as input and generates a 7-bit correspondence for counter segment activation as output without the point. Note that the 7 segments react to logic level "0", lighting up when this level is detected.

![image](https://github.com/ESN2024/Regent_lab2/assets/131348400/7f73d774-053a-4c42-a229-1ac8f558ee86)

- regent_lab2.vhd: This top-level file defines inputs and outputs, with a 21-bit output corresponding to the segments of the 3 counters.

![image](https://github.com/ESN2024/Regent_lab2/assets/131348400/7770fc1b-2374-4d40-91df-8cec02a1e94d)

## NIOS II software flow 
Open a NIOS II terminal and generate the BSP according to the DE-10 board specifications: ![image](https://github.com/ESN2024/Regent_lab2/assets/131348400/16b6257c-9d4d-4020-9876-23bd3e1925c5)

Generate the project Makefile : ![image](https://github.com/ESN2024/Regent_lab2/assets/131348400/79a71e3d-132d-4239-a6ae-7844b26252f0)

All set, we're now going to write our C code to run on our own processor. Don't forget to update the Makefile with the source code. Compilation is performed on a Nios II shell.

The generated program can be loaded and executed in the FPGA softcore: ![image](https://github.com/ESN2024/Regent_lab2/assets/131348400/a5969fbf-d143-4502-a333-315edc484c3e)

## C language 
The C code is structured in three distinct sections:
- conv_bcd(): It convert the value in order to give the hundreds, tens and units which are shifted to be displayed on the various displays.
- irqhandler_timer(): This routine manages the timer interrupt. Each time the routine is triggered, the interrupt flag is reset. It generates a binary-to-BCD conversion based on a 16-bit counter (val), splitting the result into three 8-bit variables representing the unit, ten and hundred of the counter. This routine is triggered at every interrupt, occurring every second.
- main(): The main program is designed to register timer interrupts. The program then enters an infinite loop.

![image](https://github.com/ESN2024/Regent_lab2/assets/131348400/7b52e6cd-2cd9-4426-b875-018170de8fd1)

## Results 
Everything works as attempted !

## Conclusion
This second project carried out on our DE10 lite was slightly more complicated than the first, where the difficulty lay in the use of interrupts. Here, the difficulty lay in creating the VHDL blocks and their links under the Top Level. In fact, VHDL is a rather unusual language in its writing, and we had to delve deep into it. The second difficulty was inserting a timer, implementing it and using it in our source code to increment the counter as required.


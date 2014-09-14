Lab1_Cabusora
=============

A Simple Yet Awesome Calculator
C2C Kevin Cabusora, Dr. York, ECE 382
=============
##Objectives
The objective for this lab was to create a calculator using assembly language.  The basic principle was to store three bytes:  the first and third would be the operands, and the middle byte is the operation.  The goal was to create a calculator able to execute an operation by reading the operation.

#Prelab

##Preliminary Design/Schematic

The first consideration was to the broad overview of how to perform this program.  In general, the program will read an input (first byte), then an operation code (second byte).

The operation codes listed are as follows:
* 0x11 = ADD_OP: takes two inputs, adds them, stores results to 0x200.
* 0x22 = SUB_OP: takes two inputs, subtracts them, stores results to 0x200.
* 0x44 = CLR_OP: clears result by storing 00 to memory, stores second operand as initial value for next operation.
* 0x55 = END_OP: terminates program operations.

Compare functions will be used to determine if the code in the second byte corresponds to any of the above operation codes.

First, the program will decide if the operation listed compares to the operation code for the clear function.  (For clear, it doesn't matter what the second input's value is.)

Next, the second input will be read, and the operation code will be compared to see if matches the corresponding codes for addition or subtraction.  If it is either of these values, it will store them in 0x200, then reset and read the next set of values.

If END_OP is accessed, the program will enter an infinite loop at the end, effectively ending the program.

Below is a minimal schematic of the most basic processes required for this program.

![Prelab Schematic](https://github.com/KevinCabusora/Lab1_Cabusora/blob/master/Prelab%20Schematic.PNG?raw=true "Image")

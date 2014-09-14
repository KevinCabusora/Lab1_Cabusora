Lab1_Cabusora
=============

A Simple Yet Awesome Calculator

C2C Kevin Cabusora, Dr. York, ECE 382

=============
##Objectives
The objective for this lab was to create a calculator using assembly language.  The basic principle was to store three bytes:  the first and third would be the operands, and the middle byte is the operation.  The goal was to create a calculator able to execute an operation by reading the operation.

#Prelab

###Preliminary Design/Schematic

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

#Functionality

## Coding for Basic Functionality

[Full Assembly Language Code](Lab1_Basic_Functionality)

The first step was to figure out how to store the tested input, then to allow them to be read by the program, executed, and to progressed through.

To execute this, I used the .byte directive to store them, and named the set of inputs myProgram.

Below is the test case.

  .text
  myProgram:	.byte	0x11, 0x11, 0x11, 0x11, 0x11, 0x11, 0x11, 0x11, 0xDD, 0x44, 0x08, 0x22, 0x09, 0x44, 0xFF, 0x22, 0xFD, 0x55
  
Next, under the label List_Of_Operations, I entered in the numeric values of the operations' codes as constants.  Then, I moved the myProgram into r5.  I stored #0x200 into r4 as where the outputs would be stored in RAM.  Restart was added to the beginning of the process, as a jump later in the program would then bring the calculator to its previous state.  

Effectively, the process in restart treats r5 as the accumulator.  It points to r6, which is set as the register which will hold the first input.  In OperationSelect, the pointer points at r7 which contains the operation.  It then points to r8, which is the second output.  I saved entering in the operations for later.  I added an infinite loop at the end to effectively end my program.

I then entered in the operations.  After the pointer points at r8, I would compare the r7 with the operation codes.  If any of them were equal, they would jump to their respective operations.  For CLR_OP, I put it right after r7 is pointed to, as CLR_OP does not require a second input.

## B-Grade Functionality

For this process, the test case was:

  0x11, 0x11, 0x11, 0x11, 0x11, 0x11, 0x11, 0x11, 0xDD, 0x44, 0x08, 0x22, 0x09, 0x44, 0xFF, 0x22, 0xFD, 0x55.

To create this, I added this to the list:
  SET_MIN:		.equ	0x00
  SET_MAX:		.equ	0xFF

This created a minimum value of 0, and a maximum value of 255 in decimal.

Next, in the addition and subtraction operations, I compared r6, which held the result of the operation, to their respective constants.  If the result exceeded 255, it jumped to the operation labeled "ceiling", which set the result to 255 by default, and jumped to "floor" to be set to 0 if the answer was negative.

# Debugging

During testing, I came upon problems with the CLR_OP function.  The result of the operation would be 0, but the program would fail to move on to read the next byte in myProgram.

I then realized I had failed to add an inc function to r4.  Adding this solved my problem.

For testing for B-functionality, the program also experienced a similar problem, except it had no idea what to do after setting the result to either 0 or 255.

I realized that in the ADD_OP and SUB_OP processes, there had been a jump at the end which led back to OperationSelect.  This was missing in the ceiling and floor functions.  Adding this fixed it.

# Testing

On 12 September 2014, at 1430, Captain Virginia Trimble checked the program for basic and B-functionality.  It worked successfully using the test codes.

# Documentation
* On 9 September 2014, C2C Austin Bollinger explained how to use .equ to simplify my process and not have out-of-nowhere numbers.  After he demonstrated it with ADD_OP and SUB_OP, I then implemented this for use with the clear, end, ceiling and floor functions as well.
* On 11 September 2014, C2C Hunter Her helped me through an issue I was having with the OperationSelect process being unable to read the Operation codes.  He pointed out that I had used an ampersand instead of the correct hashtag to have the cmp read them as values to be compared. 



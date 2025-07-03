

The ALU is the first module that I've designed for this computer, an ALU is the most crucial part of a computer which performs all the mathematical operations going on in a program. The ALU for SAP-1 is a simple circuit which can perform addition and subtraction of two 8-bit binary numbers and looping instructions can be used to perform Multiplication and Division on them. The ALU here takes input from the two main registers A and B through 2-state outputs so that the data is available for operation at all instants. The result is then output to the data bus of the computer which then can store the result in any register or in the memory.

## Components:
- IC 7483 : 4 bit full adder X2
- IC 7486 : Quad XOR Gate X2
- Red LEDs X8


## Circuit Diagram:
The ALU is a combinational digital circuit this circuit is made up of two 4 bit adders with the output carry of the first nibble* chained to the input carry of the upper nibble, this makes the setup able to process 8 bit data.
The input is two 8 bit binary numbers and the number stored in B is first taken through a XOR gate which we can all call a controlled inverter, The first input of the XOR gate is the SUB line and the second input is the second operand that you want to work with. 
If the SUB line is LOW:
	 One input to XOR is 0 and the other input passes just as it is (1 if it's 1, 0 if it's 0 as XOR gate acts like a dissimilarity detector).
If the SUB line is HIGH (we want to perform subtraction):
	 One input to the XOR gate is 1 and thus the other input is just inverted from the original input. 
The SUB line is also connected to the input carry of the lower nibble part of adder ic 

When the SUB line is low the circuit performs an addition of the two input binary numbers.
When the SUB line is HIGH The circuit uses the two's complement method to perform a subtraction of the two input  numbers. 
	A-B = A + 2's complement of B (for any binary number)
	 2's complement = 1's complement + 1
	 1's complement is just inversion of any binary number 

Thus the inversion is performed by the XOR gate and +1 is done by the input carry being high and the resulting 2's complement of the number is added to input A and the result is a subtraction.
If the MSB of the result of subtraction is 1 it is a negative number and the modulus can be found by again taking the 2's complement of the result.
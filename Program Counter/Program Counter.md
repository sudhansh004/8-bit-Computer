
A program counter is a **Register** in the CPU which is responsible for the execution of instructions. During a program run the Program Counter outputs the address to the memory location from which the next instruction or data is to be fetched onto the **data bus**. The [[MAR]]  then fetches the particular instruction from the RAM and it is decoded and executed by the [[Control Unit]]

In this SAP we have a 4 bit counter which can access 16 memory locations (from 0000 to 1111). To keep it simple I have not implemented jump instructions thus the Program Counter only gives output to the **data bus** and does not take any input addresses. 

## Components :
- IC 74107 X 2 : JK Flip flop 4 bits
- IC 74125 X 1 : 4 bit buffer register

## Circuit : 

The Circuit is an **asynchronous** 4 bit counter made of 4 JK Flip Flops being used as T Flip-Flops. The **Cp** line when high makes all J = K = 1 thus for every rising clock pulse the Flip-Flops will now toggle from their previous state. 

- The LSB flip-flop toggles on every clock pulse.
- Each subsequent flip-flop toggles when the previous bit changes from `1 → 0`.
-  This circuit is also known as a **Frequency divider** as each progressing bit divides the Frequency of the clock by 2.

I decided to go with the asynchronous design for the simplicity of circuit as a synchronous design will require other logic gates to make a counter, but that introduces a slight delay in the transition of each bit which may cause issues for higher clock speeds or a larger Program Counter like 8/16bits.




**Refer to the circuit diagram and video for better understanding**




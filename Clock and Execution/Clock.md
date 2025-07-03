
Every computer contains sequential circuits that require a clock signal for synchronization. The clock circuit in this SAP-1 design provides a steady pulse train (max 3.3V) enabling the fetch-decode-execute cycle in order. The final clock frequency is ~1 kHz with a 50% duty cycle.

---
### Components

- 555 Timer IC 
- IC 74107 – Master-Slave JK Flip-Flop
- 2 × Capacitors – 0.01 μF
- 3 × Resistors – 18 kΩ (R2), 36 kΩ (R1 = 2×18 kΩ)

---
### Working Principle
#### 555 Timer (Astable Mode)

- On power-up, the capacitor at pin 6 charges via R1 and R2.
- Once the voltage exceeds the trigger voltage **1.67V (1/3 Vcc)**, the lower comparator sets the flip-flop, turning output **HIGH**.
- Charging continues until voltage crosses the threshold voltage **3.3V (2/3 Vcc)**; this triggers the upper comparator, setting output **LOW**.
- The LOW output turns ON the discharge transistor which grounds pin 7, discharging the capacitor through R2.
- When voltage drops below 1.67V the lower comparator again sets the Flip Flop and the cycle restarts.
#### Output (before flip-flop):

- **TimeON** (Time taken by capacitor to charge from 1.6v-3.3v) ≈ (R1 + R2) × C × ln(2) ≈ 374 μs
- **TimeOFF** (Time taken by the capacitor to discharge from 3.3v-1.6v) ≈ R2 × C × ln(2) ≈ 124 μs
- **Period** ≈ 498 μs
- **Frequency** ≈ 2 kHz
- **Duty Cycle** ≈ 75%

---
###  Duty Cycle Correction
To achieve a 50% duty cycle:
- A **JK Flip-Flop** (IC 74107) is added.
- The 555 output is connected to the **clock input** of the flip-flop.
- **J = K = HIGH** configures it as a toggle flip-flop. (Later will be used a HLT' pin for the computer)
- This configuration toggles the output of FF with every falling edge of the input thus utilizing the whole Time Period as ON time and OFF Time, giving us a duty cycle of 50%

#### Final Output:
- **Frequency** ≈ 1 kHz
- **Duty Cycle** ≈ 50%
- **Voltage Level**: TTL-compatible 3.3V peak

---

*I didn't have access to an oscilloscope to analyze the outputs, i wrote a simple Arduino Script to measure the pulse durations. That not being so precise still gave a satisfying result.*


### Manual / Auto Debouncer:
We needed a switch to change the operating mode of the computer from **automatic**(completing the process continuously) to **manual** (processing the program step by step by pressing the single-step button). That is helpful in debugging the circuits and programs on the computer. A debouncer is needed as a normal switch when pressed gives out a transition from suppose ON to OFF while bouncing between ON and OFF for a short period of time, that may cause irregularities in the computer and we need a debouncer to smooth out the fluctuations.
I created mine using an SPDT switch and an SR latch, when the button is toggled the input S = 1 and R = 0 (refer to the circuit diagram) setting the output Q = 1. Turning the Clock into manual mode.


### Single Step:
During the manual-mode the computer needs an input pulse through a button to proceed step by step, I used another SR Latch to for debouncing this button which is an SPST push button thus i also had to add a NOT gate to send inverted outputs to the S and R inputs of the latch, when the button is pressed it turns on the gate with the input from Manual switch generating a pulse executing the next step (check circuit diagram).


### Clear/Start:
A button is needed to start the operation of the computer by initializing all the registers and program counter to start the execution of the program. I used an SR latch Debouncer for this switch as well which was an SPST push button, when the button is pressed a CLR signal is sent to all the registers and the clock to initialize the program and releasing the button starts the program.


### Components:
- IC 7400 X 2 : 4 - 2 input Nand Gates
- IC 7410 X 1 : 3 - 3 input Nand Gates
- IC 7404 X 1: 6 Not gates
- SPST Push button X 2
- SPDT Toggle switch X 1






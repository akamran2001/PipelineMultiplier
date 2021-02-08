# PipelineMultiplier
Pupline Multiplier circuit built using LogiSim
Ahmed Kamran &amp; Ziad Attia 8 bit Pipeline Multiplier 12/12/20

8-Bit Pipeline Multiplier ![](RackMultipart20210208-4-1jfjj75_html_f7fd3e766a07ebb2.png)

Instructions on how to use the multiplier:

- Inputs A1-A8 are the multipliers.
- Inputs B1-B8 are the multiplicands.
- Outputs AB1-AB8 are the products of the corresponding multipliers and multiplicands.
  - AB1 corresponds to the product of A1 and B1
  - AB2 corresponds to the product of A2 and B2
  - So and so forth for the rest.
- The SUM output adds the result of outputs AB1-AB8
- Enter the desired numbers into the inputs
- Then repeatedly click on the clock until an output is produced. Alternatively, click tick enabled in the Logisim toolbar.
- To clear the registers, click on the reset input.

Input Control: ![](RackMultipart20210208-4-1jfjj75_html_b6da44e320898fe6.png)

The input control takes the eight multiplicands and eight multipliers. It then uses two multipliers connected to a 3-bit counter to select a pair of multipliers &amp; multiplicands from top to bottom. Each bit of the multiplier is then fed into the control input of 8 more multiplexers. Each of those eight multiplexers is connected to the selected multiplicand along with a 0 constant. If the multiplier bit is 0 then the output is eight 0 bits. If the multiplier bit is 1 then the output is the multiplicand.

Shift-Add Circuit: ![](RackMultipart20210208-4-1jfjj75_html_14a9899762dee2b6.png)

The output from the eight multiplexers then flows through the circuit. The circuit performs shift-add multiplication, repeatedly shifting the bits and adding them.

Because this is an 8-bit multiplier, there are three rows of adders since 2^3=8. The first row has four adders, the next row 2, and the third 1. By repeatedly shifting the bits to the left and adding them, the product is produced.

Clock, Reset &amp; Counter: ![](RackMultipart20210208-4-1jfjj75_html_14a9899762dee2b6.png)

The clock is a central component of the circuit. When the clock is on the rising edge, the registers save the data coming in. When the clock is on the falling edge, the counter increments by 1. The 3-bit counter counts from 0-7 and then goes back to zero. Each increment of the counter selects a new pair of inputs from the input control and also selects a new pair of outputs from the output control. The reset button clears all register &amp; counter values.

Registers:

The multiplication circuit was divided into three stages (three-stage multiplication pipeline), which means that three multiplication operations with different operands will be in execution during each clock cycle. The three stages of multiplication are input control and the first level of additions, the second level of additions, and the final addition and output control. Between every two stages, a set of registers (R1 and R2) were added to hold data that is needed for the final result of the multiplication. ![](RackMultipart20210208-4-1jfjj75_html_34db73e91641ab19.png)

R1 stores the 10-bit results of the four first-level adders (results of adder1 and adder3 are extended) that are going to be the inputs for the 2 second-level adders and the 4 lower bits that are not going to be included in the 2 second-level adders (2 for each adder). So, R1 stores 6 values.

R2 stores a 3-bit number whose two lower bits are the first (lower) unused bits from R1, and its higher bit is the lowest bit from the result of adder5, a 13-bit extended number whose lower bits are the 1-9 bits of adder5&#39;s result, and a 13-bit shifted number whose 1-2 bits are the second unused (higher) bits from R1 and 3-12 bits are the result of adder6. ![](RackMultipart20210208-4-1jfjj75_html_6b7d0f118962f5ae.png)

The two 13 bits are the inputs of adder7. The 13-bit result of adder7 (higher bits) with the 3-bit number (lower bits) from R2 form the result of the entire multiplication.

Output Control: ![](RackMultipart20210208-4-1jfjj75_html_5e5772ad3c1f1ff3.png)

The Output Control consists of an 8-output decoder (1 for each multiplication), and the three select bits are the result of the addition of the counter value and binary 6 because multiplication reaches the final stage, the counter would be two counts ahead, so the 3-bit adder calculates the original count, so that we can store the result in its correct place (e.g., A1\*B1=AB1). Each output of the decoder is ANDed with the clock, and the result triggers the corresponding register. The result of the multiplication is connected to the inputs of those registers, but only the corresponding register gets triggered and stores the value ![](RackMultipart20210208-4-1jfjj75_html_5e5772ad3c1f1ff3.png)

Accumulate: ![](RackMultipart20210208-4-1jfjj75_html_f748b42a7a907f48.png)

The register values of the output control are added using three layers of adders (7 adders) to find the sum of all multiplications (A1\*B1 + A2\*B2 + … + A8\*B8).

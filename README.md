# SHREK
An Experimental Bit Serial Architecture

Bit Serial Computers have been around for 80 years - but nobody seemed to notice.

Born out of 1940's wartime electronics, bit serial architectures would dominate the early days of mainframes and desk top calculators.

Despised as being "slow" they are an excellent architecture for teaching digital design.

In 1948, the Cambridge University EDSAC achieved 600 instructions per second. The "SHREK" will do at least 50,000 instructions per second, on a small circuit board costing $30.

Buckle up and enjoy the ride....

Part 1 - the ALU

So, any bit serial CPU begins with an ALU. (Arithmetic Logic Unit). Its function is to add and subtract and provide the common logic functions AND, XOR and OR.

The simplest design I can make from basic gates consists of two XOR gates and 6 NAND gates.

It is based around a full adder, which adds the bits supplied by the A and B shift registers, plus any carry generated from the last bit-addition.

The SUM of two bits is the XOR function and the CARRY is the AND function.

A multiplexer on the output of the full adder allows you to select between AND and XOR. If you select neither (I1:I0=0) you get zero and if you select both (I1:I0=1) you get the OR of A and B.

To turn this into a practical design we must find a way to invert the B input when we wish to do subtraction (another XOR gate) and a way of suppressing the carry from propagating, when we are doing Boolean logic functions rather than arithmetic (two more NAND gates).

In the sketch below, the inputs from the shift registers are Ain and Bin. The carry from the previous bit-sum is Cin. 
I0 and I1 control the selection from the output multiplexer.

Fout is the function produced by this ALU. It will be fed back into the input of the Accumulator shift register.
Cout is the carry output.  It will pass through the Carry Suppress gates and then into a D-type flipflop where it will be held until the next bit-sum operation.

![image](https://github.com/user-attachments/assets/f0b16c7c-96f8-4e50-bd40-2e3ff0cb1d48)


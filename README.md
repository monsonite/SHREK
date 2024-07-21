# SHREK
An Experimental CPU using a Bit Serial Architecture

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

Part 2:

Here is the augmented ALU with the means to invert B for subtraction and suppress the Carry, when we are doing the logical operations AND, OR, XOR.

As you can see, it uses most of a 74HC86 (1 gate unused), quad 2-input XOR, plus two 74HC00 quad 2-input NAND gates.

This makes a package count of 3 when we use basic gates.

Further reductions are possible - and that will be the subject of a later post.

![image](https://github.com/user-attachments/assets/e130fa44-8b48-4507-b1be-37911d8bf257)

Part 3. Timing.

Any CPU has signals that co-ordinate the various operations, controlled by a clock input.

A Bit Serial CPU is no exception, it has its own specific timing pulse requirements - needed to control the hardware and the transfer of serial data between shift registers and memory.

Getting the Timing Pulse Generator (TPG) correct is fundamental to any bit serial architecture.

If we have a 16-bit machine, we must generate a burst of 16 clock pulses to move data between the shift registers.

But outside of this "gated clock burst" GCLK, we need control pulses before it and after it. I call these head and tail pulses, and we need circuitry to implement them.

In parts 1 and 2 we only dealt with combinational logic. Now we need to employ flipflops, counters and other sequential logic parts.

So from experimentation, I found that I need 2 head pulses T0 and T1, followed by 16 gated clocks T2 to T17, and then 2 tail pulses T18 and T19.

The Timing Pulse Generator (TPG - though sometimes I call it the clock sequencer) must produce these signals relentlessly for every instruction executed.

To produce a burst of 16 gated clock pulses is fairly easy with a 4-bit counter such as the 74HC161.

The head and tail pulses are produced from a quad D-type flipflop (D-FF) type 74HC175.

In addition we need a SR latch (Set-Reset Latch) to make our clock gating signal. This is done using a single 74HC00 quad 2-input NAND gate.

So in the last 3 parts we have introduced the ALU and the TPG.

The next part will look at the Instruction Decoder and its associated circuitry.

![image](https://github.com/user-attachments/assets/6a0faace-34f5-44a8-aa10-8c763acc226e)

Part 4. Instruction Decoding.

With any CPU, we have instructions stored in ROM or RAM, and we need to convert these instructions into several control signals that will co-ordinate the operation of the hardware
.
In keeping with minimalist computing goals, my bit serial CPUs have just 8 instruction groups:

000 LOAD

001 AND

010 OR

011 XOR

100 ADD

101 SUB

110 STORE

111 JUMP

So to decode these groups we start with a 74HC138, 3 to 8 line decoder.

From its outputs !Y0 to !Y7 we must add combinational logic to create the necessary control signals.

The sketch below shows what we are trying to achieve:

![image](https://github.com/user-attachments/assets/c9bf701e-baad-4ae1-a2dc-e85deaf86ed3)










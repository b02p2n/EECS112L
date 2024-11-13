java c
Organization of Digital Computers Lab
EECS112L
Fall 2024
Lab 1 (100 Points)
In this lab, you are given an arithmetic logic unit (ALU) design, which you are to test. The design has some bugs in it. You will design a testbench to test the functions of the ALU to find the bugs. You will also revise the sequential logic (D flip-flops) and create testbench for it.
1 ALU Overview
An ALU performs basic arithmetic and logic operations. Examples of arithmetic operations are addition, subtraction, multiplication, and division. Examples of logic operations are comparisons of values ,such as NOT, AND, and OR. This ALU has 2 4-bit inputs A and B for operands, a 4-bit input ALU_Sel for selecting the operation, a 4-bit output ALU_Out, and a 1-bit output CarryOut. The datapath is just 4-bits wide to allow for easy coverage testing of the different operations. The operations of the ALU are specified in the table below.
ALU_Sel          ALU Operation
0000              ALU ut = A + B
0001              ALU ut = A - B
0010              ALU ut = A * B
0011              ALU ut = A / B
0100              ALU ut = A << 1
0101              ALU ut = A >> 1
0110              ALU ut = A rotated left by 1
0111              ALU ut = A rotated right by 1
1000              ALU ut = A and B
1001              ALU ut = A or B
1010              ALU ut = A xor B
1011              ALU ut = A nor B
1100              ALU ut = A nand B
1101              ALU ut = A xnor B
1110              ALU ut = 1 if A>B else 0
1111              ALU ut = 1 if A=B else 0
Note: Check canvas for some review slides on Verilog operators.
Below you see the Verilog code for the ALU. Create a new Vivado project and copy this code to a file named alu.v.
Code 1: 4-bit ALU.module alu (input [3:0] A,B,// ALU 4- bit Inputsinput [3:0] ALU_Sel ,// ALU Selectionoutput [3:0] ALU_Out , // ALU 4- bit Outputoutput CarryOut // Carry Out Flag);reg [7:0] ALU_Result ;wire [4:0] tmp ;assign ALU_Out = ALU_Result ; // ALU outassign tmp = {1’b0 ,A} + {1’b0 ,B};assign CarryOut = tmp [4]; // Carryout flagalways @ (*)begincase ( ALU_Sel )4’ b0000 : // AdditionALU_Result = A + B;4’ b0001 : // SubtractionALU_Result = A - B;4’ b0010 : // MultiplicationALU_Result = A ** B;4’ b0011 : // DivisionALU_Result = A/B;4’ b0100 : // Logical shift leftALU_Result = A <=1;4’ b0101 : // Logical shift rightALU_Result = A > >1;4’ b0110 : // Rotate leftALU_Result = {A [2:0] , A [3]};4’ b0111 : // Rotate rightALU_Result = {A[3] ,A [3:1]};4’ b1000 : //Logical andALU_Result = A  B;4’ b1001 : //Logical orALU_Result = A || B;4’ b1010 : //Bitwise xorALU_Result = A ˆ B;4’ b1011 : //Logical norALU_Result = ∼(A || B);4’ b1100 : // Logical nandALU_Result = ∼(A  B);4’ b1101 : // Bitwise xnorALU_Result = ∼ (A ˆ B);4’ b1110 : // Greater comparisonALU_Result = (A>B) ? 4’b0 : 4’b1;4’ b1111 : // Equal comparisonALU_Result = (A==B) ? 4’b1 : 4’b0;default : ALU_Result = A + B;endcaseendendmodule
2 Verification of Combinational Logic - ALU
1. Create the testbench, called tb alu.v and add it to your Vivado project. You can use the testbench from lab 0 as a starting point.
2. Test the Addition (0000) operation.
(a) (15 points) Modify the testbench to provide stimulus that sets ALU sel to the appropriate op code and sweeps through all possible combinations of inputs from A=0000, B=0000 to A=1111, B=1111. Include the simulation waveform. in your report. (Note: only report several combinations of A and B. Explain the different parts of these waveform. in details such as, the inputs, the outputs, and the expected result.)
(b) (15 points) Is the operation implemented correctly? If not, indicate on the waveform. where at least one of outputs was incorrect. Identify which line in alu.v contains the mistake, and propose a correction to this line in you report. Create a new file called alu fixed.v with the mistakes being corrected. The ports declaration should be the same as alu.v and the name of the new module should be alu fixed. Any changes in the port declaration will cause your module to fail during grading and you will lose all the points.
3. (15 points) Repeat Step 2(a-b) for the following operations: multiplication, logical shift left, rotate right, logical AND, and greater comparison.
Note: If the operation is not implemented correctly, you need to report the simulation waveform. for at least one of the parts for which the output was incorrect. If the operation is implemented correctly, you need to report the simulation waveform. for only 2-3 parts of the waveform. and show why the outputs are correct.
Note: Remember the fixed version of your ALU has to fix all the bugs. For example, the carryout is not implemented correctly in the given ALU. You need to think how to fix it to make sure that your operations are running correctly.
Note: Your module name of the the fixed version of the alu has to be called (alu fixed) with the same ports names as the provide module above. The file name has to be called (alu fixed.v)
3 (40 points) Verification of Sequential Logic - D flip-flop
In lecture 1, we covered D flip-flop as an example for sequential logic circuits. Use the code from the lecture as a start and implement the following two versions of the D flip-flop:
• D flip-flop with synchronous set/reset. (filename: dff_SR.v)
• D flip-flop with asynchronous preset/clear. (filename: dff_PC.v)
Design two testbenches to test f代 写EECS112L Organization of Digital Computers Lab Fall 2024 Lab 1R
代做程序编程语言or the correct operation of the two version of D flip-flop. Show the waveform. from your testbench. Explain on your waveform. how your flip-flop works correctly as expected. File names should be (filename: tb_dff_SR.v), and(filename: tb_dff_PC.v).
The module declarations in your code for this question must be as follows:
• D flip-flop with synchronous set/reset: module dff SR (clk, d, set, reset, q);
• D flip-flop with asynchronous preset/clear: module dff PC (clk, d, preset, clear, q);
Note: If the simulation of the new testbenches does not show up, you should do the following:
• File > Close Simulation
• Right-Click on the testbench you want to simulate > Set as Top
• Run the simulation again.
(10 points design + 10 points tb and waveform. for each FF)
4 (15 points) Midterm Question Winter 2022
Below is a Verilog code for synchronous reset (active low), synchronous load for an 8-bit counter with carryout.
Convert it into asynchronous reset (active low), synchronous load 8-bit counter with carryout.
(Full points will be based on the correct design and syntax.)
Code 2: 4-bit ALU.module c tr 8 (output reg [ 7 : 0 ] counter ,output reg carryout ,input [ 7 : 0 ] data , input load , clk , r e s e t );always @( posedge c l k )begini f (! r e s e t ) { carryout , count e r } <= 9 ’ b0 ;e ls e i f ( load ) { carryout , count e r } <= data ;e ls e { carryout , count e r } <= count e r + 1 ’ b1 ;endendmodule
The solution for this question is a written solution and has to be added to the submitted report. No need for Verilog design file for this one (Check the assignment deliverables).
5 Assignment Deliverables
Your submission should include the following:
• Testbench for the provided alu.v, the new proposed ALU design (tb alu.v, alu fixed.v).
• Design and testbench files for the two versions of the D flip-flops (dff SR.v dff PC.v, tb_dff_SR.v, and tb_dff_PC.v).
• A report in pdf format. Follow the rules in the “sample report” under “additional resources” in the Canvas. The report file name should be “firstname_last_name_Lab1.pdf”
Note1: Compress all files (7 files : 6 .v files + report) into zip and upload to canvas before deadline.
Note2: Use the code samples that are given in the lab description and in the lectures. The module part of your ALU should exactly look like the code sample otherwise you lose the lab points (module name, module declaration, ports names, and ports declaration) - If you are not sure, please ask your TA during the discussion if your submission files follow the submission guidelines.
Note3: The name of the files should be exactly as mentioned above.
Note4: Work individually and submit before the deadline.
If you are using Vivado on the server, you need to copy your file from the server to some local drive on your computer.
You can type this command (change the file path) in the terminal:
scp username@hostname:/path/to/remote/file /path/to/local/file
6 Extra Exercise - No submission needed for this part
In order to review the digital design basics and syntax, it is highly recommended that you try to implement the following Digital Blocks. It will help you with the future labs and with the Verilog syntax.
• Multiplexer
• Decoder
• Encoder
• Comparator
• Half Adder
• Full Adder
• Adder/Subtractor
• Sign extender
• Shift Register
• Complementer
Revise single cycle processor (data path + control path) from EECS112.
Appendix
set/reset in D flip-flop
when set is “0” and reset is “0”, then output Q will follow the input D.
when set is “1” and reset is “0”, then output Q is 1.
when set is “0” and reset is “1”, then output Q is 0.
when set is “1” and reset is “1”, it is an illegal combination of the inputs. (We are just going to avoid this combination of inputs in our test.)
preset/clear in D flip-flop
when preset is “0” and clear is “0”, then output Q will follow the input D.
when preset is “1” and clear is “0”, then output Q is 1.
when preset is “0” and clear is “1”, then output Q is 0.
when preset is “1” and clear is “1”, it is an illegal combination of the inputs. (We are just going to avoid this combination of inputs in our grading testbench).
Carryout in ALU
Operation                              CarryOut
A + B                                    the 5th bit (carry out)
A − B                                    the barrow bit
A ∗ B                                     the 5th bit of the result
A / B                                     the 5th bit of the result (result for divide by 0 are undefined or X)
A << 1                                   the shifted bit
A >> 1                                       -
rotate left                                   -
rotate right                                  -
and                                             -
or                                                -
xor                                               -
nor                                              -
nand                                             -
xnor                                              -
greater than                                   -
equal                                             -
You can find more about these topics in any logic design or computer architecture book (materials from EECS31 or EECS112 courses)







         
加QQ：99515681  WX：codinghelp  Email: 99515681@qq.com

# BARREL SHIFTER

## Introduction
A barrel shifter is a digital circuit that can shift a data word by a specified number of bits without the use of any sequential logic, only pure combinational logic, i.e. it inherently provides a binary operation. It can however in theory also be used to implement unary operations, such as logical shift left, in cases where limited by a fixed amount (e.g. for address generation unit). One way to implement a barrel shifter is as a sequence of multiplexers where the output of one multiplexer is connected to the input of the next multiplexer in a way that depends on the shift distance. A barrel shifter is often used to shift and rotate n-bits in modern microprocessors,typically within a single clock cycle.

For example, take a four-bit barrel shifter, with inputs A, B, C and D. The shifter can cycle the order of the bits ABCD as DABC, CDAB, or BCDA; in this case, no bits are lost. That is, it can shift all of the outputs up to three positions to the right (and thus make any cyclic combination of A, B, C and D). The barrel shifter has a variety of applications, including being a useful component in microprocessors (alongside the ALU).

![image](https://github.com/KushalR17/Pes_bs/assets/142383052/7d19f3a6-d4e7-46f7-9781-c3c2bc4daf8d)


![image](https://github.com/KushalR17/Pes_bs/assets/142383052/2dd4160d-fe43-4159-9287-f06882e4c3f4)

## Barrel shifter Truth table

The truth table of barrel shifter is given by :

![image](https://github.com/KushalR17/Pes_bs/assets/142383052/e1334fe8-71c1-4abf-92db-9b0e30867ff3)

## verilog code for Barrel shifter

The Verilog code for barrel shifter is given below :
```
module pes_bs_8bit (in, ctrl, out);
  input  [7:0] in;
  input [2:0] ctrl;
  output [7:0] out;
  wire [7:0] x,y;

//4bit shift right
mux2X1  ins_17 (.in0(in[7]),.in1(1'b0),.sel(ctrl[2]),.out(x[7]));
mux2X1  ins_16 (.in0(in[6]),.in1(1'b0),.sel(ctrl[2]),.out(x[6]));
mux2X1  ins_15 (.in0(in[5]),.in1(1'b0),.sel(ctrl[2]),.out(x[5]));
mux2X1  ins_14 (.in0(in[4]),.in1(1'b0),.sel(ctrl[2]),.out(x[4]));
mux2X1  ins_13 (.in0(in[3]),.in1(in[7]),.sel(ctrl[2]),.out(x[3]));
mux2X1  ins_12 (.in0(in[2]),.in1(in[6]),.sel(ctrl[2]),.out(x[2]));
mux2X1  ins_11 (.in0(in[1]),.in1(in[5]),.sel(ctrl[2]),.out(x[1]));
mux2X1  ins_10 (.in0(in[0]),.in1(in[4]),.sel(ctrl[2]),.out(x[0]));

//2 bit shift right

mux2X1  ins_27 (.in0(x[7]),.in1(1'b0),.sel(ctrl[1]),.out(y[7]));
mux2X1  ins_26 (.in0(x[6]),.in1(1'b0),.sel(ctrl[1]),.out(y[6]));
mux2X1  ins_25 (.in0(x[5]),.in1(x[7]),.sel(ctrl[1]),.out(y[5]));
mux2X1  ins_24 (.in0(x[4]),.in1(x[6]),.sel(ctrl[1]),.out(y[4]));
mux2X1  ins_23 (.in0(x[3]),.in1(x[5]),.sel(ctrl[1]),.out(y[3]));
mux2X1  ins_22 (.in0(x[2]),.in1(x[4]),.sel(ctrl[1]),.out(y[2]));
mux2X1  ins_21 (.in0(x[1]),.in1(x[3]),.sel(ctrl[1]),.out(y[1]));
mux2X1  ins_20 (.in0(x[0]),.in1(x[2]),.sel(ctrl[1]),.out(y[0]));
mux2X1  ins_07 (.in0(y[7]),.in1(1'b0),.sel(ctrl[0]),.out(out[7]));
mux2X1  ins_06 (.in0(y[6]),.in1(y[7]),.sel(ctrl[0]),.out(out[6]));
mux2X1  ins_05 (.in0(y[5]),.in1(y[6]),.sel(ctrl[0]),.out(out[5]));
mux2X1  ins_04 (.in0(y[4]),.in1(y[5]),.sel(ctrl[0]),.out(out[4]));
mux2X1  ins_03 (.in0(y[3]),.in1(y[4]),.sel(ctrl[0]),.out(out[3]));
mux2X1  ins_02 (.in0(y[2]),.in1(y[3]),.sel(ctrl[0]),.out(out[2]));
mux2X1  ins_01 (.in0(y[1]),.in1(y[2]),.sel(ctrl[0]),.out(out[1]));
mux2X1  ins_00 (.in0(y[0]),.in1(y[1]),.sel(ctrl[0]),.out(out[0]));

endmodule

/////////////////////////
//2X1 Mux
/////////////////////////
 
module mux2X1( in0,in1,sel,out);
input in0,in1;
input sel;
output out;
assign out=(sel)?in1:in0;
endmodule
```

![bs1](https://github.com/KushalR17/Pes_bs/assets/142383052/7ede8612-e0bc-4538-a9bc-0ff4d5f1db75)

## Testbench code for barrel shifter :
```
module pes_bs_8bit_tb;
  reg [7:0] in;
  reg [2:0] ctrl;
  wire [7:0] out; 
  
pes_bs_8bit uut(.in(in), .ctrl(ctrl), .out(out));
  
initial 
 begin
   $dumpfile("pes_bs_8bit_tb.vcd");
   $dumpvars(0,pes_bs_8bit_tb);
    $display($time, " << Starting the Simulation >>");
        in= 8'd0;  ctrl=3'd0; //no shift
    #10 in=8'd128; ctrl= 3'd4; //shift 4 bit
    #10 in=8'd128; ctrl= 3'd2; //shift 2 bit
    #10 in=8'd128; ctrl= 3'd1; //shift by 1 bit
    #10 in=8'd255; ctrl= 3'd7; //shift by 7bit
  end
    initial begin
      $monitor("Input=%d, Control=%d, Output=%d",in,ctrl,out);
    end
endmodule
```

![bs2a](https://github.com/KushalR17/Pes_bs/assets/142383052/ef36e01c-fb48-4dea-9a48-b704dbae682b) 

## RTL Pre simulation
Code for the simulation is given below:
```
iverilog pes_bs_8bit.v pes_bs_8bit_tb.v
```
```
./a.out
```
```
gtkwave pes_bs_8bit_tb.vcd
```

![bs3](https://github.com/KushalR17/Pes_bs/assets/142383052/18447840-b146-45c3-8cf7-08dfdab9d9d4)

## Synthesis
Code for the synthesis is given below:
```
yosys> read_liberty -lib /home/kushal/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
```
yosys> read_verilog pes_bs_8bit.v
```
```
yosys> synth -top pes_bs_8bit
```

![bs41](https://github.com/KushalR17/Pes_bs/assets/142383052/cd940cd8-dcc4-4ddc-91af-cd09d3de78e3)

![bs5](https://github.com/KushalR17/Pes_bs/assets/142383052/90ffcb47-386f-487c-a032-440177d2eafc)

```
yosys> abc -liberty /home/atom/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
```
yosys> show
```
### Synthesized Circuit 

![bs7b](https://github.com/KushalR17/Pes_bs/assets/142383052/a734509f-29a5-4a7b-a659-f1aa6d7712e7)

![bs7a](https://github.com/KushalR17/Pes_bs/assets/142383052/8b53018e-4217-4850-bce6-899fb86d20d0)

## Gate level simulation (GLS) Post-Simulation
Code for gate level simulation is given below:
```
iverilog /home/kushal/VLSI/sky130RTLDesignAndSynthesisWorkshop/my_lib/verilog_model/sky130_fd_sc_hd.v pes_bs_8bit.v pes_bs_8bit_tb.v
```
```
./a.out
```
```
gtkwave pes_bs_8bit_tb.vcd
```

![bs6](https://github.com/KushalR17/Pes_bs/assets/142383052/93d16616-7fe7-44e9-aa05-14c7c299c76d)


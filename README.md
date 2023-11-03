# pes_bs_8bit
# Contents
- [BARREL SHIFTER](#barrel-shifter)
- [RTL Pre simulation](#rtl-pre-simulation)
- [Gate level Post Simulation](#gate-level-post-simulation)
- [Synthesis](#synthesis)
- [Installation of ngspice magic and OpenLANE](#installation-of-ngspice-magic-and-openlane)
- [OpenLANE Flow](#openlane-flow)

## BARREL SHIFTER

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

## Gate level Post Simulation
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



## Installation of ngspice magic and OpenLANE

**ngspice**
- Download the tarball from https://sourceforge.net/projects/ngspice/files/ to a local directory
```
cd $HOME
sudo apt-get install libxaw7-dev
tar -zxvf ngspice-41.tar.gz
cd ngspice-41
mkdir release
cd release
../configure  --with-x --with-readline=yes --disable-debug
sudo make
sudo make install
```

**magic**
```
sudo apt-get install m4
sudo apt-get install tcsh
sudo apt-get install csh
sudo apt-get install libx11-dev
sudo apt-get install tcl-dev tk-dev
sudo apt-get install libcairo2-dev
sudo apt-get install mesa-common-dev libglu1-mesa-dev
sudo apt-get install libncurses-dev
git clone https://github.com/RTimothyEdwards/magic
cd magic
./configure
sudo make
sudo make install
```

**OpenLANE**
```
sudo apt-get update
sudo apt-get upgrade
sudo apt install -y build-essential python3 python3-venv python3-pip make git

sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io
sudo docker run hello-world
sudo groupadd docker
sudo usermod -aG docker $USER
sudo reboot 
# After reboot
docker run hello-world (should show you the output under 'Example Output' in https://hub.docker.com/_/hello-world)

- To install the PDKs and Tools
cd $HOME
git clone https://github.com/The-OpenROAD-Project/OpenLane
cd OpenLane
make
make test
```


## OpenLANE Flow

![pd1](https://github.com/KushalR17/Pes_bs/assets/142383052/d0047cf2-a19c-4692-9ac2-f1cd4c36bbac)
- First we create a folder under the name of our design in the 'designs' folder.
- Do ```cd pes_bs_8bit```

![pd2](https://github.com/KushalR17/Pes_bs/assets/142383052/bc1dbb16-be88-4f07-a899-d9760bce2ec4)
  - Here we create a config.json file.

![pd3](https://github.com/KushalR17/Pes_bs/assets/142383052/fd7de708-1810-4dc9-a0e0-21d91cafd859)
- We add the following files to this directory.
- All these files are found above in the 'pes_bs_8bit' folder.

![pd5](https://github.com/KushalR17/Pes_bs/assets/142383052/83774711-8035-4e36-a17c-89af5f580788)
 - Now in the main 'Openlane' directory type ```mkdir pdks```.
- Copy paste the above file in it. Found in the verilog_model folder above.


![pd7](https://github.com/KushalR17/Pes_bs/assets/142383052/ee56f5d0-20f2-4bc1-a843-22589c36282e)

 - Type ```make mount``` in the main Openlane folder.
- Then type ```./flow.tcl -interactive```.
- To prep the design type
```
prep -design pes_bs_8bit
```

![pd8](https://github.com/KushalR17/Pes_bs/assets/142383052/3fc035ca-9027-4d34-8c68-8b2fb5aa204c)

### Synthesis
- Type
```
run_synthesis
```

**Physical Cells**

![pdsynthesis](https://github.com/KushalR17/Pes_bs/assets/142383052/eef3741e-7fdd-4e07-b24c-23f074750262)


### Floorplan
- Now to run the floorplan we type
```
run_floorplan
```
![pd9](https://github.com/KushalR17/Pes_bs/assets/142383052/10c43a08-69d8-46b4-bb04-29993862cb4f)

- To view the design we type
```
magic -T /home/kushal/OpenLane/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def pes_bs_8bit.def &
```
![pd11](https://github.com/KushalR17/Pes_bs/assets/142383052/f84771db-bd6d-4973-a87a-a51545bacf9b)


### Placement
- Now to run the placement we type
```
run_placement
```
![pd_12](https://github.com/KushalR17/Pes_bs/assets/142383052/1fba9061-b6a9-4f43-bf12-414225244f34)

- To view the design we type
```
magic -T /home/kushal/OpenLane/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def pes_bs_8bit.def &
```
![pd13](https://github.com/KushalR17/Pes_bs/assets/142383052/b02fb04d-e2b7-4e87-a924-0bf96778865c)


### CTS(Clock Tree Synthesis)
- Now to run cts we type
```
run_cts
```
![pd15](https://github.com/KushalR17/Pes_bs/assets/142383052/20d1c1b4-4fcf-4b7e-8e1c-2f17d1c0bae7)

- The reports generated are given below
![pd16](https://github.com/KushalR17/Pes_bs/assets/142383052/039aa11e-df3a-43e5-8975-b76c70c8349b)
![pd17](https://github.com/KushalR17/Pes_bs/assets/142383052/0cf3e39b-5f5d-4c0a-9baf-7096a7e389cb)
![18](https://github.com/KushalR17/Pes_bs/assets/142383052/bdc7d8ef-bd96-40e3-be4b-fcc343bcee4b)
![19](https://github.com/KushalR17/Pes_bs/assets/142383052/89935c6f-6fcb-4429-ba3f-0dbdcb0336f7)
![20](https://github.com/KushalR17/Pes_bs/assets/142383052/d75708aa-2ad2-40df-b6d7-fd5d2280e67f)
![21](https://github.com/KushalR17/Pes_bs/assets/142383052/8d3502ff-233a-4141-b166-04b0dcd380cb)
![22](https://github.com/KushalR17/Pes_bs/assets/142383052/f2ee4f03-23b2-4853-8712-af05a3eb515d)
![23](https://github.com/KushalR17/Pes_bs/assets/142383052/14520e30-ff70-4a8c-870b-53b2a0b2006b)
![24](https://github.com/KushalR17/Pes_bs/assets/142383052/b1a0ad25-8bec-4acb-84a3-dc702f5acc89)
![25](https://github.com/KushalR17/Pes_bs/assets/142383052/27907a00-459a-48c6-91bf-d17a6234fc49)
![26](https://github.com/KushalR17/Pes_bs/assets/142383052/d67ec906-7638-4e0a-8775-218e0320a057)
![27](https://github.com/KushalR17/Pes_bs/assets/142383052/2d05c84c-5176-4b5d-b242-5b7cbdc3e4e8)
![29](https://github.com/KushalR17/Pes_bs/assets/142383052/8a66d264-ced6-48f2-87a0-0e94341d31ef)
![30](https://github.com/KushalR17/Pes_bs/assets/142383052/23664747-25f2-428c-9027-05d0a6cab77b)

**Power, Clock Skew, summary and area Report**

![31](https://github.com/KushalR17/Pes_bs/assets/142383052/b3264549-6573-4704-92f5-222bb416188d)

### Routing
- Now to run routing we type
```
run_routing
```

- To view the design we type
```
magic -T /home/kushal/OpenLane/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def pes_bs_8bit.def &
```
![rou1](https://github.com/KushalR17/Pes_bs/assets/142383052/8c2de2b7-5453-40fb-a2df-316e2d0fde0f)

![pd32](https://github.com/KushalR17/Pes_bs/assets/142383052/80662483-bddd-4372-beb4-4b2f432dc751)

![routing](https://github.com/KushalR17/Pes_bs/assets/142383052/43ae000a-8fba-4a56-a598-2aa7e32fd2e6)

**Power, Clock Skew, summary and area Report**

![31](https://github.com/KushalR17/Pes_bs/assets/142383052/b3264549-6573-4704-92f5-222bb416188d)

**Statistics**
- Area = 0.137 um2
- Internal Power =  8.90e-06W
- Switching Power = 1.15e-05W
- Leakage Power = 3.37e-10W
- Total Power = 2.04e-05W








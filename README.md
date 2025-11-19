# SIMULATION AND IMPLEMENTATION OF 4:1 MULTIPLEXER

## AIM
To design and simulate a 4:1 Multiplexer (MUX) using Verilog HDL in four different modeling styles—Gate-Level, Data Flow, Behavioral, and Structural—and to verify its functionality through a testbench using the Vivado 2023.1 simulation environment. The experiment aims to understand how different abstraction levels in Verilog can be used to describe the same digital logic circuit and analyze their performance.

## APPARATUS REQUIRED
- **Vivado 2023.1**

## Procedure

1. Open **Vivado 2023.1**.  
2. Create a **New RTL Project** and give a name (e.g., `Mux4_to_1`).  
3. Add/create your Verilog files and testbench.  
4. Select an FPGA part (e.g., `xc7a35ticsg324-1L`).  
5. Run **Synthesis** to check for errors.  
6. Run **Simulation** → **Run Behavioral Simulation**.  
7. Observe the waveforms of inputs and outputs.  
8. Adjust simulation time if needed (e.g., 1000ns).  
9. Save the project and take screenshots of results.  
10. Close simulation.  

---

## Logic Diagram
![image](https://github.com/user-attachments/assets/d4ab4bc3-12b0-44dc-8edb-9d586d8ba856)

---

## Truth Table
![image](https://github.com/user-attachments/assets/c850506c-3f6e-4d6b-8574-939a914b2a5f)

---

## Verilog Code

### 4:1 MUX Gate-Level Implementation
```verilog
`timescale 1ns / 1ps
module mux_4_1(i,s,y);
input [3:0]i;
input [1:0]s;
output y;
wire [4:1]w;
and g1(w[1],i[0],~s[1],~s[0]);
and g2(w[2],i[1],~s[1],s[0]);
and g3(w[3],i[2],s[1],~s[0]);
and g4(w[4],i[3],s[1],s[0]);
or g5(y,w[1],w[2],w[3],w[4]);
endmodule
```
### 4:1 MUX Gate-Level Implementation- Testbench
```verilog
module mux_4_1_tb;
reg [3:0]i;
reg [1:0]s;
wire y;
mux_4_1 uut(i,s,y);
initial
begin
i=4'b1001;
s=2'b00;
#10;
$display("selection is %b %b, output is %b",s[1],s[0],y);
s=2'b01;
#10;
$display("selection is %b %b, output is %b",s[1],s[0],y);
s=2'b10;
#10;
$display("selection is %b %b, output is %b",s[1],s[0],y);
s=2'b11;
#10;
$display("selection is %b %b, output is %b",s[1],s[0],y);
$finish;
end
endmodule
```
## Simulated Output Gate Level Modelling

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c9c296da-366e-49f7-a537-3b0d1008ee4b" />



---
### 4:1 MUX Data flow Modelling
```verilog
`timescale 1ns / 1ps

module mux_4_1(i, s, y);
  input [3:0]i;      
  input [1:0]s;     
  output y;           
  assign y = (~s[1] & ~s[0] & i[0]) | 
             (~s[1] &  s[0] & i[1]) |
             ( s[1] & ~s[0] & i[2]) |
             ( s[1] &  s[0] & i[3]);
endmodule
```
### 4:1 MUX Data flow Modelling- Testbench
```verilog
module mux_4_1_tb;
reg [3:0]i;
reg [1:0]s;
wire y;
mux_4_1 uut(i,s,y);
initial
begin
i=4'b0110;
s=2'b00;
#20;
$display("Selection is %b %b, Output is %b",s[1],s[0],y);
s=2'b01;
#20;
$display("Selection is %b %b, Output is %b",s[1],s[0],y);
s=2'b10;
#20;
$display("Selection is %b %b, Output is %b",s[1],s[0],y);
s=2'b11;
#20;
$display("Selection is %b %b, Output is %b",s[1],s[0],y);
$finish;
end
endmodule
```
## Simulated Output Dataflow Modelling

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/72e845e3-1826-4525-9e3e-9b8e4088a1af" />


---
### 4:1 MUX Behavioral Implementation
```verilog
`timescale 1ns / 1ps
module mux_4_1(i,s,y);
input [3:0]i;
input [1:0]s;
output reg y;
always @(i,s)
begin 
if (s[1]==0&&s[0]==0)
y=i[0];
else if (s[1]==0&&s[0]==1)
y=i[1];
else if (s[1]==1&&s[0]==0)
y=i[2];
else
y=i[3];
end
endmodule
```
### 4:1 MUX Behavioral Modelling- Testbench
```verilog
module mux_4_1_tb;
reg [3:0]i;
reg [1:0]s;
wire y;
mux_4_1 uut(i,s,y);
initial
begin
i=4'b1010;
s=2'b00;
#20;
$display("Selection is %b %b, Output is %b",s[1],s[0],y);
s=2'b01;
#20;
$display("Selection is %b %b, Output is %b",s[1],s[0],y);
s=2'b10;
#20;
$display("Selection is %b %b, Output is %b",s[1],s[0],y);
s=2'b11;
#20;
$display("Selection is %b %b, Output is %b",s[1],s[0],y);
$finish;
end
endmodule
```
## Simulated Output Behavioral Modelling

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/32b70429-a94f-4b63-bfe9-90811a2b289a" />



### 4:1 MUX Structural Implementation

![image](https://github.com/user-attachments/assets/eea81c2c-7dfa-43aa-9cea-1ab4ed54db6c)


```verilog
`timescale 1ns / 1ps
module mux_2_1(input a, input b, input s, output y);
  assign y =(~s & a)|(s & b);
endmodule

module mux_4_1(i, s, y);
  input [3:0] i;    
  input [1:0] s; 
  output y;
  wire w1, w2;       
  mux_2_1 m1(i[0], i[1],s[0], w1);
  mux_2_1 m2(i[2], i[3],s[0], w2);
  mux_2_1 m3(w1,w2, s[1], y);
endmodule
```
### Testbench Implementation
```verilog
module mux_4_1_tb;
reg [3:0]i;
reg [1:0]s;
wire y;
mux_4_1 uut(i,s,y);
initial
begin
i=4'b1010;
s=2'b00;
#20;
$display("Selection is %b %b, Output is %b",s[1],s[0],y);
s=2'b01;
#20;
$display("Selection is %b %b, Output is %b",s[1],s[0],y);
s=2'b10;
#20;
$display("Selection is %b %b, Output is %b",s[1],s[0],y);
s=2'b11;
#20;
$display("Selection is %b %b, Output is %b",s[1],s[0],y);
$finish;
end
endmodule
```
## Simulated Output Structural Modelling

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/831a05eb-1b5e-485f-a646-452bce43dea3" />


---
### CONCLUSION

In this experiment, a 4:1 Multiplexer was successfully designed and simulated using Verilog HDL across four different modeling styles: Gate-Level, Data Flow, Behavioral, and Structural.The simulation results verified the correct functionality of the MUX, with all implementations producing identical outputs for the given input conditions.

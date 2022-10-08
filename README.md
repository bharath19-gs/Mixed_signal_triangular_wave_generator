
# Mixed Signal Circuit Design and Simulation Marathon

# Mixed_signal_triangular_wave_generator

- [Abstract](#abstract)
- [Details](#details)
- [Reference Circuit Block Diagram](#reference-circuit-block-diagram)
- [Reference Waveform](#reference-waveform)
- [Circuit Details](#circuit-details)
- [Software Used](#software-used)
  * [eSim](#esim)
  * [NgSpice](#ngspice)
  * [Makerchip](#makerchip)
- [Circuit Diagram in eSim](#circuit-diagram-in-esim)
- [Verilog Code](#verilog-code)
- [Makerchip](#makerchip-1)
- [Makerchip Plots](#makerchip-plots)
- [Netlist](#netlist)
- [NgSpice Plots](#ngspice-plots)
- [Steps to run generate NgVeri Model](#steps-to-run-generate-ngveri-model)
- [Steps to run this project](#steps-to-run-this-project)
- [Acknowlegdements](#acknowlegdements)
- [References](#references)
- [Author](#author)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>


## Abstract
 Triangular waves, An audio waveform comprised of an infinite set of odd harmonic sine waves, It is often used in sound synthesis where its tone quality is less harsh/noisy than the square waves because the amplitude of its upper harmonics falls off more rapidly.
now these waves will be needed in alot of places, we might have to for different frequencies, but we can't use different clock generators all the time, so we can use a baud generator/ square wave generator for different frequencies(tunable) using select lines.
 


## Details 
 [Triangular Wave generator](https://www.eeeguide.com/triangular-wave-generator-using-op-amp/),  triangular wave is generated by alternatively charging and discharging a capacitor with a constant current. This is achieved by connecting integrator circuit at the output of square wave generator.
 It consists of a sqaure wave generator and an integrator . The output of the square wave of amplitude ± Vsat and is applied to the inverting (-) input terminal of the integrator. The output of integrator is a triangular wave.

## Reference Circuit Block Diagram
![image](https://github.com/bharath19-gs/Mixed_signal_triangular_wave_generator/blob/main/BlockDiagram.png)
## Reference Waveform
![image](https://github.com/bharath19-gs/Mixed_signal_triangular_wave_generator/blob/main/Adobe%20Scan%20Sep%2027%2C%202022_2%20(1).jpg)

## Circuit Details
As shown in the figure we have 2 main blocks
1. a baud rate generator for manipulating the frequency/ pulsating square wave.</br>
Firstly, we will use a baud rate generator for getting a square for the required frequency from a fixed clock(XTAL or crystal oscillator). We can see the basic baud rate generator in figure 2, we will be using the standard baud rate values i.e., 2400, 9600, 115200, etc.

2. a integrator using a 2 stage opamp</br>
 Integrator is made of 3 main components a resistor, a capacitor, and an  op-amp(SKY130nm transistors). we can observe this in Figure 2.   
The opamp design has 5 PMOS transistors and 3 NMOS transistors.
</br>



## Software Used
### eSim
It is an Open Source EDA developed by FOSSEE, IIT Bombay. It is used for electronic circuit simulation. It is made by the combination of two software namely NgSpice and KiCAD.
</br>
For more details refer:
</br>
https://esim.fossee.in/home
### NgSpice
It is an Open Source Software for Spice Simulations. For more details refer:
</br>
http://ngspice.sourceforge.net/docs.html
### Makerchip
It is an Online Web Browser IDE for Verilog/System-verilog/TL-Verilog Simulation. Refer
</br> https://www.makerchip.com/


## Circuit Diagram in eSim
The following is the schematic in eSim:
![image](https://github.com/bharath19-gs/Mixed_signal_triangular_wave_generator/blob/main/circuit.png)
## Verilog Code
![image](https://github.com/bharath19-gs/Mixed_signal_triangular_wave_generator/blob/main/verilog%2Bcode.jpg)
## Makerchip
```
\TLV_version 1d: tl-x.org
\SV
/* verilator lint_off UNUSED*/  /* verilator lint_off DECLFILENAME*/  /* verilator lint_off BLKSEQ*/  /* verilator lint_off WIDTH*/  /* verilator lint_off SELRANGE*/  /* verilator lint_off PINCONNECTEMPTY*/  /* verilator lint_off DEFPARAM*/  /* verilator lint_off IMPLICIT*/  /* verilator lint_off COMBDLY*/  /* verilator lint_off SYNCASYNCNET*/  /* verilator lint_off UNOPTFLAT */  /* verilator lint_off UNSIGNED*/  /* verilator lint_off CASEINCOMPLETE*/  /* verilator lint_off UNDRIVEN*/  /* verilator lint_off VARHIDDEN*/  /* verilator lint_off CASEX*/  /* verilator lint_off CASEOVERLAP*/  /* verilator lint_off PINMISSING*/  /* verilator lint_off BLKANDNBLK*/  /* verilator lint_off MULTIDRIVEN*/   /* verilator lint_off WIDTHCONCAT*/  /* verilator lint_off ASSIGNDLY*/  /* verilator lint_off MODDUP*/  /* verilator lint_off STMTDLY*/  /* verilator lint_off LITENDIAN*/  /* verilator lint_off INITIALDLY*/  

//Your Verilog/System Verilog Code Starts Here:
module baud(out,clk,reset,select_line);
input clk, reset;
input [1:0] select_line;
output reg out;
reg[11:0]modulus=12'd0;
reg[11:0]count=12'd0;

always@(select_line)
begin
case(select_line)
2'b00:modulus=12'd1303;
2'b01:modulus=12'd325;
2'b10:modulus=12'd162;
2'b11:modulus=12'd64;
default:modulus=12'd325; //9600 baud
endcase
end

always@(posedge clk,negedge reset)
begin

if (!reset)
begin
out <= 1'b0;
end
else if(count==modulus)
begin
count<=1'b0;
out<=~out;
end
else
begin
count<=count+1'b1;
out<=out;
end
end

endmodule


//Top Module Code Starts here:
	module top(input logic clk, input logic reset, input logic [31:0] cyc_cnt, output logic passed, output logic failed);
		logic  [1:0] select_line;//input
		logic  out;//output
//The $random() can be replaced if user wants to assign values
		assign select_line = $random();
		baud baud(.clk(clk), .reset(1), .select_line(select_line), .out(out));
	
\TLV
//Add \TLV here if desired                                     
\SV
endmodule



```
## Makerchip Plots
we can observe the baud rate at 9600
![image](https://github.com/bharath19-gs/Mixed_signal_triangular_wave_generator/blob/main/image1_waveform.jpg)
![image_2](https://github.com/bharath19-gs/Mixed_signal_triangular_wave_generator/blob/main/waveform_2.jpg)

## Netlist
![image](https://github.com/bharath19-gs/Mixed_signal_triangular_wave_generator/blob/main/netlist.png)

## NgSpice Plots
this is the input from the constant pulse generator</br>
![image](https://github.com/bharath19-gs/Mixed_signal_triangular_wave_generator/blob/main/V_inp.png)
</br>
this is the output from the baud generator(set at 3 as observed in the code 9600) </br>
![image](https://github.com/bharath19-gs/Mixed_signal_triangular_wave_generator/blob/main/baud_op.png)
</br>
we can see the baud being instantiated in the ngspice window(the digital block)</br>
![image](https://github.com/bharath19-gs/Mixed_signal_triangular_wave_generator/blob/main/data.png)

we can observe a trinagular wave being generated, by manipulating the feedback capacitor we get a perfect output</br>
![image](https://github.com/bharath19-gs/Mixed_signal_triangular_wave_generator/blob/main/traing_op.png)

## Steps to run generate NgVeri Model
1. Open eSim
2. Run NgVeri-Makerchip 
3. Add top level verilog file in Makerchip Tab
4. Click on NgVeri tab
5. Add dependency files
6. Click on Run Verilog to NgSpice Converter
7. Debug if any errors
8. Model created successfully
## Steps to run this project
1. Open a new terminal
2. Clone this project using the following command:</br>
```git clone https://github.com/bharath19-gs/Mixed_signal_triangular_wave_generator/ ```</br>
3. Change directory:</br>
```cd Mixed_signal_triangular_wave_generator ```</br>
4. Run ngspice:</br>
```ngspice opamp.cir```</br>
5. To run the project in eSim:

  - Run eSim</br>
  - Load the project</br>
  - Open eeSchema</br>
  - Run the Simulation tool icon after KiCad to ngspice 
  
## Acknowlegdements
1. [FOSSEE, IIT Bombay](http://iitb.ac.in/)
2. [Google-skywater](https://www.google.co.in/)
3. https://spoken-tutorial.org/
4. Steve Hoover, Founder, Redwood EDA
5. Kunal Ghosh, Co-founder, [VSD Corp. Pvt. Ltd.](https://www.vlsisystemdesign.com/) - kunalpghosh@gmail.com
6. Sumanto Kar, eSim Team, FOSSEE
7. [C2S](https://www.c2s.gov.in/)

## References
1. eeeguide.com/triangular-wave-generator-using-op-amp/
2. Halwai, Shubham & Arulmurugan, R.. (2021). Triangular Wave Generator using Integrator and Square Waveform Generator. 
3. Yuan, T. and Fan, Q., 2020. Design Of Two Stage CMOS Operational Amplifier in 180nm Technology. arXiv preprint arXiv:2012.15737.
4. Suman, S., 2019. Two Stage CMOS Operational Amplifier: Analysis and Design. Social Science Research Network, Rochester, NY, SSRN Scholarly Paper ID 3433181.
5. https://www.geeksforgeeks.org/baud-rate-and-its-importance/
6. https://www.sfu.ca/sonic-studio-webdav/handbook/Triangle_Wave.html
7. https://www.tutorialspoint.com/linear_integrated_circuits_applications
8. https://github.com/Eyantra698Sumanto/Two-in-One-Low-power-XOR-XNOR-Gate.git


## Author 

Bharath G S, bharathgs.bgs@gmail.com

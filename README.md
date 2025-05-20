# 4Bit-Up-Down-Asynchronous-Reset-Counter-Synthesis

## Aim:

Synthesize 4Bit-Up-Down-Asynchronous-Reset-Counter design using Constraints and analyse reports, Timing, area and Power.

## Tool Required:

Functional Simulation: Incisive Simulator (ncvlog, ncelab, ncsim)

Synthesis: Genus

### Step 1: Getting Started

Synthesis requires three files as follows,

◦ Liberty Files (.lib)
![image](https://github.com/user-attachments/assets/a3dfc279-08cf-491a-9b6c-fd68320d962b)

◦ Verilog/VHDL Files (.v or .vhdl or .vhd)

```
reg clk;
reg reset;
reg up_down;
reg enable;
wire [3:0] count;


up_down_counter uut(
    .clk(clk),
    .reset(reset),
    .up_down(up_down),
    .enable(enable),
    .count(count)
);


initial begin
    clk = 0;
    forever #5 clk = ~clk;  
end


initial begin

    reset = 1;
    up_down = 1;
    enable = 0;
    

    #20 reset = 0;
    

    #10 enable = 1;
    #200;  
    

    up_down = 0;
    #200;  
    

    enable = 0;
    #50;
    

    reset = 1;
    #20 reset = 0;
    

    #50;
    

    $finish;
end


initial begin
    $monitor("Time: %t, Reset: %b, Up/Down: %b, Enable: %b, Count: %b", 
             $time, reset, up_down, enable, count);
end
```

##Design
```always @(posedge clk or posedge reset) begin
    if (reset) 
    begin
        count <= 4'b0000;
    end

    else if (enable)
    begin
        if (up_down)
        begin
            count <= count + 1'b1;
        end
        else begin
            count <= count - 1'b1;
        end
    end
end
```
◦ SDC (Synopsis Design Constraint) File (.sdc)
![image](https://github.com/user-attachments/assets/035a8c83-4d9a-44ec-932a-264d5f340dfe)

 ### Step 2 : Creating an SDC File

•	In your terminal type “gedit input_constraints.sdc” to create an SDC File if you do not have one.
![Screenshot 2025-05-20 140652](https://github.com/user-attachments/assets/02a49424-6ed2-4d0e-ada6-bee052c87532)


•	The SDC File must contain the following commands;

create_clock -name clk -period 2 -waveform {0 1} [get_ports "clk"]

set_clock_transition -rise 0.1 [get_clocks "clk"]

set_clock_transition -fall 0.1 [get_clocks "clk"]

set_clock_uncertainty 0.01 [get_ports "clk"]

set_input_delay -max 0.8 [get_ports "rst"] -clock [get_clocks "clk"]

set_output_delay -max 0.8 [get_ports "count"] -clock [get_clocks "clk"]

i→ Creates a Clock named “clk” with Time Period 2ns and On Time from t=0 to t=1.

ii, iii → Sets Clock Rise and Fall time to 100ps.

iv → Sets Clock Uncertainty to 10ps.

v, vi → Sets the maximum limit for I/O port delay to 1ps.

### Step 3 : Performing Synthesis

The Liberty files are present in the library path,

• The Available technology nodes are 180nm ,90nm and 45nm.

• In the terminal, initialise the tools with the following commands if a new terminal is being
used.

◦ csh

◦ source /cadence/install/cshrc

• The tool used for Synthesis is “Genus”. Hence, type “genus -gui” to open the tool.
![Screenshot 2025-05-09 114342](https://github.com/user-attachments/assets/401bb372-1dc4-4f1b-ad87-785ec9fd78af)
![Screenshot 2025-05-20 141238](https://github.com/user-attachments/assets/d600144c-8438-40be-9545-1c06068927ff)


• Genus Script file with .tcl file Extension commands are executed one by one to synthesize the netlist.

#### Synthesis RTL Schematic :


#### Area report:
![Screenshot 2025-05-20 141626](https://github.com/user-attachments/assets/384682e2-2a9c-4c60-839f-75a7919bbc7c)



#### Power Report:
![Screenshot 2025-05-20 141729](https://github.com/user-attachments/assets/698344b5-e326-442d-b4e5-c587e83080bb)



#### Timing Report: 
![santo timing](https://github.com/user-attachments/assets/03fd8c1b-285e-4fa8-befb-5b43c042c625)



#### Result: 

The generic netlist has been created, and area, power, and timing reports have been tabulated and generated using Genus.
![image](https://github.com/user-attachments/assets/18873b94-a5a0-4b14-97f2-7ee419a1c2a4)








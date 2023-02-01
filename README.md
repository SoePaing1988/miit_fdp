# miit_fdp

# Day 1 : Introduction to Verilog RTL design and Synthesis 
## Introduction to open-source simulator iverilog

RTL : Register-transfer level (RTL) is a representation at the abstraction level that expresses a synchronous digital circuit in terms of the flow of digital signals (data) between hardware registers along with the logical operations performed on those signals.Register-transfer-level abstraction is used in hardware description languages (HDLs) like Verilog and VHDL to create high-level representations of a circuit, from where we can derive lower-level representations. Simulator : It is a tool for checking whether our RTL design meets the required specifications or not. Icarus Verilog is a simulator used for simulation and synthesis of RTL designs written in verilog which is one of the many hardware description languages.
Design : It is the code or set of verilog codes that has the intended functionality to meet the required specifications.
Testbench : Testbench is a setup which is used to apply stimulus (test_vectors) to the design to check it's functionality.It tests whether the design provides the output that matches the specifications.The RTL Design gets instantiated in the testbench.

![image](https://user-images.githubusercontent.com/123365615/214894163-9bbe8ed4-35af-40a5-b431-e2a49a9379bd.png)

### Iverilog Based Design Flow :
1. The iverilog simulator takes RTL design and Testbench as inputs.
2. It produces a VCD file(Value change dump format) as output. Only changes in the input are dumped to changes in the output.
3. We use Gtkwave to see these output changes graphically.

### Labs using iverilog and gtkwave

We install the opensource software Virtual box for ruuning the Linux Ubuntu without actually installing it. Then we can download any version of Linux comfortable to us. Once done,We start with the following steps for our environment seup in our virtual terminal.

        mkdir vsd  
        cd vsd  
        git clone https://github.com/kunalg123/vsdflow.git  
        mkdir vlsi  
        cd vlsi  
        git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git  
        cd SKY130RTLDesignAndSynthesisWorkshop  
        cd my_lib  
        cd lib  
        cd ..  
        cd verilog_model  
        cd ..  
        cd ..  
        cd verilog_files
 
Below screenshot shows the above directory structure inside the vsd upto lib directories that was set up through the terminal.

 ![pic1](https://user-images.githubusercontent.com/123365615/214267684-e71c0ed4-e9ca-4c5c-a034-1d92c7c23d7b.PNG)
 
Below screenshot shows the list of verilog files. Each verilog design file has an assosciated test bench file.

 ![pic 2](https://user-images.githubusercontent.com/123365615/214267577-b00347a0-7829-49c1-b9d9-7a3aca0166f2.PNG)

Since the environment is now set up,we try to simulate a verilog code named good_mux in one of our verilog_files with the help of it's test bench and gtkwave. The steps are mentioned below:

 1. We simulate the RTL design and assosciated test bench .
 
         iverilog good_mux.v tb_good_mux.v

![pic 3](https://user-images.githubusercontent.com/123365615/214267764-316bdd57-075f-4634-b25f-0b774102ea65.PNG)

 2. We dump the output into a vcd file.

          ./a.out

 3. The following command invokes gtkwave window where in we can see all our outputs.

          gtkwave tb_good_mux.vcd

![pic 4](https://user-images.githubusercontent.com/123365615/214267848-6f917d3e-4cf7-4280-a218-d406e7cd3bfb.PNG)

In the gtkwave window we drag all the variables and zoom fit to see the transitions for selected variable.

![sp11](https://user-images.githubusercontent.com/123365615/215048158-7fa6fe5e-d82b-4264-8bcf-1e5254d084c8.PNG)

 4. We can also view our Verilog RTL design and testbench code using

          gvim tb_good_mux.v -o good_mux.v

![pic 6](https://user-images.githubusercontent.com/123365615/214267970-0e58d783-ae22-47da-97e1-dd832577626d.PNG)

Add vcode and testbench

## Introduction to Yosys and Logic synthesis
Synthesizer :It is a tool used for the conversion of an RTL to a netlist.
Netlist : It is a representation of the input design to yosys in terms of standard cells present in the library. Yosys is the Synthesizer tool that we will be using. Diiferent levels of abstraction and synthesis

![image](https://user-images.githubusercontent.com/123365615/214898155-ad26dae6-9f4d-4b61-bc00-c2daaf47f9e9.png)

![image](https://user-images.githubusercontent.com/123365615/214898290-5fad2857-fdd5-45bb-983f-9efbbfa402b2.png)

 - read_verilog : It is used to read the design.
 - lib : It is used to read the library .lib.
 - write_verilog : It is used to write out the netlist.
Veifying Synthesis We verify our synthesis to check if any unwanted changes have been made in our RTL design due to above operations.

![image](https://user-images.githubusercontent.com/123365615/214898459-990ea581-6773-41fc-9efa-7dcc2f3a8ea7.png)

The set of primary inputs/outputs will remain same between the RTL design and Synthesized netlist. Hence,same testbench can be used.Netlist and RTL are essentialy same. RTL is written in behavioural form whereas netlist is written in the form of standard cells.
The waveform in the above figure should be same as observed in RTL simulation Design.

### WHAT HAPPENS DURING SYNTHESIS? During Logic Synthesis an RTL code is transformed into a gate level netlist.

 - A synthesizer performs gate level translation.
 - Design is converted into gates and connections are made between the gates.
 - The file what we write out finally is a gate-level netlist.
 
 For example the following code:

        module sample_code(

        input clk, rst,

        output result, done);

        always@(posedge clk, posedge rst)

        if(rst)

        ...

        else

        ...

        endmodule
    
    
![image](https://user-images.githubusercontent.com/123365615/214899112-784d52ca-19b2-4e0a-a70a-f6b06c870af8.png)

is coverted to the following digital circuit .

### Understanding .lib
.lib is the collection of logic modules which includes basic logic gates such as AND, OR, NOT, etc. It also contains different flavours of the same gate.

Like,

      2 - input AND gate
      Versions:
      - slow
      - medium
      - fast

     3 - input AND gate
     Versions:
     - slow
     - medium
     - fast

     4 - input AND gate
     Versions:
     - slow
     - medium
     - fastin
### But why do we need different flavours of gate? Consider the below circuit:

![image](https://user-images.githubusercontent.com/123365615/214899581-9c8bf3a9-3209-4400-a7e3-95b626737a74.png)

### Setup time Analysis
It should take 1 CLK cycle for the signal to propagate from the Launch DFF-A to the Capture DFF-B through the combimational circuit.
While this propagation all delays : Propagation delay of DFF A (Tcq-a)+ Propagation delay of combinational circuit(Tcomb)+Setup time of DFF-B(Tsetup_b). Thus ,the constraint becomes:

        Tclk>Tcq-a+Tcomb+Tsetup_b

Setup time is the time for which data should arrive before the launch clock edge to be reflected at the output reliably.
For high performance we need high speed gates.So, The frequency of a gates should be high or the Tclk should be less . So all the above mentioned delay should be less. We need cells that work fast to make the combinational small.

### Hold up time Analysis
After the after the edge of the clock I don't want data to change . So, the fastest change that can happen at the input of we would be after the hold time window so that I can reliably capture the previous clock edge of launch flop DFF-A to ensure there are no hold issues at DFF-B . Hence ,the constraint we have:

         Thold_b<Tcq-a+Tcomb
 
We need cells to work slowly to ensure that there are no hold time issues at Capture flop DFF-B.

![image](https://user-images.githubusercontent.com/123365615/214899961-adf4d911-bc86-4ef9-ad34-cb53e90918d6.png)

We need cells that work fast to meet the required performance .We need cells that work slow to meet whold time requirements .
This collection of cells forms the .lib

#Comparison of Faster Cells and Slower Cells
The load for a cell in a digital logic circuit usually is taken as a capacitor.
Propagation delay is the time required for the input to be reflected in the output of the cell it depends on the gate driving capacity which is dependent on the capacitance. Faster the charging and discharging of a capacitance, lesser will be the cell delay. In order to charge/discharge the capacitance fast, we need transistors which are capable of sourcing more current which means it is a wide transistor as the current carrying capacity of a transistor is the function of its width. Wider the transistors, lower will be the delay but more is the area and power. Narrow Transistors will have more delay but less area and less power.

So fastercells don't come free. They come at the tradeoffs of area and power.

### How do we select cells?

 - We need to guide the synthesizer to select the flavour of cells for optimum implementation of our logic circuits.
 - More use of faster cells results in bad circuit in terms of area and power and potentially may result in hold time violations.
 - More use of slower cells may result in a sluggist circuit and may not meet the performance. It is required for us to offer guidance to the synthesizer to pick correct set of cells .This guiding parameters to the synthesizer are called as CONSTRAINTS.

## Labs using Yosys and Sky130 PDKs

Commands to obtain a synthesized implementation of good_mux design

1.We need to read verilog files, .lib files and writing out the netlist after invoking yosys. 2.Read the library using the read -liberty command. 3. We read the verilog design file. 4. We generate the netlist 5. We synthesize the rtl code with the top module's name using synth -top command. 6. Use abc -liberty to generate the netlist. 7. Display the synthesis implementation using show.

The commands to synthesise a rtl code called good_mux is as follows:

        yosys

        read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

        read_verilog good_mux.v

        synth -top good_mux

        abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

        show

![pic 7](https://user-images.githubusercontent.com/123365615/214268038-cdb72106-c621-4449-a791-247138b61229.PNG)

Note: ABC is the command that converts our RTL file into a gate .What gate it has linked to, that gate is specified in the library with the path ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib .The logic of good mux will be realised through standard cells present in the library of the mentioned path. The execution of ABC command gives us a report of the number of input and the output signals of our standard cell .

![pic 8](https://user-images.githubusercontent.com/123365615/214268103-8f3664c3-f5f7-4d59-b534-f74146f3ae55.PNG)

It also specifies the type and number of cells in a synthesis of RTL design.

![pic 9](https://user-images.githubusercontent.com/123365615/214268179-d188de38-476c-4408-a722-99d6c517fafb.PNG)

On execution of show command,a synthesized implementation of good_mux design is visible.

![pic 10](https://user-images.githubusercontent.com/123365615/214268357-011b448c-11cb-4a30-88ea-01e791dec386.PNG)

### Commands to write a netlist

        write_verilog -noattr good_mux_netlist.v

        !gvim  good mux_netlist.v

Note: We use the switch -noattr i.e. no attributes for a more simplistic view of our netlist as shown below

![pic 11](https://user-images.githubusercontent.com/123365615/214268277-f7e77afe-d4b8-464c-8867-cbacade01b1d.PNG)



## DAY2 : TIMING LIBS, HIERARCHICAL Vs FLAT SYNTHESIS AND EFFICIENT FLOP CODING STYLES

### INTRODUCTION TO TIMING .lib

We take a walk through the library that is said to have a collection of all the standard cells along with their different flavors. We begin by understanding the name of the library. To look into the library,we use the gvim command

        gvim ../lib/SKY130_fd_sc_hd__tt_025C_1v80.lib

The following window appears that shows the library file SKY130_fd_sc_hd__tt_025C_1v80.lib

![c1](https://user-images.githubusercontent.com/123365615/214469650-4432376c-9e3a-4f40-8df9-e02774e739b7.PNG)

First line is the name of the library where TT stands for typical. For a design to work three parameters of the library are important :

 -  P- Process:Process is the variations due to fabrication
 -  V -voltage: Variations in voltage also impact the behavior of the circuit.
 -  T- temperature :Semiconductors are very sensitive to temperature variations too. All this variations determine the performance of a silicon whether it is fast or slow. Thus, our libraries are characterized to model this variations.
Switching off the syntax color red

        Command used : syn off
  
Enabling the line numbers

        Command used : se nu
  
We can see that the lib file contains the technology and units of all the parameters.

![c2](https://user-images.githubusercontent.com/123365615/214469704-904e2ed5-c157-47fc-b5dc-0265b810741b.PNG)

The voltage process and temperature conditions are also specified.

![c3](https://user-images.githubusercontent.com/123365615/214469737-5271f59a-2394-4cba-9579-f5fc043be768.PNG)

The lib contains different flavors of this same as well as different types of cells.

![c4](https://user-images.githubusercontent.com/123365615/214470489-daab5614-5a75-4e03-8fb7-d2eef07613b5.PNG)

![c5](https://user-images.githubusercontent.com/123365615/214470914-b589af3b-85ee-4182-a344-ffc5f74053b3.PNG)

As we see in the above window,
The library also represents the different features of the cell like its leakage power,the various input's combinations and the operations between them.

The name a21110 signifies that it's And OR gate where in the first two inputs A1 and A2 are And'ed. It's result is OR'ed with the rest of the three inputs B1,C1 and D1. In order to understand the functionality of the cell we can also look into the equivalent verilog model. Each of the input can take a high or a low power level.For 5 inputs we have 32 combinations in total. The behaviour model specifies the delay and power for each of these inputs. Inside the cell block we have different power combinations and their respective leakage power values. It also shows the area. It gives the description of various pin in terms of their capacitance transition , internal power and the delay associated with this pins.

![C6](https://user-images.githubusercontent.com/123365615/214477178-26d9a54f-40cd-4dfa-ab3f-a84694e3722e.PNG)

We pick a small gate small gate for better understanding. We see it's behaviour view.

![C7](https://user-images.githubusercontent.com/123365615/214477409-7a126f53-84ae-4b33-b98c-a9fde7ef3b39.PNG)

We can see in the GVIM window above that there are two input for And gate, and thus four possible combinations the leakage power and the logic levels of which are specified. We now perform the comparison between the and gates.

![C8](https://user-images.githubusercontent.com/123365615/214478703-e0499855-643e-46f0-a511-03005fbc64fb.PNG)

On comparison we see that the and gate "and2_4" has more area as compared to the and gate "and2_2" which in turn has more area with the and gate "and2_0". It is thus evident that and2_4 employs wider transistors. These are the different flavours of the same and gate. And and2_4 being the widest also has large leakage power values as well as large area. But it will have small delay values as it is faster .

### HIERARCHIAL VS FLAT SYNTHESIS

While syntheisizing the RTL design in which multiple modules are present, the synthesis can be done in two forms. We understand it through the following example:

                vim multiple_modules.v

![c9](https://user-images.githubusercontent.com/123365615/214479914-b901e696-26eb-447e-8e93-44cff5c7422c.PNG)

                yosys

                read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

                read_verilog multiple_modules.v

                synth -top multiple_modules

![c10](https://user-images.githubusercontent.com/123365615/214482340-20b879ab-d13f-4f9d-8d4f-32a8bd3484c9.PNG)

![c11](https://user-images.githubusercontent.com/123365615/214482371-a2b8f37c-9294-4693-88ca-bf2c548ee748.PNG)

The report has inferred submodule1 having one AND gate ,submodule2 to having one OR gate and multiple module having two cells . Now we link this design to the library using abc command. To show the graphical version ,we use the command.

                show multiple_modules

![c12](https://user-images.githubusercontent.com/123365615/214482920-55f2dfaa-c060-4642-b608-3375f217ebbb.PNG)

Instead of or and and gates it shows the instances u1 and u2 while preserving the hierarchy. This is called the hierarchical design.

Instead of or, and the circuit is implemented using nand and inverter gates. We always prefer stacked NMOS's(nand gates)to stacked the PMOS's(nor cascaded with inverter for or). Because pmos has a very poor mobility and therefore they have to be made quite wide to obtain a good logical effort .
We use flatten to generate a flat netlist. Here there are no instances of U1 and U2 and hierarchy is not present.

                        write_verilog multiple_modules.hier.v

                        abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 

                        !vim multiple_modules.hier.v 

                        flatten

                        show

![c13](https://user-images.githubusercontent.com/123365615/214483629-bab5ae03-a318-4c14-a046-d617db135419.PNG)

Even in the design view using show command we see that it simply displays the structure completely without any hierarchy.

![c14](https://user-images.githubusercontent.com/123365615/214485987-9eeadce9-f389-4ae2-b49e-79e9b2ae08b6.PNG)

### SUB MODULE LEVEL SYNTHESIS AND ITS NECESSITY

Need for Sub-module synthesis

Module level synthesis is preferred when we have multiple instances of the same module.
Let's assume a top module having multiple instances of the same unit gates(say, a multiplier) .Rather than synthesizing multiplier multiple times as mult1,mult2,mult3, It's better to synthesise it once and replicate it multiple Times.

Divide and conquer approach
Let's assume our RTL design is very very massive and we are giving it to a tool which is not doing a good job. Instead of giving one massive design to the tool, we give portions by portions to the tool so that it provides an optimised netlist and we can stitch all these net lists at at the top level.
Hence we control the model that we are synthesizing using the keywords

        synth-top "sub-module's name"

In the following example, I am going to synthesize at sub_module 1 level although I have read the RTL file at higher module level multi_modules.v

        read_iverilog multiple_modules.v

        synth -top sub_module1

![c15](https://user-images.githubusercontent.com/123365615/214489124-ee75cd60-3435-4acf-8634-631ffbf1580d.PNG)

Notice ,In the synthesis report,it inferring only 1 AND gate.

### GLITCHES

Glitches are the unwanted or unexpected transactions that occur due to propagation delays in digital circuits. Glitches occur when an input signal changes state ,provided the signal takes two or more paths for circuit and both paths have unequal delays. The higher delay on one of the parts can cause a glitch when the single pass arrive at the output gate.

![image](https://user-images.githubusercontent.com/123365615/214907383-4d56010c-b0a9-48f0-8995-622e0ce99921.png)

### Why Flops?

More the combinational circuits more glitchy is the output .We therfore need an element to store the value of the output and that element is called FLOP(storage element).Flop provides resistance to glitches as they transition only at the clock edges .Even though the input of the flop is glitching ,the output will be stable. This avoids glitch propagation in further combinational circuits .
Also,Initialising the flop is required else the combination circuit will evaluate it to a garbage value. To initialise the flop we have reset and set pins. These two pins can be either synchronous or asynchronous.

### Asynchoronous and Synchronous resets

Asynchronous reset: this reset signal does not wait for a clock .The moment asynchronous reset signal is received output queue becomes 0 irrespective of the clock.

Asynchronous set: this set signal does not wait for a clock. The moment asynchronous reset is signal received output queue becomes 1 irrespective of the clock.
Verilog codes of asynchronous reset and set :

![c16](https://user-images.githubusercontent.com/123365615/214490495-78aa07a6-0804-4d94-bd80-a871ce710f0f.PNG)

### GTKWAVE RTL Simulation and Observations :

                read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

                !vim dff_asyncres.v -o dff_async_set.v 

                ./a.out

                gtkwave tb_dff_asyncres.vcd

![c17](https://user-images.githubusercontent.com/123365615/214492166-21a1245d-ed4d-44aa-85b2-1123a838ef60.PNG)

        Q follows d only at the posedge of the clock.
        But as and when async_rest=1,Q becomes 0 without waiting for the next edge of the clock.

![c18](https://user-images.githubusercontent.com/123365615/214492195-5b473fb1-0537-4f73-85b9-fbac4c20e179.PNG)

        But when async_reset goes low(1 to 0),Q doesn't become 1 immediately ,it waits for the next clock edge to follow D.
        Even if asunc_reset=1 and D=1, Q=0 as reset takes high precedence(that is how the code has been written,if condition of reset is checked first).

### Synthesis implementation results :

### asynchoronous reset :

![image](https://user-images.githubusercontent.com/123365615/214512361-aca28e93-0901-4a53-8923-72d01c9c63f4.png)

### asynchoronous set :

![image](https://user-images.githubusercontent.com/123365615/214512486-1eaf8094-db86-4215-aa98-5ba78692cb45.png)

### Synchronous Reset and set : 
On application of set or reset signals,the signal does not immediately goes high or low,but it waits for the next edge of the clock cycle. RTL code of synchronous reset:

![C19](https://user-images.githubusercontent.com/123365615/214514170-8116a50d-af67-4c75-a2e6-2f8a61de10f7.PNG)

Note: The loop is entered only at positive edge of the clock.Upon the posedge of the clock I look for the presence of synchronous reset. If present,Q goes low else D is reflected to output Q.

### Synthesis Results:

![image](https://user-images.githubusercontent.com/123365615/214513823-68d5f342-cab2-4359-af3a-3105949d128a.png)

Case wherein both both asynchronous and synchronous resets are applied together :

### RTL CODE:

![image](https://user-images.githubusercontent.com/123365615/214513880-21584d67-18dd-4fe7-9432-59570fa9964a.png)

### Synthesis Results:

![image](https://user-images.githubusercontent.com/123365615/214513923-b23ae7a0-2305-4264-9802-ef76258ba4e5.png)

## OPTIMISATIONS

We now observe some Interesting Optimisations For Special Cases :

### Case 1:

Let's Consider the following design where the 3 bit input is multiplied by 2 and the output is a 4 bit value.

                module mul2 (input [2:0] a , output [3:0] y);
                        assign y = a* 2;
                endmodule

Looking at it's truth table :

![211](https://user-images.githubusercontent.com/123365615/214910757-11c1794d-4e74-4974-825f-b9501cc19f0e.png)
       
Observation : The output y[3:0] is the input a[2:0] appended with a 0 at the LSB. or, we can say that y = aX2 = {a,0} .

On synthesizing the netlist and look at its graphical realisation , we will see the same optimisation occuring in the netlist.There is no hardware required fot it.

![C20](https://user-images.githubusercontent.com/123365615/214514537-bfd5be37-c759-42c5-aaed-19ca24f8e568.PNG)

![image](https://user-images.githubusercontent.com/123365615/214514618-e804157a-58e5-45c3-9bba-f459f136847e.png)

### Case 2:

Let's consider the following design where the 3 bit input is multiplied by 9 and the output is a 6 bit value.

                module mult8 (input [2:0] a , output [5:0] y);
                        assign y = a* 9;
                endmodule
Looking at it's truth table :

![212](https://user-images.githubusercontent.com/123365615/214910830-8a3ec2d7-4a94-42fd-a3a9-39f9676e330a.png)
       
Observation : The output y[5:0] is equal to the input a[2:0] appended with itself i.e.

y = aX9 = aX(8+1)= aX8+aX1 = {a,0}+a 
y = {a,a}
On synthesizing the netlist and look at it's graphical realisation , we will see the same optimisation occuring in the netlist.

![image](https://user-images.githubusercontent.com/123365615/214515256-1f9783c8-9215-4a03-b7d3-e675c239143d.png)


# DAY 3 : Combinational and Sequential Optimisations

## Introduction to Logic optimisations

Inorder to produce a digital circuit design which is optimised interms of area and power, the simulator performs many types of optimisations on the combinational and sequential circuits.

1.      Combinational optimisation methods:

        Squezzing the logic to get the most optimised design
                Area and Power savings
        Constant propogation
                Direct Optimisation
        Boolean Logic Optimisation
                K-map
                Quine-mckluskey Algorithm
2.      Sequential optimisation methods:

        Basic
                Sequential constant Propogation
        Advanced
                State Optimisation
                        Retiming
                        Sequential Logic Cloning (Floor Plan Aware Synthesis)
                        
## Combinational Logic Optimisations

We will try to understand each of the above mentioned combinational optimisations through different RTL code examples. For each example, We also check the synthesis implementation through yosys to understand how the optimisations take place.
All the optimisation examples are in files opt_check.v, opt_check4.v, and multiple_modules_opt.v. All of these files are present under the verilog_files directory.

### Example 1:

                vim opt_check.v

![a0](https://user-images.githubusercontent.com/123365615/214607368-b60555a2-dedb-416a-8429-7945c2572304.PNG)Ideally ,the above ternary operator should give us a mux. 

But the constant 0 propagates further in the logic .Using boolean simplification we obtain y = ab.

Synthesizing this in yosys :

Before realising the netlist, we must issue a command to yosys to perform optimisations. It removes all unused cells and wires to prduce optimised digital circuit.This can be done using the opt_clean -purge command as shown below.

                read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

                read_verilog opt_check.v

                synth -top opt_check

                opt_clean -purge

![a1](https://user-images.githubusercontent.com/123365615/214541758-cf805f8b-fc9a-4f52-a4f0-d6f0024dd911.PNG)

Observation : After executing synth -top opt_check ,we see in the report that 1 AND gate gas been inferred.

Next,

                abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

                write_verilog -noattr opt_check_netlist.v 

                show

On viewing the graphical synthesis realisation , we can see the Yosys has synthesized an AND gate as expected.

![a2](https://user-images.githubusercontent.com/123365615/214564585-9bf2d5b2-ea87-4298-b1fd-bd042c96e0b9.PNG)

### Example 2: opt_check2.v

                vim opt_check2.v

![a01](https://user-images.githubusercontent.com/123365615/214608542-002a42df-4813-4c57-baf3-ec58c0c77139.PNG)

                read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

                read_verilog opt_check2.v

                synth -top opt_check2

                opt_clean -purge

![a11](https://user-images.githubusercontent.com/123365615/214610463-a9f69d79-d03f-4411-8078-47b8ad27ca7a.PNG)

                abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

                write_verilog -noattr opt_check2_netlist.v 

                show

After simplification,we expect the output y to be an OR gate, since the output of the mux can be simplified to y = a + b. If we generate the netlist and look at its graphical representation , we get

![a3](https://user-images.githubusercontent.com/123365615/214611377-d77f5fb9-d027-4d36-a896-bc55314809ec.PNG)

Note: The synthesis tool instead of OR gates infers a nand gate with inverted inputs based on Demorgan's Law. It is done to avoid stacked PMOS in CMOS implemantation of OR gate.

### Example 3:opt_check3.v

                vim opt_check3.v

![a02](https://user-images.githubusercontent.com/123365615/214609125-790d24a7-931c-42b9-9407-0544ac923d65.PNG)

                read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

                read_verilog opt_check3.v

                synth -top opt_check3

                opt_clean -purge

                abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

                write_verilog -noattr opt_check3_netlist.v 

                show

For the RTL verilog code of opt_check3.v , we expect the output to be a 3 input AND gate based on constant propagation and boolean logic optimisation.The output y can be simplified to y = abc.
Next we generate the netlist and observe its graphical representation after synthesis

![a4](https://user-images.githubusercontent.com/123365615/214565792-aa70ae43-0b33-433f-86c8-fa69fde12daf.PNG)

Yosys synthesizes a 3 input AND gate as expected because of optimisations.


### Example 4: opt_check4.v

                vim opt_check4.v

![a03](https://user-images.githubusercontent.com/123365615/214609606-671bd73b-16e9-46a9-8676-f9da3055ecac.PNG)

                read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

                read_verilog opt_check4.v

                synth -top opt_check4

                opt_clean -purge

![ab1](https://user-images.githubusercontent.com/123365615/214617000-bb47def6-9c73-4d6f-a7f1-1c7b116f3e13.PNG)

                abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

                write_verilog -noattr opt_check4_netlist.v 

                show

In this case,the boolean logic optimisation simplifies the output to a single xnor gate i.e. y = a xnor c. Next we generate the netlist and observe its graphical representation after synthesis

![a5](https://user-images.githubusercontent.com/123365615/214567264-9ddad9b2-c009-4f04-8f6c-4205224b445e.PNG)

Yosys synthesizes a 3 input XNOR gate as expected because of optimisations.

### Example 5: multiple_module_opt.v

                vim multiple_module_opt.v

![ab11](https://user-images.githubusercontent.com/123365615/214618053-7b926404-2d52-4f5b-a3b7-529c2449460b.PNG)

                read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

                read_verilog multiple_module_opt.v

                synth -top multiple_module_opt

                opt_clean -purge

                flatten

![ab7](https://user-images.githubusercontent.com/123365615/214619198-0738a4ba-be37-4a33-adb5-f9b78215597e.PNG)

                abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

                write_verilog -noattr multiple_module_opt_netlist.v 

                show

While synthesizing this in yosys we use flatten before opt_clean -purge. The multiple_module_opt instantiates both submodule1 and 2. We must use Flat Synthesis here otherwise the optimisations will not be performed on the sub module level.

![a8](https://user-images.githubusercontent.com/123365615/214621734-42f4972e-7973-4e9b-9758-0c03ba0fac98.PNG)

### Example 6: multiple_module_opt2.v

                vim multiple_module_opt2.v

![a9](https://user-images.githubusercontent.com/123365615/214622511-3ad7712c-d085-4e8f-8761-18b84a226573.PNG)

                read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

                read_verilog multiple_module_opt2.v

                synth -top multiple_module_opt2

                opt_clean -purge

                flatten

                abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

                write_verilog -noattr multiple_module_opt2_netlist.v 

                show

On boolean optimisation, we obtain y=1 simply. It's synthesis yields:

![a9](https://user-images.githubusercontent.com/123365615/214624095-99f4d7e2-69c4-4a00-bfbf-d96a6f108d71.PNG)


## Sequential Logic Optimisations

We will try to understand each of the sequential optimisations through different RTL code examples. For each example, We also check the synthesis implementation through yosys to understand how the optimisations take place.
All the optimisation examples are in files dff_const2.v,dff_const3.v,dffconst4.v and dff_const5.v. All of these files are under the verilog_files directory.

### Example 1: dff_const1.v

                vim dff_const1.v

![s1](https://user-images.githubusercontent.com/123365615/214625720-3c82b6fb-b609-4e95-ab58-4538d48f9a1e.PNG)

                iverilog dff_const1.v tb_dff_const1.v

                ./a.out

                gtkwave tb_dff_const1.vcd

Here, it appears that the output Q should be equal to an inverted reset or Q=!reset. However, as the reset is synchronous,even if the flop has D pinned to logic 1,when reset becomes 0, Q does not immediately goto 1. It waits untill the positive edge of the next clock cycle.
This is observed by simulating the design in verilog, and viewing the VCD with GTKWave as follows

![s5](https://user-images.githubusercontent.com/123365615/214633091-69cc4cbe-a229-44ba-aebb-3f4e5ce4acaa.PNG)

Observation : In the gtk waveform above , when reset becomes 0, Q becomes 1 at the next clock edge. Since Q can be either 1 or 0,we do not get a sequential constant, and no optimisations should be possible here. We verify it using Yosys synthesis and optimisation.
While synthesis,We use

                dfflibmao -liberty ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib

dfflibmap is a switch that tells the synthesizer about the library to pick sequential circuits( mainly Dff's and latches) from.

We then generate the netlist

                abc -liberty ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib 

                write_verilog -noattr dff_const1_netlist.v 
                
                show

                        read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

                        read_verilog dff_const1.v

                        synth -top dff_const1

                        dfflibmap -liberty ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib

                        abc -liberty ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib 

                        write_verilog -noattr dff_const1_netlist.v 

                        show

![s6](https://user-images.githubusercontent.com/123365615/214635875-20af3330-ae11-47c5-a9da-fc107468cd35.PNG)

As expected, No optimisation is performed in th yosys implementation during synthesis.

### Example 2: dff_const2.v

                vim dff_const2.v

![s2](https://user-images.githubusercontent.com/123365615/214628722-4ab54c77-6bd8-4180-99c9-6def16c0a917.PNG)

                iverilog dff_const2.v tb_dff_const2.v

                ./a.out

                gtkwave tb_dff_const2.vcd

Here, Regardless of the inputs, the output q always remains constant at 1 .
This is observed by simulating the design in verilog, and viewing the VCD with GTKWave as follows

![s8](https://user-images.githubusercontent.com/123365615/214639650-b0aed62f-e315-42f1-90ee-c8529d96c710.PNG)

                read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

                read_verilog dff_const2.v

                synth -top dff_const2

                dfflibmap -liberty ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib

                abc -liberty ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib 

                write_verilog -noattr dff_const2_netlist.v 

                show

Since the output is always constant ie Q=1, it can easily be optimised during synthesis.

![s7](https://user-images.githubusercontent.com/123365615/214638439-e8913f4c-e388-47a2-a417-29dc11a71b9c.PNG)

### Example 3: dff_const3.v

                vim dff_const3.v

![s3](https://user-images.githubusercontent.com/123365615/214628873-2fe56a6e-4035-4c4e-8475-c4df4c294c5d.PNG)

                iverilog dff_const3.v tb_dff_const3.v

                ./a.out

                gtkwave tb_dff_const3.vcd

When reset goes from 1 to 0,Q1 follows D at the next positive clock edge in an ideal ckt. But in reality, Q1 becomes 1 a little after the next positive clk edge(once reset has been made 0)due to Clock-to-Q delay.
Thus, q takes the value 0 until the next clock edge when it read an input of 1 from q1. This is confirmed with the simulated waveform below.

![s9](https://user-images.githubusercontent.com/123365615/214641311-237fa492-dc26-4f3d-81cf-bf8a8d5d5d98.PNG)

                read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

                read_verilog dff_const3.v

                synth -top dff_const3

                dfflibmap -liberty ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib

                abc -liberty ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib 

                write_verilog -noattr dff_const3_netlist.v 

                show

Since Q takes both logic 0 and 1 values in different clock cycles. It is wrong to say that Q=!(reset) or Q=Q1
Hence, both the flip-flops are retained and no optimisations are performed on this design. We can confirm this using Yosys as shown below.

![s11](https://user-images.githubusercontent.com/123365615/214643434-aa3f2864-499b-4b4b-81e9-07f0e17613db.PNG)

Both the D flip-flops are present in the synthesized netlist.

### Example 4: dff_const4.v

                vim dff_const4.v

### RTL Code:

![s4](https://user-images.githubusercontent.com/123365615/214628960-adb1600f-3ae4-431a-ac71-22f14e4a964f.PNG)

                iverilog dff_const4.v tb_dff_const4.v

                ./a.out

                gtkwave tb_dff_const4.vcd

Here, regardless of the input whether reset or not , Q1 is always going to be constant i.e. Q1=1 . As q can only be 1 or q1 depending on the reset input , but q1 = 1 .Thus q is also constant at the value 1. We can confirm this with the simulated waveforms as shown below.

![s10](https://user-images.githubusercontent.com/123365615/214642332-c4b552be-da6a-4f14-a19f-6c1ece260fbd.PNG)

                read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

                read_verilog dff_const4.v

                synth -top dff_const4

                dfflibmap -liberty ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib

                abc -liberty ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib 

                write_verilog -noattr dff_const4_netlist.v 

                show
                
As the output is always constant, it can easily be optimised using Yosys as shown in the graphical representation.

![s12](https://user-images.githubusercontent.com/123365615/214644050-6c26ea6e-2fae-498b-8f6f-2912f5f702e9.PNG)

Unused output optimisations

# Day 4: Gate Level Simulations,Blocking vs Non Blocking assignments,Synthesis-Simulation Mismatch

## Introduction to Gate Level Simulations

We validate our RTL design by providing stimulus to the testbench and check whether it meets our specifications earlier we were running the test bench with the RTL code as our design under test .
But now under GLS ,we apply netlist to the testbench as desh under test . What We did at the behavioral level in the RTL code got transformed to the net list in terms of the standard cells present in the library. So,net list is logically same as the RTL code. They both have the same inputs and outputs so the netlist should seamlessly fit in the place of the RTL code. We put the netlist in place of the RTL file and run the simulation with the test bench.
When we do simulation in with the help of RTL code there is no concept of timing analysis such as the hold and setup time which are critical for a circuit. For meeting this setup and hold time criteria there are different flavours of cell in the library.

![image](https://user-images.githubusercontent.com/123365615/214999519-f2427ef9-445e-4e31-ad1c-e8fcbd0866bb.png)

In GLS using iverilog flow, the design is a netlist which is given to Iverilog simulator in terms of standard cells present in the library. The library has different flavours of the same type of cell available.To make the simulator understand the specification of the different annotations of the cell the GATE level verilog models is also given as an input. If the GATE level models are timing aware (delay annotated ),then we can use the GLS for timing validation as well.

Question : If netlist is the true representation of my RTL code then what is the need of functional validation of my net list?
Answer: Because they can be simulation and synthesis mismatches.

### Synthesis Simulation Mismatches

        - Missing sensitivity list
        - Blocking and non blocking statements
Missing sensitivity list

Simulator functions on the basis of activity that is it looks for if either of the inputs change. If there is no change in the inputs the simulator won't evaluate the output at all.

##### Example:

        module mux(input i0, input i1, input sel, output reg y);

        always @(sel)
        begin
                if (sel)
                begin
                        y = i1;
                end
                else
                begin
                        y = i0;
                end
        end
        endmodule

The problem in this mux code is that simulation happens only when the select is high so if select is slow and there are changes in i zero aur i1 they get completely missed. So for the simulator this marks is as good as a latch a double h block but the synthesizer does not look at the sensitivity list rather on functionality creates a mux. Simulation: latch Synthesis:mux Hence,Mismatch.

Blocking and non blocking statements Inside always block

        -       Blocking Executes the statements in the order it is written So the first statement is evaluated before the second statement
        -       Non Blocking Executes all the RHS when always block is entered and assigns to LHS. Parallel evaluation

##### Example of Blocking assignments:

                module shift_register(input clk, input reset, input d, output reg q1);
                reg q0;

                always @(posedge clk,posedge reset)
                begin
                        if(reset)
                        begin
                                q0 = 1'b0;
                                q1 = 1'b0;
                        end
                        else
                        begin
                                q0 = d;
                                q1 = q0;
                        end
                end
                endmodule

In this case, D is assigned to Qo not which is then assigned to Q. Due to optimisation a single latch is formed where Q is equal to D.

##### Example of Non-Blocking assignments:

In the above RTL code if

              begin
                        q0 <= d;
                        q1 <= q0;
              end

In the non blocking assignments all the RHS are evaluated and parallel assigned to lhs irrespective of the order in which they appear. So we will always get a two flop shift register.

Therefore we always use non blocking statements for writing sequential circuits.

Other example:

module comblogic(input a, input b, input c, output reg y);
reg q0;

always @(*)
begin
	y = q0 & c;
	q0 = a|b;
end
endmodule
We enter into the loop whenever any of the inputs a b or C changes but Y is assigned with old Qo value since it is using the value of the previous Tclk ,the simulator mimics a delay or a flop. Where as, during synthesis we see the the OR and AND gates as expected.

Therefore ,while using blocking statements in this case,we should evaluate Q0 first and then Y so that Y takes on the updated values of Qo. Although both the circuits on synthesis give the same digital circuit comprising of AND, OR gates. But on simulation we get different behaviours.

Labs on GLS and Synthesis-Simulation Mismatch

### Example 1: A mux designed with the help of ternary operator

        vim ternary_operator_mux.v

![f1](https://user-images.githubusercontent.com/123365615/214810313-125798d0-fffc-4c81-9fae-e9e85f75ddb5.PNG)

	read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

	read_verilog ternary_operator_mux.v

	synth -top ternary_operator_mux

	abc -liberty ~/Desktop/vsd/vlsi/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib 

	write_verilog -noattr ternary_operator_mux.v

	show

The synthesized netlist for the above,using yosys

![y1](https://user-images.githubusercontent.com/123365615/215009271-7b157761-341a-44ff-aae0-4edeeb539af0.PNG)

To invoke GLS,

We need to read our netlist file and the test bench file assosciated with it.
We need to read 2 extra files that contain the description of verilog models in the netlist.

		iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v  ternary_operator_mux_net.v tb_ternary_operator_mux.v

To see the waveform of RTL simulation,we execute the following commands further

		./a.out
		gtkwave tb_ternary_operator_mux.vcd 

![y2](https://user-images.githubusercontent.com/123365615/215010497-7b63bf78-4678-46ea-aa6b-2e3d1b7aa3ef.PNG)

The generated netlist does behave like a 2X1 multiplexer.

### Example 2: bad_mux.v

        vim bad_mux.v

![f3](https://user-images.githubusercontent.com/123365615/214887908-00428702-c357-4d64-8697-db08dc22859b.PNG)

In this example,javascriptalways is evaluated only if javascriptselect is high.It is insensitive to javascriptio or i1 because javascriptselect is low.

	iverilog bad_mux.v tb_bad_mux.v

	./a.out

	gtkwave tb_bad_mux.vcd

This is verified by GTKWAVE of RTL Simulation below :

![y3](https://user-images.githubusercontent.com/123365615/215011355-9ef8af17-6468-4428-9741-2eabc43962df.PNG)

The design simulates a latch rather than a 2x1 mux.
But the Yosys implementation shows a 2X1 mux .

	read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

	read_verilog bad_mux.v

	synth -top bad_mux

	abc -liberty ~/Desktop/vsd/vlsi/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib 

	write_verilog -noattr bad_mux.v

	show

![y4](https://user-images.githubusercontent.com/123365615/215012096-8908aa2e-5e86-4e31-b2d3-f9ec1bdd7f4a.PNG)

	iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v  bad_mux_net.v tb_bad_mux.v
	
	./a.out
	
	gtkwave tb_bad_mux.vcd 

If we now implement it's GATE level netlist through GLS and observe the waveform,it shows the behaviour of a 2X1 mux as shown below:

![y5](https://user-images.githubusercontent.com/123365615/215012678-1ee977e5-4ddb-4e70-82f8-a974a25728af.PNG)

Since,the waveforms of stimulated RTL Code : Is of a LATCH the waveforms of gate level netlist thruogh GLS after synthesis: Is of 2X1 MUX We see a Synthesis-Simulation Mismatch.

### Example 3: This is an example of synthesis-simulation mismatch due to wrong order of assignment in blocking assignments.

        vim blocking_caveat.v

![f4](https://user-images.githubusercontent.com/123365615/214887961-7175fe34-905a-4c5e-bcf8-afb222bb5708.PNG)

We enter into the loop whenever any of the inputs a b or C changes but D is assigned with old X value since it is using the value of the previous Tclk ,the simulator mimics a delay or a flop. Where as, during synthesis we see the the OR and AND gates as expected.

	iverilog blocking_caveat.v tb_blocking_caveat.v

	./a.out

	gtkwave tb_blocking_caveat.vcd

![sp1](https://user-images.githubusercontent.com/123365615/215041432-7dd9ea7a-19c5-49b3-9d54-0d4dc36f2430.PNG)

At the instance where both the inputs a and b are 0. a | b should output 0, which when ANDed with c, should give an output y of 0. The output y thus should hold the value 0. Instead,it holds the value 1 . But due to the blocking statements in the rtl code, x actually holds a the value of a OR b from the previous clock, hence giving us an incorrect output.

	read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

	read_verilog blocking_caveat.v

	synth -top blocking_caveat

	abc -liberty ~/Desktop/vsd/vlsi/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib 

	write_verilog -noattr blocking_caveat.v

	show

The netlist representation on synthesis yields

![y8](https://user-images.githubusercontent.com/123365615/215014885-9439c2a7-de6b-4616-9f47-03ff212b1543.PNG)

The synthesizer does not see the sensitivity list rather the functionality of the RTL design.Hence,the netlist representation does not include any latches to hold delayed values pertaining to the previous cycle. It only includes an OR 2 AND gate.

	iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v  blocking_caveat_net.v tb_blocking_caveat.v
	
	./a.out
	
	gtkwave tb_blocking_caveat.vcd 

If we run gate level simulations on this netlist in verilog, we observe the following waveform.

![sp2](https://user-images.githubusercontent.com/123365615/215042116-e5253db2-1259-4f1e-86dc-da6525afc96b.PNG)

Here , we observe that the circuit behaves as intended combinational ckt. Output d results from the present value of inputs, and not the previous clock values like in the simulation results. Since the waveforms of the stimulated RTL verilog code do not match with the gate level simulation of generated netlist,we get a Synthesis-Simulation Mismatch again.

# Day 5 - If, Case, For Loop and For Generate

## If Constructs

If condition is used to to write priority logic. The condition one has a priority or if has more priority than the consecutive else statements . Only when condition 1 is not met condition 2 is evaluated and so on and y is assigned accordingly depending on the matching conditions.

	if (cond_1)
	begin 	
		y = statement_1;
	end
	else if (cond_2)
	begin
		y = statement_2;
	end
	else if (cond_3)
	begin
		y = statement_3;
	end
	else
	begin
		y = statement_4;
	end

If-else block implements a Priority logic that is if cond_1 is satisfied, next if statements are not executed. Thus above If-Else code translates to a ladder like multiplexer structure in the final design instead of a single multiplexer.

Dangers of using Incomplete If statements Inferred Logic which occurs due to bad coding styles that is incomplete if statements.

	if (condt1) 
	    y-a;
	else if (condt2)
	    y=b;
    
In the above code if condition 1 is matched y is equal to a else if condition 2 is matched y is equal to b but there is no specification for the case when condition2 is not matched, as a result of which the simulator tries to latch this case to the output y.It wants to retain the value of y.
This is a combinational loop to avoid that the simulator infers a latch. Enable of this latch is OR of the condition 1 and condition 2. If neither condition 1 or condition 2 is met the OR gate output disables the latch . The latch retains the value of y and stores it.
This is called the inferred latch due to incomplete if statements which is very dangerous for RTL designing. It should be avoided except for some special cases like the counter.

	reg[2:0]
	always @(posedge clk,posedge reset)
	Begin
	If (reset)
	Count<= 3'b000;
	else if(enable)
	Count<= count+1;
	end

This is also a case of incomplete if statements. Here ,if there is no enable the counter should latch onto the previous value.For example if the counter has counted up till 4 and there is no enable then it should retain the value 4 rather than going to 0 again.
So here the incomplete if statements result in latching And retaining the previous value which is our desired behavior in a counter. The earlier mux example was a combinational circuit and therefore we cannot have inferred latches.

#### Note:

If, case statements are used inside always block.
In verilog whatever variable we use to assign in if or case statements must be a register variable.

#### CASE Constructs

Let's look at the following verilog code block. Here, the inferred hardware should be a 4:1 multiplexer. The CASE statements do not have priority logic like IF statements.

	always @(*)
	begin
	     case(sel)
			2'b00: begin
				y = statement_1;
				end
			2'b01: begin
				y = statement_2;
				end
			2'b10: begin
				y = statement_3;
				end
			2'b11: begin
				y = statement_4;
				end
		endcase
	end

Depending on the cases matching the select y is assigned accordingly.

Some caveats with using CASE statements:

1.Incomplete CASE

Let's say I have a two bit variable select.

	reg [1:0] sel
	always @(*)
	begin
	     case(sel)
	     2'b00: begin
			   . condition 1
			   end
	     2'b01: begin
			   . condition 2
			   end
	    end case
	    end
    
Then select is C1 or C3 the conditions are not specified. It causes an incomplete case which results in inferred latches for these two cases that latch on to output y.This occurs when some cases are not specified inside the CASE block .For example, if the 2'b10 and 2'b11 cases were not mentioned , the tool would synthesize inferred latches at the 3rd and 4th inputs of the multiplexer.
Solution is code case with default inside the CASE block so that the tool knows what to do when a case that is not specified occurs.

#### Correct code :

	reg [1:0] sel
	always @(*)
	begin
	     case(sel)
	     2'b00: begin
			   . condition 1
			   end
	     2'b01: begin
			   . condition 2
			   end
	    default :begin
			   . condition 3
			   end   
	    end case
	    end
    
2.Partial assignments

	always @(*)
	begin
		case(sel)
			2'boo: begin
				x = a;
				y = b;
			2'b01: begin
				x = c;
			default: begin
				x = d;
				y = d;
				end
		endcase
	end

In the above example, we have 2 outputs x and y. This will create two 4X1 multiplexers with the respective outputs. If we look at case 2'b01, we have specified the value of x for this case ,but not the value of y. It appears that it is okay to do so, as a default case is specified for both the outputs, and if we don't directly specify the value of y for any case, the simulator will implement the default case. This, however , is incorrect. In partial assignments such as this, the simulator will infer a latch at the 2nd input for multiplexer y as no value is specified for a particular case.

3.Overlapping cases

	always @(*)
	begin
		case(sel)
			2'b00: begin
				y = a;
			2'b01: begin
				y = b;
				end
			2'b10: begin
				y = c;
			2'b1?: begin
				y = d;
				end
		endcase
	end

In the above code block ,2'b1? specifies that the corresponding bit can be either be 0 or 1. This means when the sel input is holding a value 3 i.e 2'b11, cases 3 and 4 both hold true. What is synthesized depends on the mercy of the simulator. It can lead to Synthesis-Simulation mismatches.

If we used an IF condition here, due to priority logic, condition 4 would be ignored when condition 3 is met. However,in the CASE statement , even if the upper case is matched,all the cases are checked.So,if there is overlapping in cases,it poses a problem as the cases are not mutually exclusive. And we would get an unpredictable output.

## Labs on Incorrect IF and CASE constructs

### Example 1: Incomplete If statements

Below is the file titled incomp_if.v, and can be found in the directory verilog_files.

	vim incomp_if.v

![ps1](https://user-images.githubusercontent.com/123365615/215052418-a6c8d3e1-6d49-46f7-abeb-26e15e3ff492.PNG)

	iverilog incomp_if.v tb_incomp_if.v

	./a.out

	gtkwave tb_incomp_if.vcd

The code contains an incomplete IF statement as no else condition corresponding to it is mentioned . On simulating this design , following gtkwave is obtained

![ps2](https://user-images.githubusercontent.com/123365615/215053427-d7ab5c98-d331-4db3-b5d0-7b096f686a6b.PNG)

From the above waveform, We observe no change in y when i0=0.It's equal to previous value when io=0. This shows latching Action, which is verified by looking at the synthesis implementation using Ysosys.

	read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

	read_verilog incomp_if.v

	synth -top incomp_if

	abc -liberty ~/Desktop/vsd/vlsi/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib 

	write_verilog -noattr incomp_if.v

	show

![ps3](https://user-images.githubusercontent.com/123365615/215055389-572f3982-f818-42ee-9d20-05e31eadeab7.PNG)

We see a D latch is created in the synthesized netlist.

### Example 2:

	vim incomp_if2.v

Below is a similar example of incomp_if2.v

![ps4](https://user-images.githubusercontent.com/123365615/215055805-81536bba-dbce-4cb8-bd84-a575b5bc55bf.PNG)

	iverilog incomp_if2.v tb_incomp_if2.v

	./a.out

	gtkwave tb_incomp_if2.vcd

The above code contains an incomplete IF statement as well. Here, we have 2 inputs i1 and i3, as well as 2 conditional inputs i0 and i2. As we do not specifythe case when both i0 and i2 go low,which results in an issue in the synthesis. The gtkwaveform of the simulated design is below

![ps5](https://user-images.githubusercontent.com/123365615/215056834-f164a455-82fd-460b-9e4e-97ffae67571a.PNG)

Observation: When io is high,output follows i1. When io is low,it looks for i2.If i2 is high,it follows i3. But if i2 is low(and io is already low),y attains a constant value=previous output.

	read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

	read_verilog incomp_if2.v

	synth -top incomp_if2

	abc -liberty ~/Desktop/vsd/vlsi/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib 

	write_verilog -noattr incomp_if2.v

	show

This can be verified by checking the graphical realisation of the yosys synthesis below.

![ps6](https://user-images.githubusercontent.com/123365615/215057451-b80b07b6-7d1a-409b-9d91-6fb39c319d42.PNG)

Yosys synthesizes a multiplexer as well as a latch with some combinational logic at its enable pin.

### Example3:

	vim incomp_case.v

Below is a design with Incomplete Case's specification in a mux.

![ps7](https://user-images.githubusercontent.com/123365615/215057990-d74d2f5d-ffb7-4896-8252-146d78b65fa3.PNG)

The truth table for the above 2X1 mux looks like:

![10001](https://user-images.githubusercontent.com/123365615/215058257-c589f074-9f59-4ebf-b30a-7bbad64b96cf.png)

	read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

	read_verilog incomp_case.v

	synth -top incomp_case

	abc -liberty ~/Desktop/vsd/vlsi/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib 

	write_verilog -noattr incomp_case.v

	show

Whenever se[1]=1 ,latching action takes place. The yosys synthesis implementation is given below.

![ps8](https://user-images.githubusercontent.com/123365615/215059160-cff5b653-ce93-4043-9d7a-34a44e663ea2.PNG)

Observation: 1. !(sel[1]) is going to D latch enable. 2.The inputs io,sel[0], !(sel[1]) go to the upper mixing logic that is implemented on D pin of the latch.

### Example 4:

vim comp_case.v

Complete Mux along with all the cases specified.

![ps9](https://user-images.githubusercontent.com/123365615/215059539-ccbf2932-1389-4571-8940-df7d8389d84f.PNG)

	read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

	read_verilog comp_case.v

	synth -top comp_case

	abc -liberty ~/Desktop/vsd/vlsi/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib 

	write_verilog -noattr comp_case.v

	show

Output follows i2 at default case,if i1 and io go low. Hence a 4X1 mux is synthesized without any latch that can be verified below.

![ps10](https://user-images.githubusercontent.com/123365615/215061571-d4dd2974-0619-42aa-b251-bdf4841c5125.PNG)

### Example 5: Partial Assignments

	vim partial_case_assign.v

![ps11](https://user-images.githubusercontent.com/123365615/215062105-5bbf277b-8ff7-4992-ae73-11f797125829.PNG)

Expected Circuit: The 2X1 mux with output y is inferred without any latch. To find out the latching condition of the second mux we take the help of the following truth table:

![10002](https://user-images.githubusercontent.com/123365615/215062305-b016335b-5a97-42f8-922f-a245da4e6210.png)

Condition for enabling:

	en=sel[1]+!(sel[0]) 

#### Using Redundancy Theorem

	read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

	read_verilog partial_case_assign.v

	synth -top partial_case_assign

	abc -liberty ~/Desktop/vsd/vlsi/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib 

	write_verilog -noattr partial_case_assign.v

	show

Yosys implementation of the above design after synthesis,

![ps12](https://user-images.githubusercontent.com/123365615/215063144-3f9af51e-a94e-41a1-91ad-eea680716b9c.PNG)

As expected, Only 1 D latch is inferred for X. No latch is inferred for y.

### Example 6: Design of 4X1 mux having overlapping cases

	vim bad_case.v

![ps13](https://user-images.githubusercontent.com/123365615/215063587-382774ba-8df5-4cdb-8ca8-6848f1cc8c1b.PNG)

	iverilog bad_case.v tb_bad_case.v

	./a.out

	gtkwave tb_bad_case.vcd


In gtkwaveform of RTL simulation:

![ps14](https://user-images.githubusercontent.com/123365615/215075906-d914f761-9362-4548-a970-593190965d7c.PNG)

Observation : When sel[1:0]=11, the output neither follows i2 nor i3. It simply latches to 1.

Whereas while running GLS on the netlist,the waveform of the synthesized netlist behaves as 4X1 mux as shown below

![ps15](https://user-images.githubusercontent.com/123365615/215076218-2f0a567b-d85c-477a-86d1-6aad361e1abc.PNG)

Thus ,Overlapping cases confuse the simulator and leads to Synthesis-Simulation Mismatches.

## Introduction to Looping Constructs

There are two types of FOR loops in verilog.

1.	FOR loop 1.Used within the always block 2.Used to evaluate expressions

2.	Generate FOR loop 1.Only used outside the always block 2.Used for instantiating hardware

Necessity of FOR loops

For loops are extremely useful when we want to write a code /design that involves multiple assignments or evaluations within the always block. Lets us take an example: If we want to write the code for 4:1 multiplexer, we can easily do so using a either four if blocks or using a case block with 4 cases,as seen in the previous if-else blocks.But this approach is not suitable for complicated design with numerous inputs/outputs say 256X1 mux.If we wanted to design a 256X1 multiplexer, we will have to write 256 lines of condition statements using select and corresponding assignments. But in for loop ,be it 4X1 or 256X1 we would always be writing 4 lines of code only. Although we need to provide 256 inputs using an internal bus.

	integer k;
	always @(*)
	begin
		for (k = 0, k < 256, k= i  +1)
		begin
			if (k== sel)
				y = in[i];
			end
		end
	end

This code can be infinitely scaled up by just replacing the condition i < 256 with the desired specification for our multiplexer.

Similarly, we can create High input demultiplexers as well.

	integer k
	always @(*)
	begin
		int_bus[15:0] = 16b'0;
		for (k= 0; k< 16; k= k+ 1) 
		begin
			if (k == sel)
				int_bus[k] = inp[k];
			end
		end
	end

Here , we have created a 16:1 demultiplexer using for loops within the always block. The int_bus[15:0] specifies our internal bus which takes on the input of the demux. It is necessary to assign all outputs to low for a new value of sel else latches will be inferred resulting in the incorrect implementation of our logic.

Below are few of the examples of FOR construct .

### Example 1:

file mux_generate.v that generates a 4X1 mux using For loop.

	vim mux_generate.v

![ps16](https://user-images.githubusercontent.com/123365615/215077203-3a628841-acf3-465a-8330-9c2dde0b0a0c.PNG)

The 4 inputs get assigned to a the internal 4 bit bus named i_int.

	iverilog mux_generate.v tb_mux_generate.v

	./a.out

	gtkwave tb_mux_generate.vcd

The gtkwave obtained after the simulation

![ps17](https://user-images.githubusercontent.com/123365615/215078078-f7e33dca-a368-4a8a-b3f2-d6a86f7443dd.PNG)


### Example 2:

	vim demux_generate.v

Similar to example 1, file demux_generate.v that generates a 4X1 demux using For loop.

![ps18](https://user-images.githubusercontent.com/123365615/215078338-7b7214e0-0aac-457e-840e-998477e653da.PNG)

The above code has good readabilty,scalability and easy to write as well. Let's verify if it functions as a 8X1 demux as expected by viewing its gtkwave simulated waveform.

	iverilog demux_generate.v tb_demux_generate.v

	./a.out

	gtkwave tb_demux_generate.vcd

![ps19](https://user-images.githubusercontent.com/123365615/215080523-6cf1273f-4b5d-42ac-81f6-8c3d20a830aa.PNG)

### FOR Generate and its Uses

FOR Generate is used when we needto create multiple instances of the same hardware. We must use the For generate outside the always block.

We take example of a 8 bit Ripple Carry Adder(RCA) to understand the ease of instantiations provided by the For generate statement. An RCA consists of Full Adders tied in series where the carry out of the previous full adder is fed as the carry in bit of the next full adder in the chain. Hence, we can make use of generate for to instantiate every full adder in the design , as they are all represent the same hardware.

For this example , we use the file rcs.v which holds the code for the ripple carry adder. It also needs to be included in our simulation.

	module rca (input [7:0] num1 , input  [7:0] num2 , output [8:0] sum);
	wire [7:0] int_sum;
	wire [7:0] int_co;

	genvar i;
	generate
		for (i = 1; i < 8; i=i+1) begin
			fa u_fa_1 (.a(num1[i]),.b(num2[i],.c(int_co[i-1],.co(int_co[i]),.sum(int_sum[i]));
		end

	endgenerate
	fa u_fa_0 (.a(num1[0]),.b(num2[0]),.c(1'b0),.co(int_co[0]),.sum(int_sum[0]));

	assign sum[7:0] = int_sum;
	assign sum[8] = int_co[7];
	endmodule

Here, fa references another verilog design file containing the definition of for the full adder submodules .This is shown below, from the fa.v file

	module fa(input a, input b , input c,  output co, output sum);
		assign  {co,sum} = a +b + c;
	endmodule

In the RCA verilog code, we instantiate fa in a loop using generate for outside the always block.

Rules for addition :

N + N bit number --> Sum will be N + 1 bits N +M bit number --> Sum will be max(N,M) +1 bits

Now, let us simulate this design in verilog and view its waveform with GKTWave .As the rca design referances the file fa.v , we must specify it in our commands as follows

	iverilog fa.v rca.v tb_rca.v
	./a.out
	gtkwave tb_rca.v

the resulting gtkwaveform is shown below that shows an adder being simulated:

![ps21](https://user-images.githubusercontent.com/123365615/215082717-708e272a-f669-41d3-95e7-64c6adcddb05.PNG)

Takeaways from this workshop:

-	We succesfully learnt How to write the verilog codes using for,generate,if-else,case ,blocking/non blocking assignments so that our intended functionality is met.
-	We learnt to genertaed the gtk waveform ans see the simulated wave results. We learnt to read timing diagrams as well.
-	We learnt to synthesize the rtl verilog design using yosys and understand how the synthesizer implements the logic considering area and delay optimisations
-	We learnt to verify the simulation -synthesis results by feeding netlist+testbench+Gate verilog models to the iverilog,during GLS(gate level synthesis simulation).
-	We learnt good coding practices and ways to write optimised verilog codes.

# Day 6
 
![image](https://user-images.githubusercontent.com/123365615/215937593-7cbf414c-11ef-45dc-864f-d40aa86d469b.png)

![image](https://user-images.githubusercontent.com/123365615/215937631-149ac37b-8f82-4768-b88c-53165b652bf0.png)

![image](https://user-images.githubusercontent.com/123365615/215937662-b98c05e6-251f-4a47-8889-20997905ba67.png)

![image](https://user-images.githubusercontent.com/123365615/215937685-c8cad4dc-c107-466d-8ae3-d3eb521e79ec.png)

![image](https://user-images.githubusercontent.com/123365615/215937719-d1f1e54a-82b0-435b-8d80-44e099356ac9.png)

![image](https://user-images.githubusercontent.com/123365615/215937748-0db6007a-2eae-4a4d-9f00-5ff47dce7457.png)

![image](https://user-images.githubusercontent.com/123365615/215937807-ca2871a4-aa7f-4cd5-995e-cb8616be40a8.png)

![image](https://user-images.githubusercontent.com/123365615/215937829-60a6684f-89ce-4237-a8a9-e895a14b370c.png)

![image](https://user-images.githubusercontent.com/123365615/215937842-73957c8a-6cfb-40e8-b78a-192e89612398.png)
 
Warning

![image](https://user-images.githubusercontent.com/123365615/215937872-b6c9a0ae-9a9a-460b-a6b0-c8e801badd8b.png)

![image](https://user-images.githubusercontent.com/123365615/215937916-eb970407-3c34-416d-be0d-1ba51eb6db66.png)

![image](https://user-images.githubusercontent.com/123365615/215937958-70095f95-16df-46cd-aee4-2107619497b2.png)

![image](https://user-images.githubusercontent.com/123365615/215937991-60ef9b7a-e92b-42a7-9a56-9d8183a7c069.png)

Error1

![image](https://user-images.githubusercontent.com/123365615/215938026-7ef698c8-e072-476f-bcec-813bc491da8f.png)

Error2

![image](https://user-images.githubusercontent.com/123365615/215938056-b11944a5-14f0-4753-b31a-e0df900e8c4a.png)

 
# Day 7
















































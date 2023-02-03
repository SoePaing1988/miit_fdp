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

 
# Day 7 -Inception of open-source EDA, OpenLANE and sky130 PDK

## How to talk to computers?

To perform the intruction, which are given externaly in computers, some hardware is there which convert our external instruction into understandable language of computers. In this hardware, many things are there. For example FPGA board, Arduino board etc. are provides the medium for external inputs and outputs.

Taking the example of Aurdino board. Arduino consists of both a physical programmable circuit board (often referred to as a microcontroller) and a piece of software, or IDE (Integrated Development Environment) that runs on the computer, used to write and upload computer code to the physical board.

![image](https://user-images.githubusercontent.com/123365615/216037006-07604476-5fc9-46f6-98c1-979eaabb1a84.png)

Here in the board, the ATmega328 microcontroller is used. Basically microcontroller is made from package, inside the package chip is there.Chip is made by pads, core, die and foundary IP's.

### Package
The package is a case that surrounds the circuit material to protect it from corrosion or physical damage and allow mounting of the electrical contacts connecting it to the printed circuit board (PCB). here what we are seeing in the black color is the Package of the microcontroller.

![image](https://user-images.githubusercontent.com/123365615/216037180-c317d6c0-60a2-4177-ad06-4cc585b9646c.png)

### Chip

Inside the Package, chip is available which contains, foundary IP's (for example, RISC-V Soc, PLL, ADC, DAC, SRAM, SPD), core (core contains AND, OR etc. all gates and digital logics), pads (through which input and output signals are communicates).

![image](https://user-images.githubusercontent.com/123365615/216037288-105ac39a-e682-475c-8c05-71868ba49b01.png)

## What is RISC-V?
RISC-V, where five refers to the number of generations of RISC architecture that were developed at the University of California, Berkeley. RISC is an open standard instruction set architecture (ISA) based on established RISC principles. Unlike most other ISA designs, RISC-V is provided under open source licenses that do not require fees to use. A number of companies are offering or have announced RISC-V hardware, open source operating systems with RISC-V support are available, and the instruction set is supported in several popular software toolchains.

The instruction set is designed for a wide range of uses. The base instruction set has a fixed length of 32-bit naturally aligned instructions, and the ISA supports variable length extensions where each instruction can be any number of 16-bit parcels in length. The instruction set specification defines 32-bit and 64-bit address space variants. The specification includes a description of a 128-bit flat address space variant, as an extrapolation of 32 and 64 bit variants, but the 128-bit ISA remains "not frozen" intentionally, because there is yet so little practical experience with such large memory systems.

## How software communicate with Hardware?
Between apps or software (in our mobilephones and computers or laptops) and Hardware (in our mobilephones and computers or laptops), One full channel is there, which contains O.S. (operating system), compiler and assembler. This channel proccesses the inputs from the apps and gives the outputs to the Hardware. And according to the output of assembler, the hardware will perform.

![image](https://user-images.githubusercontent.com/123365615/216037644-e9a5900a-87ac-4dac-b8b2-b3a20b9992ba.png)

when we gives the inputs from the software, inputs is in the C,C++,JAVA etc. languages. Hardware not understand this language. So, this inputs goes throuth the system software cahnnel, where O.S. handles the input/output operations, it allocates the memory to for the codes and low level systems functions are there in O.S. Then program goes to the compiler, where program will compile and generate the HDL program.This HDL program is basically the sets of Instructions which can hardware is able to understand. The instructions are basically depends on what kind of hardware we are using. For example if we are using Mips processor then instruction formaate will be according to the Mips processor, if Hardware is RISC-V then the instruction formate will be according to the RISC-V and similerly for Arm and intel processors also.

This sets of instrusctions is now given to the Assembler. here according to the instruction set, assembler will generate the Binary code (contains only 0s and 1s). this binary code can be understandable for hardware. And according to this code, hardware will gives the output.

## How to design Digital ASIC?
To design Digital ASIC, few tools or things which are required from the day one. These are

- RTL Design
- EDA tools
- PDK data

### what is RTL design?
In digital circuit design, register-transfer level (RTL) is a design abstraction which models a synchronous digital circuit in terms of the flow of digital signals (data) between hardware registers, and the logical operations performed on those signals.for this designs many open sorces are available. like, librecores.org, opencores.org, github.com, etc...

### What is EDA tools?
The term Electronic Design Automation (EDA) refers to the tools that are used to design and verify integrated circuits (ICs), printed circuit boards (PCBs), and electronic systems, in general. many open sorces tools are available like Qflow, OpenROAD, OpenLANE, etc...

### What is PDK Data?
PDK is process design kit. It is interface between FAB and design. This data is collections of files like,

- process design rules: DRC, LVS, REX
- Digital standerd cell libreries
- i/o librerirs
- etc.....
- 
which are used to model a fabrication process for the EDA tools used to design an ICs. for example, in 2020, google release the open source PDK for FOSS 130nm production with the skywater technology. But right now it is at cutting age of the 5 nm also. But in many applications, the advance node is not required, and the cost of advanced node is also high as compared to 130nm processors. This 130nm processors are also fast processor. for example,
-	intel: P4EE @3.46 GHz(Q4'o4)
-	sky130_OSU (single cycle RV32i CPU) pipeline version can achieve more than 1 GHz clock

## Simplified RTL to GDSII flow

![image](https://user-images.githubusercontent.com/123365615/216038471-5d0ef070-c423-4e08-a18b-50d970a893c5.png)

### Synthesis In the synthesis, the design RTL is translated to a circuit out from the SCL. The resultant circuit is describes in HDL and usualy refered to the gate level netlist. the gate level netlist is functionaly equivelent to the RTL. "standard Cells" have regular layouts.

### Floor planning and Power planning
The main objective here is that to plan silicon area and distribute the power to the whole circuit. In the chip floor planning, the partition chip die between different system building blocks and place the i/o pads. In micro floor planning, we define the dimensions, pin locations, rows.

![image](https://user-images.githubusercontent.com/123365615/216038532-9d23f078-d5cb-4750-9b78-98078cf18c8c.png)

In power planning, the power network is connstructed. tipically, the chip is power by multiple VDD and GND. so, total components are connected to power supply horizontaly and vertically by metal streps. here parallel structures are used to reduce the resistance. To address the electromagnetization problem, power distribution network uses upper metal leyers, which are thicker than lower metal layers. Hence have less resistance.

![image](https://user-images.githubusercontent.com/123365615/216038624-daa4dcbe-2992-4ecb-b8a0-9d2240e53c1f.png)

### Placement
In this process, we place the gate level netlist on the floor planning rows, alligned with the sites. cells should be placed very closed to eachother to reduce the interconnnect delay. Usually placement is done in 2 steps:

- Global placement
- Detailed placement

![image](https://user-images.githubusercontent.com/123365615/216038954-9dd74862-568b-48c5-a026-4c107c1f2dce.png)

### Global placement
Global placement is very first stage of the placement where cells are placed inside the core area for the first time looking at the timing and congestion. Global Placement aims at generating a rough placement solution that may violate some placement constraints while maintaining a global view of the whole Netlist.

### Detailed placement
In detailed placements, we determined the exact route and layers for each netlist. the objective of detailed placement is valid routing, minimize area and meet timing constrains. Additional objective is minimum via and less power.

### Clock tree synthesis (CTS)
Before routing the signals, we have to route the clock. In the process of clock synthesis, we have distribute the clock to the every sequential elements. for example flipflops, registers, ADC, DAC ete. basically clock netwroks looks likes a tree. where the clock source is roots and the clock elements are end leaves. Synthesization should be done in a manner that with minimum skew and in a good shape.To minimize the clock skew by using the low-skew global routing resources for clock signals.Microsemi devices provide various types of global routing resources that significantly reduce skew.Usually a tree is a H tree, X tree etc.

![image](https://user-images.githubusercontent.com/123365615/216039024-8c2c09dc-5dc7-493d-ab53-fec05ea24e93.png)

### Routing
After routing the clock, the signal routing comes. Making physical connections between signal pins using metal layers are called Routing. Routing is the stage after CTS and optimization where exact paths for the interconnection of standard cells and macros and I/O pins are determined. There are two types of nets in VLSI systems that need special attention in routing:

-	Clock nets
-	Power/Ground nets

The sky130 PDK defines the 6 routing leyers. the lowest leyer is called local interconnect layer (titanium nitride layer). Other five layers are alluminium layers.

![image](https://user-images.githubusercontent.com/123365615/216039412-e1e244ed-087f-4bde-bc70-30498b7e092e.png)

In the proccess of routing, metal trackes forms a routing grids and these grids are huge. so, devide and conquer approach is use for routing. The two types of routing is used:

-	Global routing: Generates the routing guides
-	Detailed Routing: Uses the routing guides to implement the actual wiring

### Sign off

Once the routing is done, we can construct the final layout. This final layout will goes under the verification. Two types of verifications are there:

-	Physical verification: Here design rule checking will done and it will check the final layout and owners layout
-	Timing Verification: Here Static Timing Analysis will done

## Introduction to OPENLANE:

OPENLANE is an automated RTL to GDSII flow that is composed of several tools such as OpenROAD, Yosys, Magic, Netgen, Fault, CVC SPEF-Extractor, CU-GR, Klayout and a number of scripts used for design exploration and optimization. It is started as an Open-source flow for a true Open Source tape-out Experiment. striVe is a family of open everything SoCs:

-	Open PDK
-	Open EDA
-	Open RTL

striVe SoC Family

![image](https://user-images.githubusercontent.com/123365615/216039770-11bef97b-b1f4-46e4-9b53-34104a557934.png)

The main goal of OPENLANE is to produce a clean GDSII with no human intervation (no-human-in-the-loop). here the meaning of clean is that:
-	No LVS violations
-	No DRC Violations
-	No timing Violations

OPENLANE is tuned for skyWter130nm open PDK. it can be used to harden Macros and chips.there is two mode of operation
-	Autonomus : it is the push botton flow. with the push botton , it is a some time base design and due to this push botton, we get final GDSII
-	interactive : here we can run comamds and steps one by one.

It has large number of design examples(43 designs with their best configurations).

## OpenLANE ASIC Flow (Detailed)

![image](https://user-images.githubusercontent.com/123365615/216040005-170db5c0-14c3-45ec-b6ea-cd4a00a9e326.png)

The design exploration utility is also used for regression testing(CI). we run OpenLANE on ~ 70 designs and compare the results to the best known ones.

### DFT(Design for Test)

it perform scan inserption, automatic test pattern generation, Test patterns compaction, Fault coverage, Fault simulation.

![image](https://user-images.githubusercontent.com/123365615/216040195-1f05094d-73d2-4368-99fd-a89b534deea1.png)

After that physical implementation is done by OpenROAD app. physical implementation involves the several steps:

-	Floor/Power Planning
-	End Decoupling Capacitors and Tap cells insertion
-	Placements: Global and Detailed
-	Post Placement Optimization
-	Clock Tree synthesis (CTS)
-	Routing: Global and Detailed

Every time the netlist is modified.(CTS modifies the netlist and Post Placements optimization also modifies the netlist).so for that verification must be performed. The LCE(yosys) is used to formally confirm that the function did not change after modifying the netlist. ### Dealing with antenna rules Violation: when a metal wire segment is fabricated, it can act as antenna.as an antenna, it collect charges which can demaged the transister gates during the fabrication.

![image](https://user-images.githubusercontent.com/123365615/216040498-6cde0a18-edd0-4e8f-9840-3a4803399465.png)

To address this issue, we have to limit the lenght of the wire. usually this is the job of the router. If router fails to do this, then there are two solutions:

-	Bridging attaches a higher layer intermediary

![image](https://user-images.githubusercontent.com/123365615/216040586-1038cb43-4c9a-4401-b903-9836bcb1ce7b.png)

-	Add antenna diode cell to leak away charges.(Antenna diodes are provided by the SCL)

![image](https://user-images.githubusercontent.com/123365615/216040743-b9507725-c95a-4d34-b412-53b12b9bcc5b.png)

With OpenLANE, we took a preventive approach. here we add fake antenna diode next to every cell input after placement. Then run the Antenna checker on the routed layout. If the checker reports a violation on cell input pin, replace the fake diode cell by a real one.

![image](https://user-images.githubusercontent.com/123365615/216040807-65124dfe-fd1b-4879-8016-06a585c63973.png)

## Static Timing analysis(STA)

It involves the interconnect RC Extraction(DEF2SPEF) from the routed layout, followed by STA on OpenSTA(OpenROAD) tool. resulting report will shows the timing violations if any violations is there.

### Physical Verification (DRC and LVS)
Magic is used for design Rules checking and SPICE Extraction from Layout. Magic and Netgen are used for LVS.

## Open source EDA tools

OpenLANE Directory Structure in detail
##### cd command
cd means change directory. this command will help us to go inside the directory.

##### ls command
ls means listing the directory. It is used to find the list of total details of directory.

##### ls --ltr
This command will help to list the details in cronological order.

Here we are working in Sky130_fd_sc_hd PDK varient. where, "sky130" is process name or node name."fd" is a foundary name (skyWater foundary)."sc" means standerd cell librery files and the last one "hd" stands for high density(basically one type of varient).

Sky130_fd_sc_hd varient contains many technology files like verilog, spice, techlef, meglef,mag,gds,cdl,lib,lef,etc. (techlef file contains the layer information).

### Design Preparation Step

when we enter in the OpenLANE, we have to use flow.tcl because as a name says, it will goes with the flow using the script. And by using interactive switch, we will do step by step process. without interactive switch, it will run complete flow from RTL to GDSII. Now OpenLANE is open and we can see that prompt will change now.

	cd Desktop/work/tools/openlane_working_dir/openlane/
	docker
	./flow.tcl -interactive

![image](https://user-images.githubusercontent.com/123365615/216041538-bf8a6cee-f4bf-40c5-8c7c-718fc31fca5f.png)

Now we have to input all the packages which required to run the flow.

Now, here we are ready to execute the command.

Now, if we are going into the design folder in openlane, there are nearly 30-40 designs are already builted. Out of them we can open any of the design. for example, here we are opening the picorv32a.v design. In this design we can see many files are available. i.e., scr, config.tcl, etc. This config.tlc file contains every details about the design. for example, details about enrollment, clock period, clock period port etc.
 
	% package require openlane 0.9
	% prep -design picorv32a
 
![image](https://user-images.githubusercontent.com/123365615/216041848-2f8cd14c-fb33-4d0f-823f-21d4a5f44dcc.png)

Here we can see that the time period is set to the 5.00 nsec. but is we see in the openlane sky130_fd_sc_hd folder, the period is set about 24 nsec. so it is not override to the main file. If it override then give first priority to the main folder.

Another Terminal

	cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a	

	less config.tcl 

![image](https://user-images.githubusercontent.com/123365615/216042210-240620d8-da24-43c7-98bb-55523602c5a0.png)

Now, in openlane, we are going to run the synthesis, but before synthesis, we have to prepare design setup stage. for that command is " prep -design picorv32a".
so, here it is shown that preparation is completed.

### Review after design preparation and run synthesis

After completing the preparation, in the picorv32a file, the run terictory is created. Inside the folder, Today's date is created. so in this terictory some folders are available which is required for openlane.
	
	less sky130A_sky130_fd_sc_hd_config.tcl

![image](https://user-images.githubusercontent.com/123365615/216042850-f9c9aa86-ea92-466b-9532-101049cf98ee.png)

	cd runs
	ls
	cd 01-02_11-17/
	ls -ltr

![image](https://user-images.githubusercontent.com/123365615/216042936-2855620b-9921-44dc-8ea5-20c05ee33d44.png)

In the temp file, merged.lef file is available which was created in preparation time. if we open this merged.lef file, we get all the wire or layer level and cell level information.

	cd tmp
	less merged.lef 

![image](https://user-images.githubusercontent.com/123365615/216044358-d060b580-0cee-4c1e-80ae-4c9cca6597c6.png)

While, in the result folder is empty because till we have not run anything and in the report folder all the folders are there about synthesis, placement, floorplanning,cts,routing,magic,lvs.
 
	cd ..
	cd results/
	ls -ltr
	cd ..
	cd reports/
	ls -ltr

![image](https://user-images.githubusercontent.com/123365615/216044493-483c9a39-8636-4a6b-a3d8-a3bed608a2df.png)

now here also one config.tcl file is available similar like design folder. But this config.tcl file contains all default parameter taken by the run.
 
	cd ..
	less config.tcl

![image](https://user-images.githubusercontent.com/123365615/216044610-88f5d540-3f4a-472d-898b-98f31b3fc86a.png)

when we make some change in the origional configuration and then we run, for example if we make a change in core utilization in the floorplanning and then we run the floorplanning, at this time in the congig.tcl file, the core utility will change and by cross checking it we can check that the modification is reflected in the exicution or not.

Now, cmds.log file takes all the record of the commands, what we have fab.
 
	less cmds.log

![image](https://user-images.githubusercontent.com/123365615/216044666-8463caaa-002a-40f8-ad0a-f48a019bebaa.png)

 
At openlane terminal

	% run_synthesis
	
Now coming to the openlane, we are going to run the synthesis. It will take some 3-4 mnts to run the synthesis and finally synthesis will complited.

![image](https://user-images.githubusercontent.com/123365615/216044790-b5c6e77a-98ab-46fd-ab40-6e3d05ee9088.png)

From the data of synthesis, total number of counter D_flip-flops is 1613. and the number of cells is 14876.

![image](https://user-images.githubusercontent.com/123365615/216044832-bf7c1c51-3f70-430d-96bb-ac519569b6cd.png)

![m13](https://user-images.githubusercontent.com/123365615/216045477-23e570f5-0b73-4559-9d25-33452c1acc98.PNG)

So, the flop ratio = (number of flip flops)/(number of total cell).

So, the flop ratio is 10.84%.

Before run, we saw that the result folder is empty. but now, after running the synthesis, we can see that all the mapping have been done by ABC.
 
And in the report, we can see when the actual synthesis has done. and the actual statistics synthesis report is showing below, which is same as what we have seen before.
At normal terminal

	cd results/synthesis/
	ls
	less picorv32a.synthesis.v

![image](https://user-images.githubusercontent.com/123365615/216044886-f7683d8c-ed14-4a84-b642-7c8089be4fb4.png)

# Day 8 -Good floorplanning Vs Bad floorplanning and introduction to library cells

## Chip floorplanning considerations

### Utilization factor and aspect ratio

#### (1)defining the width and height of core and Die

![image](https://user-images.githubusercontent.com/123365615/216298473-36930eb0-adaa-453b-bc61-0fbea523f6c2.png)

let's begin with netlist( Netlist describes the connectivity of an electronic designs). Considering a netlist with 2 flops and 2 gates.

![image](https://user-images.githubusercontent.com/123365615/216298516-07d6ede8-7c10-47cd-bb1e-9692ad0840cb.png)

lets giving the height and width of standerd cell is 1 unit. So, area of standerd cell is 1 square unit. And assuming the same area for the flip flops also.

Now lets calculate the area occupied by the netlist on a silicon wafer by arranging them together. The total area occupied by netlist in silicon wafer is 4 square units.

![image](https://user-images.githubusercontent.com/123365615/216298721-f753f373-3c39-4817-9e2e-f585b701374a.png)

#### what is the core and die?

A 'core' is the section of the chip where the fundamental logic of the design is placed.

A 'Die', which is consist of core, is small semiconductor material specimen on which the fundamental circuits is fabricated.

![image](https://user-images.githubusercontent.com/123365615/216298830-4dae6780-6f95-4186-b879-fd46eef857a3.png)

If we put the 'arranged netlist' in the core, then whole core is occupied by netlist. that means utilization of core is 100%.

![image](https://user-images.githubusercontent.com/123365615/216298886-aba517ef-a3aa-435d-b3d8-6cbb67126272.png)

##### Utilization factor

it is defined as, 'the area occupied by netlist' devided by the 'total area of core'.

so, in above case, the utilization factor is 1.

practically, we don't go with 100% uti;ization. practically utilization factor is about 50%-60% to add other extra cells (like buffer) in the core after netlist is placed.

##### Aspect Ratio

it is defined as (height)/ (width). here in above example the aspect ratio is 1.

### Concept of pre-placed cells

#### (2) Define locations of preplaced cells

to define the preplaced cells, let's take a combinational logic i.e., mux, multiplier, clock devider, etc. and assuming that the output of the circuit is huge and assuming that the circuit has some 100k gates. so let devides in two blocks of 50k gates.

![image](https://user-images.githubusercontent.com/123365615/216299167-4ddfd067-004f-4bc4-89a2-a9abad0211d3.png)

This both blocks are implimented separatly. Now, extending the input and output pins from the both blocks. now, let's detached these box. we will black box the boxes and detached them. After black boxing, the upper portion is invisible from the top or invisible to the one , who is looking into the main netlist. now we make separate them out.

![image](https://user-images.githubusercontent.com/123365615/216299210-ae32f470-cf67-48eb-92d1-11975d5c8683.png)

This blocks are implemented in netlist once and then we can reuse it multiple time. Similarly, there are other modules or IPs also readily available.i.e.,memory, clock gating cell, comparater, mux. these all are part of the top level netlist.They recieve some signals, they perform some functions, they deliver the outputs but the functionality of the cell is implemented only once. That what we called as preplaced cells.

The arrangement of these ip's in a chip is refferd as floorplanning.These IP's have user-defined locations, and hence are placed in chip before automated placement and routing are called "preplacement cells".These cells are place din such fashion that, the placement and routing tool not touch the location of the cell.

![image](https://user-images.githubusercontent.com/123365615/216299276-47d31030-8875-4d0d-9d9b-dc336b937bee.png)

Let consider memory as a preplaced cells as a block 'a', block 'b' and block 'c'. Memory of any device is implemented once and reused multiple times. these preplaced cells are located as per the design bacckground. the location of the cells are never touched.

### De-coupling capacitors

#### (3) surround pre-placed cells with Decoupling capacitors

Let consider some circuit, which is part of the blocks which were defined above. When some gate (let consider AND gate) switched from 0 to 1 ot 1 to 0, considered amount of the switching current required because of small capacitance is available there. this capacitor should be completely charged to represent logic 1 and completly discharged to represent logic 0. the required amount of the charge is suplied from the Vdd and absorb from the Vss. during supplying the current, wire has some drop of voltage due to resistence and inductunce of the physical wire.

![image](https://user-images.githubusercontent.com/123365615/216299370-48bc9cca-5e65-4fed-93e3-6927f4320185.png)

So, due to this if ideal logic 1 = 1 volt then here practically it can be less then 1 volt i.e., 0.97 volts (Vdd'). So, for any signal to be considered as Logic '0' and '1' in the NM low and NM high range. It is danger case.

![image](https://user-images.githubusercontent.com/123365615/216299413-f8d2c7c9-48b3-4e2c-bcb3-5fa06b7d5525.png)

To solve this problem,, we have to put De-coupling capacitor in parallel with the circuit. Every time the circuit switches, it draws current from Cd, whereas, the RL network is used to replacenish the charge into Cd.

![image](https://user-images.githubusercontent.com/123365615/216299450-04b89a97-a9ce-42a9-80f3-b1540df802d8.png)

![image](https://user-images.githubusercontent.com/123365615/216299494-3cb1c9e9-c2a8-494d-8e8c-c6dec7a2240b.png)

### Power Planning

#### (4) How to do power planning?

Up to here, we have taken care of local communication. Now let consider that local circuitory as a black box and it can be repeat multiple times and power supply is also connected to all of them as shown below,

![image](https://user-images.githubusercontent.com/123365615/216299767-9027711b-29c5-45f3-b145-6be8d6dbe8f8.png)

Now 16 bit bus has to retain the same signal from driver to the load. so it should get the sufficient power from the supply. But at this bus, there is no de-coupling capacitor is available because it is not physible to put capacitor at all over the place. now, power supply is far away from the bus, that is why some drop between them will definalty occurs.

Let consider this 16 bit bus connected to inverter. So, all capacitor are initially charged will get discharged and vice-versa due to inverter.

![image](https://user-images.githubusercontent.com/123365615/216299798-492f30ed-c2a5-48de-85f6-e39c85355098.png)

But the problem is occurs due to all capacitor is connected to the single ground. This will cause a bump in 'ground' tap point during discharging.

![image](https://user-images.githubusercontent.com/123365615/216299959-cfa7da1d-ef30-4130-a022-6713e6f4a2dd.png)

Same problem will occurse during the charging time also. at that time lowering of voltage occurse at 'Vdd' tap point.

![image](https://user-images.githubusercontent.com/123365615/216300035-3c391d28-43b4-497e-b54c-cc01c23e8965.png)

The solution of the problem is use multiple power supply. So, every block will take charge from neartest power supply and similarly dump the charge to the nearer ground. this type of power supply is called mesh.

![image](https://user-images.githubusercontent.com/123365615/216300174-3e50b6c9-37ed-48b3-a211-c610164be890.png)

And the power planning is shown below,

![image](https://user-images.githubusercontent.com/123365615/216300264-512713d0-47f6-4ee5-8b6c-668efadd98a4.png)

### Pin placement and Logic floor planning considerations

#### (5) Pin Placement

Lets take below designs for example that needs to implemented.

![image](https://user-images.githubusercontent.com/123365615/216300409-021ee0a2-3afe-4512-8da3-3a80f58b473f.png)

Both the sections has different inputs like Din1 and Din2 and operated from different clocks like clock 1 and clock 2. along with that we have preplaced cells as well. So, basically now we have 4 input ports and 3 output ports.

let's have one more design that needs to be implemented. this types of circuits are very much helpful to understand the timing analysis of inter clocks.

now complete design becomes like given below which has 6 input ports and 5 output ports. it is called the 'Netlist'.

![image](https://user-images.githubusercontent.com/123365615/216300458-ca48ecb1-3d89-4e7f-8541-903c7fbb7eca.png)

Let's put this netlist in the core which we have designed before and let's try to fill this empty area between core and die with the pin information. The frontend team who decides the netlist input and output and the backend team who done the pin placements. So according to the pin placements, we have to locate the preplaced blocks nearer to the inputs of the preplaced blocks.

![image](https://user-images.githubusercontent.com/123365615/216300528-6f6bae18-c837-473c-8a67-7990fad56ac2.png)

Here one thing that we noticed is that clock-in and clock-out pins are bigger in size as compared to input and output pins. reason behind this is that, input clocks are conntinuously provides the signal to the every elements of the chip and output clock should out the signal as fast as possible. So, we need least resistance path for the clocks inputs and clocks outputs. So, bigger the size, lower the resistance.

One more thing is need to take care about is that, this pin placement area is blocked for routing and cell placements. so we nned to do logical cell placement blockage. this blockage is shoown in above image in between pins.

So, floor plan is ready for Placement and Routing step.

### Steps to run floorplan using OpenLANE

Before run the floorplanning, we required some switches for the floorplanning. these we can get from the configuration from openlane.

	cd Desktop/work/tools/openlane_working_dir/openlane/configuration/

	less README.md 

![y1](https://user-images.githubusercontent.com/123365615/216312550-fcd15363-cc5a-4f76-af16-1d2a99216e97.PNG)

Here we can see that the core utilization ratio is 50% (bydefault) and aspect ratio is 1 (bydefault). similarly other information is also given. But it is not neccessory to take these values. we need to change these value as per the given requirments also.

Here FP_PDN files are set the power distribution network. These switches are set in the floorplane stage bydefault in OpenLANE.

	less floorplan.tcl

![y2](https://user-images.githubusercontent.com/123365615/216313780-5a188869-ffc2-4571-a213-3be77cce0951.PNG)

Here, (FP_IO MODE) 1, 0 means pin positioning is random but it is on equal distance.

In the OpenLANE lower priority is given to system default (floorplanning.tcl), the next priority is given to config.tcl and then priority is given to PDK varient.tcl (sky130A_sky130_fd_sc_hd_congig.tcl).

Now we see, with this settings how floorplan run.

### Reviewing floorplan files and steps to view floorplan

In the run folder, we can see the connfig.tcl file. this file contains all the configuration that are taken by the flow. if we open the config.tcl file, then we can see that which are the parameters are accepted in the current flow.

	cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/02-02_11-34

	less config.tcl 

![y3](https://user-images.githubusercontent.com/123365615/216318160-9e425496-88b3-4057-9f0a-a5722008d3d9.PNG)

here we can see that, the core utilization is 35%, aspect ratio is 1 and core margin is taken as 0. while in default the core utilization is 50%. this is the issue. because this design is override the system. but it is the taken from PDK varient.tcl file. so priority vise it is true.

To watch how floorplane looks, we have to go in the results. in the result, one def( design exchange formate) file is available. if we open this file, we can see all information about die area (0 0) (660685 671405), unit distance in micron (1000). it means 1 micron means 1000 databased units. so 660685 and 671405 are databased units. and if we devide this by 1000 then we can get the dimensions of chips in micrometer.

	cd results/floorplan

	ls -ltr

	less picorv32a.floorplan.def

![y4](https://user-images.githubusercontent.com/123365615/216318818-4c629088-5f4c-45cc-92df-efd0e271e41d.PNG)

so, the width of chip is 660.685 micrometer and height of the chip is 671.405 micrometer.

To see the actual layout after the flow, we have to open the magic file by adding the command 

	magic -T /home/soe_paing/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef  def read picorv32a.floorplan.def

And then after pressing the enter, Magic file will open. here we can see the layout.

![y5](https://user-images.githubusercontent.com/123365615/216321008-a6199f3b-e7d7-4d5f-8d14-ed6085aef7c5.PNG)

### Reviewing floorplan layot with magic.

In the layout we can see that, input output pins are at equal distance.

![y6](https://user-images.githubusercontent.com/123365615/216321840-42a0e7c4-bfb2-4471-8dec-83b97fdf045c.PNG)

after selecting (To select object, first click on the object and then press 's' from keyboard. the object will hight lited. to zoom in the object, click on the object and then press 'z' and for zoom out press 'sft+z') one input pin, if we want to check the location or to know at on which layer it is available, we have to open tkcon window and type "what". it will shows all the details about that perticular pin.

![y7](https://user-images.githubusercontent.com/123365615/216328245-1e294913-1ca7-426a-bed0-4e5c5b20ef53.jpg)

![y8](https://user-images.githubusercontent.com/123365615/216328293-78d178ad-9c4a-4fe2-a05d-a76bf4f18abd.PNG)

so, it show that the pin is in the metal 3.similarly doing for the vertical pins, we find that this pin is at metal 2.

![y9](https://user-images.githubusercontent.com/123365615/216330325-d6223e57-4c7b-481a-8aa5-5f5d0843ddad.PNG)

Along with the side rows,the Decap cells are arranged at the border of the side rows.

![y10](https://user-images.githubusercontent.com/123365615/216330756-84288785-ac18-4734-af5e-8e9b4607ff13.jpg)

Another cells also placed here, which is a tap cells. these cells are meant to avoide the latch-up problems in the CMOS devices. it connect N-well to the Vdd and substrate to the Ground. these tap cells are placed at diagonally equal distance.

![j11](https://user-images.githubusercontent.com/123365615/216331438-eca3a9b0-9443-411a-be5b-67c9405a8eaf.jpg)

In the floorplane, standerd cells are not placed but here standerd cells are available in the left side of the floorplan. we can see few boxes are there.

![y12](https://user-images.githubusercontent.com/123365615/216329373-ea7b2608-972d-4813-ab52-6a6d92573a68.jpg)

here we can see that first standerd cells is for buffer 1. similarly other cells are for buffer 2, AND gate etc.

### Library building and Placement

#### Netlist binding and initial place design

##### (1) bind netlist with physical cells

Taking netlist as what we taken before,

![image](https://user-images.githubusercontent.com/123365615/216332028-1663658b-7b9e-4e78-b41e-614995690f02.png)

Here, we can see that every gate or flip-flops have a shape to understand the functionality of the element. But practically, this cells are square or rectangular boxes which has internally some logic to perform. So, here we are taking all the elements from netlist and giving them a perfact height and width with perticular dimention. These all cells together are called 'self'. And this self are stored in the lybrary. Library have all the innformation about all the blocks, like height, width , time delay, conditional innformation, etc. library also have a option for the similar cells (with same functionality) like this with different height and width. According to our space available at floorplanning we can choose out of them.

![image](https://user-images.githubusercontent.com/123365615/216332111-2b8335b2-469a-425f-adce-363a6dc859e7.png)

After giving size and shape to each and every box, next step is to take the boxes or element from library and placed in the floorplan. This is called placements of the cells.According to the design of the netlist, we have to put physical blocks in the floorplan which we have design before.Put all the blocks according to the input and output of that perticular blocks.

![image](https://user-images.githubusercontent.com/123365615/216332122-0dbf3e09-7a1b-4f34-8dd9-560563a5a955.png)
up to here we have done stage one and stage two placement. Now we will going for stage 3 and 4. here we have to place FF1 of stage 3 nearer to the Din3 and FF2 of stage 3 nearer to the Dout3. But Din3 and Dout3 are at somme distance from eachother. same thing is there for FF1 and FF2 of stage 4. Let's we placed these all element in such manner that all elements are closed to it's input and output pins.

![image](https://user-images.githubusercontent.com/123365615/216332155-1595f17e-57fc-49b8-bc2c-f13c5baf456c.png)

But, the distance of FF1 of Stage 4 and Din4 is still far them others. By optimizing the placement, we can solve this problem.

#### Optimize placement using estimated wire lenght and capacitance

##### (2) Optimize Placement

As we seen that the distance from Din2 to FF1 of stage 2 is higher. so if we connect the wire between them then resistance and capacitance of the wire comes in to the picture. and due to this the signal integrity can not maintain.

To maintain the integrity of the signal out from Din2 to FF1, we have to put some repeaters like buffers on between Din2 and FF1. But it will cause of loss of area.

In the stage 1, there is no need of any repeater to transmit the signal. But in stage 2, due to high distance, the lenth of wire is high and signal is not transmitted in perticular range. so we required repeater.

![image](https://user-images.githubusercontent.com/123365615/216332198-6ff2f97b-e2f9-4038-834a-6424171b0bb4.png)

#### Final Optimization

Similar as stage 2, in Stage 3 also we required the buffer between gate 2 and FF2.

![image](https://user-images.githubusercontent.com/123365615/216332247-4f2b6fb1-dddc-4f0e-af73-16ffb58eff6c.png)

Stage 4 is bit tricky as compared to other stages.

![image](https://user-images.githubusercontent.com/123365615/216332274-c119c399-cb8a-489f-bde2-0f1af0b9a18b.png)

Now we have to check that, what we have done is correct or not. For that we need to do Timing analysis by considering the ideal clocks and according to the data of analysis, we will understand that, the placement is correct or not.

### Need for libraries and characterization.

#### Library charactorization and modelling

In whole IC design, we have to go through synthesis, floor/power planning, placement, routing , STA. In all this steps one thing remain common, which is "Gates or Cells". That is where the library charactorization becomes very important.

#### Congestion aware placement using RePLAce

Right now we are not constrain about timing, but constrain about the congestion. so, we are making the congrstion is less.

The placement is donne in two stages. Global and detailed. In global placement, legalization is not happened but after detailed placement legalization will be done.

When we run the placement, first Global placement is happens. main objective of glibal placement is to reducing the length of wires.

Now opening the Magic file to see actual view of standerd cells placement.And the actual view in the magic file is given below,

![image](https://user-images.githubusercontent.com/123365615/216337509-e72672a5-8e00-4137-b06a-80f55086fc4c.png)

If we zooom into this, we find the buffers, gates, flip flops in this.

![y20](https://user-images.githubusercontent.com/123365615/216337843-37517509-5866-495b-b578-0074ce9860ef.PNG)

### Cell design and characterization flows

#### Inputs for cell design flow

##### Cell design flow

As we know that standerd cells are placed in the library. And in the library many other cells are available which have same functionality but the size is different. As size is different the parameters like hVt, Ivt also different for each standerd cells.

If we take one of the standerd cells called inverter, it has their own design flow by this it can be understandable to EDA tool.

The cell design flow is devided into three part.

-	Inputs
-	Design steps
-	Outputs

![image](https://user-images.githubusercontent.com/123365615/216338410-09b41a34-e6d9-4b8c-89c7-0be06b5d87fc.png)

##### (1)inputs

inputs required for cell design is PDKs, DRC and LVS rules SPICE models, library and user defined specs.

#### Circuit design steps

##### (2)design steps

steps are circuit design, layout design, characterization

##### (3)Outputs

The typical output what we get from the circuit design is CDL(circuit description language) file,GDSII,LEF,extracted spice netlist(.cir).

#### Layout design steps

-	implement function

![image](https://user-images.githubusercontent.com/123365615/216338491-104e241a-5911-4735-b6a8-ed92ec5fd3dc.png)

-	Derive the NMOS and PMOS network graph

![image](https://user-images.githubusercontent.com/123365615/216338531-da14d07d-d81a-4e26-91d3-497d3eb13cce.png)

-	Obtain the Euler's path

![image](https://user-images.githubusercontent.com/123365615/216338623-9ce3cc4d-fc21-42cf-8958-dfc6b27a714b.png)

-	Stick diagram

![image](https://user-images.githubusercontent.com/123365615/216338658-ec65d01b-24ea-4ce3-b9bb-b043d6c8111c.png)

-	Convert stick diagram into proper layout

![image](https://user-images.githubusercontent.com/123365615/216338702-d82907a3-d678-4753-a3ef-56511bfa8093.png)

After layout design, we have to ecxtract the layout and characterize it. In characterization step, we can get the information about Timing, Noice,power.libs and function.

#### Characterization Flow

As of now, from the circuit design and layout design, we have final layout of buffer cell. where two buffers are connected in series with each other.

[image28](https://user-images.githubusercontent.com/123488595/215042474-c46416f4-7d0d-4a4d-8359-4d24144a2773.png)

Now steps of flow is:

-	Read the model file
-	read the extracted spice netlist
-	reconize the behavior of buffer
-	Read the subcircuit of the inverter
-	Ateched the neccessory power source
-	Apply the stimulus
-	Provide the necessory of output capacitance
-	Provide the necessory simulatin command
-	This all steps we have to give in "GUNA" software. and this software will give the timing, noise, power.libs and functions.

### General Timing characterization parameters

#### Timing threshold defination

![image](https://user-images.githubusercontent.com/123365615/216338883-677d6e05-bdaa-4583-8993-a4d056d61f37.png)

Let we take the waveform from the output of the first buffer and it will be input of the second buffer and taking output of the second buffer also.

[image30](https://user-images.githubusercontent.com/123488595/215048979-74e91dfe-b67e-42fc-8845-c70834af6314.png)

-	slew_low_rise_thr
here low means nearer to the ground, and rise tresold means we want to measer the slope of the increasing graph. typical value of slew low rise thr is around 20-30%.

-	slew_high_rise_thr
same as above, high means nearer to high value.

![image](https://user-images.githubusercontent.com/123365615/216338971-0f595bc6-4f1d-40bd-9535-06eba30b1957.png)

whenever we want to calculate the slew, take the point at 20% from the low and take the point at 20% from the high. according to these point, take the time data and the time difference between them will helps to calculate the slew.

-	slew_low_fall_thr

![image](https://user-images.githubusercontent.com/123365615/216339016-6f2a9310-d06c-4217-9d69-46bbf18b54f4.png)

-	slew_high_fall_thr

![image](https://user-images.githubusercontent.com/123365615/216339085-8ab4300f-7fc5-4f29-aee2-25e5b2856b4e.png)

NOw, taking the waveform of input stimulus which is input of the first buffer and with that taking output of the first buffer.

Similar as a slew, thresolds are for delay also available. for that same as slew, we have to take some rise and fall points from the waveforms. this tresolds are almost 50%.

-	in_rise_thr

![image](https://user-images.githubusercontent.com/123365615/216339169-3556935d-002d-4e02-8ecc-996c85cfd825.png)

-	in_fall_thr

![image](https://user-images.githubusercontent.com/123365615/216339241-09b3595d-ceca-4632-86fd-f6cc3798ad3a.png)

-	out_rise_thr

![image](https://user-images.githubusercontent.com/123365615/216339288-82e9fea5-2eb3-4926-ac62-195ce4b4d7a3.png)

-	out_fall_thr

![image](https://user-images.githubusercontent.com/123365615/216339379-8880626c-b9b4-4ee2-b013-0d20973a328f.png)

So, according to rise and fall theresold we can find the rise delay and fall delay of the buffer.

![image](https://user-images.githubusercontent.com/123365615/216339429-93e9331d-b208-42d7-a758-6f62b2d6e09b.png)

### Propogation delay and Transition time

#### propogation delay

let's take the same setup for understand the propogation delay. Time delay = Time(out_thr)-time(in_thr).

Let's take waveform on which we can apply above formula.

![image](https://user-images.githubusercontent.com/123365615/216339480-e0b3d2b5-ccfc-48c8-a313-e5b60737ec6e.png)

In any case if thresold point move at the top, at that time we get negative delay because output comes before input. so reason behind the negative delay is the poor choice of the tresold points. which is not acceptable. so, choosing the thresold point is very important.

![image](https://user-images.githubusercontent.com/123365615/216339513-2c13d6aa-ef78-47e0-8b22-3fc6489e36db.png)

Taking another example where the wire delay is very high because of high distance between two elements. so, here by choosing correct thresold point then also we get the negative delay.

https://user-images.githubusercontent.com/123488595/215055564-4dfad413-5dcf-47b0-89ca-bfa81a55163f.png

#### Transition time

transition time = time(slew_high_rise_thr)- time(slew_low_rise_thr)

or

transition time = time(slew_high_fall_thr)- time(slew_low_fall_thr)

let's take waveform for understand the slew calculation.

![image](https://user-images.githubusercontent.com/123365615/216339637-d285b7db-c8da-4ba6-a460-20e8c85d03ad.png)


## Day 9 -Design library cells using Magic Layout and ngspice characterization

### Labs for CMOS inverter ngspice simulations

### SPICE deck creation for CMOS innverter

#### IO placer revison

Till now, we have done floor planning and run placement also. But if we want to change the floorplanning, for example, in our floor planning, pins are at equal distance and if we want to change it then we can also make it by "Set" command.

For that first we have to check the swithes in the configuration and from that we have to take the syntax "env(FP_IO_MODE) 1". and make it to the "env(FP_IO_MODE) 2". then again run the floorplanning.

Then check the changes in the pins location through magic -T.

image1

So, here we can see that there are no pins in the upper half side. all pins are in the lower half of the core.

SPICE deck creation for CMOS inverter

### VTC- SPICE simulations.

#### Before entering into the simulation, we have to creat the spice deck. The spice deck is nothing but netlist. so, we have to creat the spice deck for CMOS.

-	Component connectivity

![image](https://user-images.githubusercontent.com/123365615/216557073-5dbacd46-b890-43a9-a96c-a16fa288f050.png)

-	Define the components values

![image](https://user-images.githubusercontent.com/123365615/216557161-2a0beacd-550a-4a6c-bd24-b18ed05466dc.png)

-	Identyfy the nodes

![image](https://user-images.githubusercontent.com/123365615/216557215-6c7498c7-ceae-4051-9027-8c17b4f8a692.png)

-	Name 'Nodes'

![image](https://user-images.githubusercontent.com/123365615/216557320-75df4402-fa7d-465f-8f4a-4bf7bdc7e05e.png)

Now, let's strat the writing the SPICE deck

![image](https://user-images.githubusercontent.com/123365615/216557365-19b1919e-aaff-4718-82c0-f2cd520c8c59.png)

Here, in the syntex, It is like Name of the mosfet, drain , gate, substrait , source. so, the meaning of the syntex is that, name of the mosfet is M1, Drain is connected to OUT node, Gate is connected to IN node, Substrait is connected to Vdd and the Source is connected to Vdd node. PMOS says the type of mosfet and the Width and lenth of channel is defined. similarly, For M2, syntex were written.

### SPICE simulation lab for CMOS

Tile we discribe the connectivity information about CMOS inverter only. Now we have to discribe connectivity information about other components also like source, capacitor etc. so, lets look into the other components.

First we discribe the load capacitor and then about the Vdd and Vin.

![image](https://user-images.githubusercontent.com/123365615/216557467-4d27e579-28f0-4ba5-8ae7-8c3217076d22.png)

Now, we have to give simulation command. which is about swiping the Vin from 0 to 2.5 with the steps of 0.05. Because we want Vout while changing the Vin.

![image](https://user-images.githubusercontent.com/123365615/216557505-ba776fbc-db88-4746-9b96-147188f3d50b.png)

The final step is to discribe the Model file.Model file contains all the details about PMOS and NMOS. from this file only we get the information about PMOS and NMOS.

![image](https://user-images.githubusercontent.com/123365615/216557568-c5b94aaa-b9da-4c81-b0a0-04df8f7a3491.png)

So, the total program is given below,

![image](https://user-images.githubusercontent.com/123365615/216557598-599f7c7d-27d5-46a6-910c-0337a1f74997.png)

Now, doing the simulation and get the graph like this,

![image](https://user-images.githubusercontent.com/123365615/216557632-5512cbcd-64ff-492c-9dcf-e4e7fa2a46e4.png)

Now, doing other simulation in which we change the PMOS width to 3 times of NMOS width. and after diong the simulation, we get the graph like this,

![image](https://user-images.githubusercontent.com/123365615/216557675-5c1306c9-7ac9-4e57-90d7-583854c71c69.png)

The difference between this two graph is that in the second graph the transfer charactoristic is lies in the ecxact middle of the graph where in the first graph it is lies left from the middle of the graph.

### Switching Thresold Vm

These both model of different width has their own application. By comparing this both waveform, we can see that the shape of the both waveform is same irrespective of the voltage level.

This thing tell us that when Vin is at low, output is at high and when Vin is at high, the output is at low. so the charactoristic is maintain at all kind of CMOS with different size of NMOS or PMOS. That is why CMOS logic is very widely used in the design of the gates.

Switching thresold, Vm (the point at which the device switches the level) is the one of the parameter that defined the robustness of the Inverter. Switching thresold is a point at which Vin=Vout.

![image](https://user-images.githubusercontent.com/123365615/216557755-14ad41aa-54f2-457c-8574-8710d8484da0.png)

In this figure, we can see that at Vm~0.9v, Vin=Vout. This point is very critical point for the CMOS because at this point there is chance that both PMOS and NMOS are turned on. If both are turned on then there is chances of leakage current(Means current flow direcly from power to ground).

By comparing this both the graph we can understang the concept of switching thresold voltage.

![image](https://user-images.githubusercontent.com/123365615/216557800-ed48e0a0-47a7-4a75-b3ea-64525e7482ea.png)

![image](https://user-images.githubusercontent.com/123365615/216557825-053a0a6c-e0f8-4e8d-a794-9988cb7b7fcf.png)

### Lab steps to git clone vsdstdcelldesign

To get the clone, copy the clone address from reporetery and paste in openlane terminal after the command "git clone". this will create the folder called "vsdstdcelldesign" in openlane directory.

image2

now, if we open the openlane directory, we find the vsdstdcelldesing folder in the openlane directory.

image3

Now if we goes in the vsdstdcelldesign folder and open it, we get the .mag file,libs file etc.

image4

now, let's open the .mag file and see that which layers are used to build the inverter. But before opening the mag file, we need tech file. so we will copy this file from this given below address,

image5

And do copy by "cp" command to the location which is given below,

image6

Now, we can see that this file is copied in the vsdstdcelldesign folder.

image7

Now, here to see the layout in magic, we don't need to write the whole address because we copy the tech file here.

Now, appying the magic comand like this,

image8

Now, we can see the layout of CMOS inverter in the magic like this,

image9

### Inception of layout CMOS fabrication process

#### create active region

##### (1) selecting a substrate

we select P-type silicon substrate with high resistivity(5~50 ohms) with moderate dopping and orientation is (100).

##### (2) creating active region for transister

First, we create the isolation layer by depositing the Sio2 layer (~40nm) on the substrate.

![image](https://user-images.githubusercontent.com/123365615/216558908-4b9d04fd-5d35-4133-b80d-e09ad3e8b40b.png)

Now, we are depisiting the Si3N4 layer (~80 nm) on the Sio2 layer.

![image](https://user-images.githubusercontent.com/123365615/216558957-ee3b1029-3afa-4734-82bf-277cea937879.png)

Now we do patterning by using depositing photoresist and using Mask 1 through UV light.

![image](https://user-images.githubusercontent.com/123365615/216559045-0fea0a4b-1316-4fa6-99ac-23fe6bb731a0.png)

Now, first we remove the mask and doing etching of Si3N4 layer on the exposed area.

![image](https://user-images.githubusercontent.com/123365615/216559077-51cc344e-fb12-4485-a45d-92f0b97dea38.png)

Now, next step is to remove photoresist by chamical reaction, because now to Si3N4 layer itslef behaves like good protecting layer for Sio2 layer. now, if we do LOCOS (local oxidation of silicon) process, the exposed sio2 part will gown and bird break also form. This grown sio2 will provide the perfect isolation between two PMOS and NMOS.

![image](https://user-images.githubusercontent.com/123365615/216559129-03b2f098-8907-4019-8b2c-150ec1eb0b83.png)

Next step is to etchout the Si3N4 layer by hot phosphoric acid.

![image](https://user-images.githubusercontent.com/123365615/216559171-aed34ff2-abb1-4be2-9fa1-50b189f92feb.png)

#### Formation of N-well and P-well

##### (3) N-well and P-well formation

we can not form P-well and N-well at a same time. we have to protect a region while forming one of the region by photoresist. And then using mask 2 and UV light, we will do patterning of photoresist to form P-well.

![image](https://user-images.githubusercontent.com/123365615/216559267-e359854c-94a6-49d1-bce9-2b5aefd75077.png)

Now, the area where we want to form the P-well is exposed. now we remove the mask and by applying the ion implantaton method (~200kev)to form P-well using Boron. But still it is P implant. After performing the high temparature anneling, it will become P-well.

![image](https://user-images.githubusercontent.com/123365615/216559311-045bceb8-e298-4f05-a6e2-1feda5a98e22.png)

We wiil do a similar process to form N-well by using mask 3 and using Phosphorus ions.

![image](https://user-images.githubusercontent.com/123365615/216559361-bcbfa95f-1783-4876-a806-47b64ec3ac58.png)

till now depth of wells are not define. so, by putting into the high temparature furnace (drive-in diffusion), we will define the depth of wells.

![image](https://user-images.githubusercontent.com/123365615/216559386-232ca1a3-0dda-45fc-b985-d900e7cacab7.png)

#### Formation of gate terminal

##### (4)Gate formation

Gate terminal is the most important terminal of the PMOS and NMOS because from the gate terminal only we can control the thresold voltage. doping concentration and oxide capacitance will control the thresold voltage.

so, first we are maintain the doping concentration here. for that we use mask 4 and again doing the ion implantation of boron ion at lower energy (~60kev).

![image](https://user-images.githubusercontent.com/123365615/216559569-0e3c203f-6719-469b-b0f8-f2c8ddd0c2c9.png)

same process we will repeat for N-well also by using mask 5 and Arsenic ion.

![image](https://user-images.githubusercontent.com/123365615/216559612-eed2c9aa-7a81-442f-8b7a-1c75cd9278e4.png)

Next step is that we have to fix the oxide layer. but before that we have to remove the oxide layer because this layer is got dammeged because of the privious processes. so,first we remove the layer using HF solution and again re-grown the high quality oxide layer with same thickness.

The final step is the deposition of polysilicon layer over oxide layer with more impurities for low resistance gate terminal.Then etched out this polysilicon layer by using mask 6 and photoresist.

![image](https://user-images.githubusercontent.com/123365615/216559649-aa037d2d-872b-474d-939f-f19eaac651c2.png)

After etching, remove the photoresist and gate terminal looks like,

![image](https://user-images.githubusercontent.com/123365615/216559695-9a6db850-10cf-4283-9edf-a4841bc39c27.png)

#### Lightly doped drain (LDD) formation

##### (5) LDD formation

Here, we actully want P+,P-,N doping profile in the PMOS and N+,N-,P doping profile for NMOS. Reason for that is

-	Hot electron effect
-	short channel effect
-	
For the formation of LDD, we again do ion implantation in P-well by using mask 7 and here we use phosphoros as a ion for light doping.

![image](https://user-images.githubusercontent.com/123365615/216559935-825783e3-6720-48e9-9e20-1a4c29ea142f.png)

Same process we will repeat for N-well. there we use mask 8 and BOron Ion.

![image](https://user-images.githubusercontent.com/123365615/216559979-eb6118c6-327c-4a7a-8011-31e21e9e7904.png)

Now, by creating the spacers, we can protect the actual structre remain constant of P-implantt and N-implant. For that we deposite a thick Sio2 or Si3N4 layer over the gate tereminal.

![image](https://user-images.githubusercontent.com/123365615/216560005-fcd9b09e-73ce-4f3f-a481-ef2480aae56f.png)

Now, we do Plasma anisotropic etching. By that side-wall spacers are formed.

![image](https://user-images.githubusercontent.com/123365615/216560036-fd8103d8-c205-47d8-bb85-3c78abb29e56.png)

#### Source and drain formation

##### (6)source-drain formation

Next step is deposite the very thin screen oxide layer to avoid the effect of channeling.

![image](https://user-images.githubusercontent.com/123365615/216560156-614a4453-a925-4779-aa38-166a2a47d508.png)

Now to form the drain and source, again we do the ion implantation of arsenic at 75kev to create the N+ implant by using mask 9 in the P-well to form PMOS.

![image](https://user-images.githubusercontent.com/123365615/216560183-36020071-e226-49f5-baec-67e3576e21e5.png)

Same process we will repeat for NMOS by using the mask 10 and boron ion in the N-well at 50kev to creat P- implant.

![image](https://user-images.githubusercontent.com/123365615/216560280-9118b404-a824-4347-aa71-e4be643a1996.png)

Now we put this Half made CMOS into the high temparature (1000 degree)anneling. So P+ implant and N+ implant now become the source and drain.

![image](https://user-images.githubusercontent.com/123365615/216560311-15c69a0a-ca4d-4048-a5b2-cb1888bf3ec4.png)

#### Local interconnect formation

##### (7)steps tp form contacts and local interconnects

First step is remove the thin screen oxide layer by etching. Then deposite the titanium (Ti) using sputtering. here Ti is used because Ti has very low resistivity.

![image](https://user-images.githubusercontent.com/123365615/216561014-eb78aada-76a2-4461-b9ed-9708a763efd1.png)

Next step is to create the reaction between Ti layer and source, gate, drain of CMOS. For that wafer is heated at about 650-700 degree temparature in N2 ambient for about 60 seconds. and after reaction, we can see the titanium siliside over the wafer. One more reaction is heppend there between Ti and N. and it results the TIN which is used for local communication.

![image](https://user-images.githubusercontent.com/123365615/216561072-54b6b892-d391-4aae-a5fc-4f56559310d5.png)

Now by using mask 11 and photoresist, we will etched out the TIN and make perticular contacts. TIN is etched out by using RCA cleaning.

![image](https://user-images.githubusercontent.com/123365615/216561132-44a5493b-b503-4ac7-9c41-b7d4798520b9.png)

Now, local interconnects are formed after etching and removing the photoresist.

![image](https://user-images.githubusercontent.com/123365615/216561180-0bb82e28-d66c-4f36-bcfc-99b328a2d79e.png)

#### Higher level metal formation

##### (8)Higher level metal formation

These steps are very semilar like previous steps. First thing that we are noticing is that the surface is non planner. it is not good to use this type of non planner serface for matel interconnects because of the problems regarding the metal disconinuty. so, we have to plannerize the surface by depositing the thick layer of sio2 with some impurity to make less resistive layer. and then we used CMP (chemical mechanical polishing) technique to plannerise the surface.

![image](https://user-images.githubusercontent.com/123365615/216561298-95cf1399-0b78-44bc-9a10-1ddb24a206e4.png)

Now using mask 12 and photorsist we etched the sio2 layer to diposite the metal in it.

![image](https://user-images.githubusercontent.com/123365615/216561333-7e869ff0-b4cd-40d7-b31c-3a65c3e69a2b.png)

Now remove the photoresist and seposite the thin later of TIN (~10nm) over the wafer. Because TiN is act as very good adession layer for sio2 and also act as a barrier between bottom layer and top layer of metal interconnects.

![image](https://user-images.githubusercontent.com/123365615/216561372-200aaf1d-8de6-43b1-8f26-3f4d491ef484.png)

Next step is to deposite the blanket tungsten (W) layer over the wafer. and then do the CMP here to plannerize the surface.

![image](https://user-images.githubusercontent.com/123365615/216561414-c42cc46f-7582-465c-881c-dbc9786d19bb.png)

This W is act as a contact holes and this holes needs to connect to the Higher metal layer. so we will deposite the Al (aluminium) layer.

![image](https://user-images.githubusercontent.com/123365615/216561467-dc44c280-4567-4ec2-afaf-bf6dd03208dd.png)

Then by using the mask 13 and photoresist, we etched the W layer out to form the contact at perticular place by Plasma etching.

![image](https://user-images.githubusercontent.com/123365615/216561498-5221b5a3-7225-40bd-b8ae-b05fbddd524c.png)

This is our first level of metal interconnets. now we again do the same process as above to deposite the second level of metal interconnect by using mask 14 for etched out the sio2 and using mask 15 for etched out Al leyer.

![image](https://user-images.githubusercontent.com/123365615/216561542-8779147b-9abb-4e55-92b4-95a81c9a6a4e.png)

The upper layer of Al is bit thicker as compared to lower layer of Al.Now, again deposite the layer of sio2 or si3N4 to protect the chip.

![image](https://user-images.githubusercontent.com/123365615/216561626-96623717-16cc-4909-916a-0f83344ec084.png)

And finally our CMOS is looks like this after the fabrication.

![image](https://user-images.githubusercontent.com/123365615/216561668-f6892d1d-96f5-44ff-9f9e-33b827ab9fd9.png)

### Lab introduction to Sky130 basic layer layout and LEF using inverter

image10

In sky130, every color is showing the different layer. here the firsst layer is for local interconnect shown by blue_perpel color, then second layer is metal 1 which is showm by light perple color, and the metal 2 is shown by pink color. N-well is showm by solide das line. green is N-diffusion region. and red is for polysilicon gate. similarly the brown color is for P-diffusion.

In tckon window, we can see that the selected area is NMOS and similarly we can chech PMOS also. and that is how we can check that the CMOS is working or not.

image11

semilarly we check for the output terminal also.(by double pressing "S" to select the entire thing at output Y).

image12

so, we can see that "Y" is attached to locali in cell def sky130_inv.

we can check the source of the PMOS is connected to the ground or not. and similarly we can check it for NMOS also.

### Lab steps to create std cell layout and extract spice netlist

To extract the file from here, we have to write the command in tckon window. and the comand is "extract all".

image13

Now let's go to this location from the terminal. it is exctracted.

image14

we will use this .ext file to create the spice file to be use with our ngspice tool. for that we have apply the comand "ext2spice cthresh 0 rthresh 0". this will not create anything new. now again we have to type "ext2spice" comand in tckon window.

image15

so, now we are checking the location and at there spice file has been created.

image16

let's see what inside the spice file by "vim sky130_inv.spice".

image17

### Sky130 Tech File labs

#### Lab steps to create final SPICE dexk using sky130 tech.

image18

here, we can see the all details about the connectivity of the NMOS and PMOS and about the power supply also.

X0 is NMOS and X1 is PMOS and both's connectivity is shown as GATE DRAIN SUBSTATE SOURCE.

But here the scale is 10000 um. but in Magic simulation, it is 0.01.

image19

SO, we are going to change the dimension here in the terminal. so any measurement will be in this scale of 0.01u. i.e., width=37*0.01u.

Now we have to include the PMOS and NMOS lib files. it is inside the libs folder in the vsdstdcellsdesign folder.

image20

so, now we include this file in the terminal by ".include ./libs/pshort.lib" and ".include ./libs/nshort.lib" comand.

And then set the supply voltage "VDD" to 3.3v by "VDD VPWR 0 3.3V" comand. and similarly set the value of VSS also.

Now, we need to specify the input files. by Va A VGND PULSE(0V 3.3V 0 0.1ns 2ns 4ns).

Also add the comand for the analysis like, ".tran 1n 20n", ".control" , "run",".endc",".end".

image21

after running this file we get output of ngspice like this,

image22

Now, ploting the graph here by comand, "plot y vs time a".

image23

### Lab steps to characterize inverter using sky130 model file

Here, we have to find value of 4 parameters.

-	rise time

it is time taken to the output waveform to 20% value to 80% value.

image24

so, rise time= (4.00742-2.51611)e-09 = 1.4913 nsec.

-	fall time

it is the time take by output for transition from 80% to 20%.

image25

so, fall time=0.08745 nsec.

-	propogation delay

it is the time difference between the 50% of input and 50% of the output.

image26

so, propogation delay = 0.60185 nsec.

-	cell fall delay

it is time for output falling to 50% and input is rising to 50%.

image27

so, cell fall delay= 0.0778 nsec.

# Day 10 -Pre-layout timing analysis and importance of good clock tree

## Timing modelling using delay tables
### Lab steps to convert grid info to track info
Till now we have done synthesis, floorplaning, placement and how to extract the spice out of it, done charecterization of it. till now, for placement and routing, we doesn't required any information about input/output port,logic path, power, ground.

Now the concept of 'lef' file will comes into the picture. it contains all the information which we discuss earlier. so our next objective is to extract the '.lef' file out of the '.mag' file. and next step is that extracted file could be placed into the picorv32a flow.

you need to folow certain guideline while making standerd cells.the guidelines are

-	The input and output port must lie on the intersection of the verticle and horizontal tracks
-	The width of the satnderd cell should be odd multiple of the track pitch and the height should be odd multiple of track verticle pitch

Now oprning the track file from the pdk/sky130/libs.tech /openlane/sky130_fd_sc_hd/track.info. where we get the info like this,

image28

so, the track is basically nothing but it is used during the routing stage.Rout will be go over the tracks. tracks are basically trases of matel layers.i.e., metal 1, matel 2, etc.

PNR is a automated. so we need to specified, where from we want routs to go. this specification is given by tracks. here we can see that each of the tracks are placed at (0.23 0.46)um horizontaly and (0.17 0.34)um vertically for li1, metal 1, metal 2 layer.

In layout, we can see that the ports are on the li1 layer. so we need to insure that the ports are on the intersection of the tracks or not. For that we have to cinvert the grid into the tracks. for that we have to first into the tracks file and the open the tckon window and type the "help grid" comand.

image29

Then again write comand according to the track file.

image30

Let's see the changes in the layout.

image31

Here we can see that, as per the guideline the ports are placed at the intersection of the tracks.

now, between the boundaries, there is 3 boxes are coverd. so our second requirment also satisfied here.

### Lab steps to convert magic layout to std cell LEF
here already the port name and the port definationa is seted, if not seted the we have to set the definanation and the name of the port.

As it parameters are set, we are ready to extract the 'LEF' file from the "mag" file. but before the extraction, let give the name to the cell by using tckon window.

Now we can see this file in the vsdstdcellsdesign folder.

image32

Now, we open this file in the magic by using comand "magic -T sky130A.tech sky130_vsdinv.mag &".

Now to extract the lef file we have to write the comand in the tckon window "lef write". so it will create a lef file and we can check it in the vsdstdcellsdesign folder.

image33

Now, lef file is created and now next step is plug this lef file in picorv32a. before that we move our files to src folder where all the design files are available at one location.

for that we are copiying this file in the src folder by 'cp' comand.

### Introduction to timing libs and steps to include new cell synthesis

The basic idea is that we have to include our custom cell into openlane flow. and the first stage in the openlane is the synthesis.

here, also we required library. so we copiying the library to the src folder.

Before we do anything, we have to modified our congig.tcl file. so opening this file by "vim" comand and modifiying it.

Now, start the new terminal and open the openlane by docker, make flow interactive and then add the package and then prep design with the privios run by the comand " prep -design pecorv32a -tag [last running time i.e.27-01_17-53] -overwrite".

Now comes the deciding part. we have to see that the synthesis run and its maps our custom vsd inveter into this flow. so, run the synthesis.

#### Introduction to delay tables

##### Power Aware CTS

If we make enable pin at logic '1' in the AND gate, the clock will propogatee to the AND gate. similarly, if we make enable pin at logic '0' in the OR gate, here also clock propogate to the OR gate.

Similarly if we make enable pin at logic 'o' in the AND gate, gate will block the clock and same this will happend with the OR gate if we make enable pin at logic '1'.

The advantage of this scenario is that, during this time period of the blocking the clocks, we can save lots of power in the clock tree. now the question is that how can we use this scenario in the clock tree.

let say we have a clocktree like this given below,

![image](https://user-images.githubusercontent.com/123365615/216563710-9bcbfcdd-844c-4263-b046-76e525f6bb6d.png)

Here we spitted the load of the 4 FF into the 2 buffers and the load of the 2 buffers is given to the level 1 buffer. here assumptions are,

-	Assume c1=c2=c3=c4=25fF
-	Assume Cbuf1=Cbuf2=30fF
-	Total Cap at node 'A'=> 60fF
-	Total Cap at node 'B'=> 50fF
-	Total Cap at node 'C'=> 50fF

We have done some observations here,

-	2 levels of buffering
-	At every level,each node driving same load
-	Identical buffer at same level

so, here capacitance at the every node of the clock tree is not the same. it is varying. Now load is varying then input transition is also verying because the output load at the level 1 buffer is the input of the other buffers of level 2. so, we have variety of the delays. To capture it, we have delay tables.

##### How delay tables are prepared?

To prepare the delay table, the perticukar element is taken out of the circuit and saparetly verying the input transition and output load and according to the variation, we will charactorize the delay of the element and make the delay table frpm it.

![image](https://user-images.githubusercontent.com/123365615/216564028-114f06ef-e522-44ce-b2f2-7776e9d50535.png)

##### Delay table usage part 1

Let's looks into the sample examples and making the table for Cbuf'1' and Cbuf'2',

![image](https://user-images.githubusercontent.com/123365615/216564074-fa859308-f054-4996-be63-4f1cfcbb2c16.png)

let's take practical example on the circuit. we take 40psec as input transition on the level 1 buffer and as per assumption load is around 60fF. and delay is comes between x9 and x10. lets take x9' as the delay of the buffer of level 1.

#####  Delay table usage part 2
Next step is to calculate the delay of the buffers of level 2. And after that we can find the letancy at the 4 clock end points.

now input transition is common for both the buffers. now assuming that the transition is around the 60psec and load at both the buffers is 50fF. so it will give the delay of y15.

The total delay from input to the output is= x9' + y15.(here we are ignoring the delay of the wires). that means the skew at the any output point is zero.

If load is not same at the every nodes, the skew will not be the zero.

Lab steps to configure synthesis settings to fix slack and include vsdinv
After synthesis, we observed that the slack is nagative here. wns(worst negative slack)= -24.89 and tns(total negative slack)= -759.

image34

let's do some modification here. for that opening the READme file from the /openlane/configuration/ less READme.md

Now lets try to make balance between area and the delay of the synthesis by changing the stratagy. comand for checking the current strategy is "echo $::env(SYNTH_STRATEGY)", and comand for changing the stategy is "set ::env(SYMTH_STRATEGY) 1". by doing this area will increase the little but but timing will improve.

Then checking the synth_bufferung and synth_sizing. if any one them is off then make it on by set the value of it by 1.

Till here we not get slack 0. to make slack 0, we ahve to write comand "set ::env(SYMTH_STRATEGY) DELAY 0"

After running synthesis we will get improved timing.

image35

Next command for run is :

set lefs [glob $::env(DESIGN_DIR)/src/*.lef]

add_lefs -src $lefs

Then again run the synthesis.

Timing analysis with ideal clocks using openSTA

### Setup timing analysis and introduction to flip-flop setup time

Timing analysis (with ideal clock)

we start with taking the ideal clock and we will do timing analysis for the ideal clock first.

Let's start the setup analysis with the ideal clock(single clock). specifications of the clock is

-	clock frequency =1 GHz
-	clock period =1 nsec

Let's take lainch Flop and Capture flop with clock. here clock tree is not built yet. so it is ideal scenario. here we have to do analysis between '0' and 'T'. with that assume that the delay of logic is 'θ'.

![image](https://user-images.githubusercontent.com/123365615/216564584-14362a9d-f3a9-482f-8d30-0b1e3df00dca.png)

Setup timing analysis says that θ<T. this condition should be neccessory for the the comninational logic work.

Now let's introduce the practical scenario here. Opening the capture flop and it has two mux inside it.

![image](https://user-images.githubusercontent.com/123365615/216564635-7e8e757d-d529-4d36-b626-251eed7192b5.png)

The way flop work, it will shown by the timing graph like this,

![image](https://user-images.githubusercontent.com/123365615/216564679-fa1fc55a-a7b8-4e92-9bc9-a8f67ab4e98a.png)

So, here mux 1 and mux 2 both have their own delay. these delay will restrict the combination delay to the requirment.

Hence finite time 's' required before clk edge for 'D' to reach Qm.

So, we can write that the internal delay of the MUX1 = set up time(S).

So, now θ<T becomes θ<(T-S).

### Introduction to clock jitter and uncertainty

Let's bring one more practical scenario here. clock is taking from the some clock source or PLL. So, because of some delay from the practical source of clock or PLL, clock pulse will not comes exacly at t=0 or at t=T. that in built variation of the clock is called jitter.

![image](https://user-images.githubusercontent.com/123365615/216564834-0e5694d0-5b6c-4e69-abc1-6dc78973376c.png)

lets consider this uncertantity time(US) in consideration. So, now equation becomes like, θ<(T-S-US). Now assuming that 'S'=0.01ns and 'US'=0.09ns. by taking that, lets identify the timing path for our existing scenario. in our circuit stage 1 and stage 3 logic path has single clock.

NOw, what we have to do is identify the combinational path delay for the given both logics.

![image](https://user-images.githubusercontent.com/123365615/216564876-a4a70af7-25d0-49e6-ab66-8a6b16d14a2e.png)

![image](https://user-images.githubusercontent.com/123365615/216564905-b045a8f9-8c3a-455a-b0b6-7360bf66d714.png)

#### Lab steps to configure OpenSTA for post-synth timing analysis.

when we do CTS, CTS is a stage where, we add clock buffers along with clockpath and build the clock tree. so, actually we are changing the netlist. Along the running CTS, actually the netlist file also created. so, after running the CTS, we will see the new Verilog file here.

Now, we see what is in the my_base.sdc file. 

image36

here, we can see the capacitor load and clock period and clock port etc.

Now, to reduce the fanout we use the command is : "set ::env(SYNTH_MAX_FANOUT) 4" and then run the synthesis.

but here also slack doesn't reduce.

Now next step is run floorplan, place IO, do global placement or detail placement and genrate pdn file the run the cts by following comands.

inut_floorplan

place_io

global_placement_or

detailed_placement

tap_decap_or

detailed_placement

After running the floorplaning and done the global placement we get positive slack.

image37

Then check the file which is created. Go to the placements folder under results and then invoke the magic tool and load the def file. The command is:magic -T /home/kunalg123/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &

image38

Clock tree synthesis TritonCTS and signal integrity

#### Clock tree routing and buffering uisng H-Tree algorithm

##### what is clock tree synthesis?

As shown in below, figure, let's connect clk1 to FF1 & FF2 of stage 1 and FF1 of stage 3 and FF2 of stage 4 with out any rules.

![image](https://user-images.githubusercontent.com/123365615/216565533-89f0a9fb-b6d3-4d2f-a924-9a8a73eb9f5d.png)

Now, let's see the problem with this clock Rout. let us say time required to reach the FF1 and FF2 of stage 1 are t1 and t2 for clock1. so, we can say that t2-t1= skew (ideally skew=o).

![image](https://user-images.githubusercontent.com/123365615/216565595-331437af-35b8-49b4-8bef-2e3403e97e6a.png)

To make, skew to be 0, this rout definatly not help. so, we can say that what we built the tree is "BAD TREE". so, the concept of H-tree comes out. with the Mid point strategy, H-tree form.

![image](https://user-images.githubusercontent.com/123365615/216565662-144d7470-dec6-4228-81a2-03db91e41273.png)

Next thing is clock tree synthesis (buffering).as we see in the clockk tree and we observed that clock has to travel through all wires and it will charge all capacitor which are comming in the path of this wire.

The problem occurse due to the charging the capacitor is signal inntigrity problem because of transition of signals. solution of this problem is add the repeaters here. the repaters are the similar as what we use in the data path but the main difference is, here repeater has equal rise and fall time.

![image](https://user-images.githubusercontent.com/123365615/216565735-3300c830-1d76-4dfa-889a-c41e88168532.png)

#### Crosstalk and clock net shielding

We build the clock tree in a manners that the skew becomes zero between launch Flop and capture flop. but if accedently any crosstalk heppense then everything what we had design is detoriated.

Let take the first clock net and protect it by shielding. here we protect the clock network from outside world. if the protection is not there then problems like glitch and delta delay is heppens.

The glitch is heppence because of Coupling capacitance between the wires.

![image](https://user-images.githubusercontent.com/123365615/216565890-1591136b-96da-4d38-b648-2c961649a8bc.png)

The shielding is the technique, by which we can protect the net from these problems. In a shielding, we put wire between the other teo wire where coupling capacitance is generate. This extra wire is grounded or connected to VDD.

Now, let's see about the delta delay.

[image](https://user-images.githubusercontent.com/123488595/215311259-ed24e431-674f-4573-9b11-6516296da86f.png)

#### Lab step to run CTS using TritonCTS

Now next step is CTS. for that write the command: "run_cts".

image39

In CTS stage the buggers are added in the paths. so, it will modifiying the netlist. so, if we go in the synthesis folfer and check the files, where cts.v file will avaiilable and this file contains the added buffers.

#### Lab steps to verify CTS runs

we have run CTS.Now before we goes into the post cts flow, we need to know that the actual meaning of the "run" command. This is the proc of tcl file. so, lets see, from where, openlane take this procs. for that we need to goes into the openlane and then goes into scripts and then check the "tcl_commands". in this file tcl commands are there for every stages what we have run till now. so let's see the "cts.tcl" file.

image40

here we can see the many procs are there. so here we can see the procs are run during the CTS run.

image41

so when we run the cts in the flow, basically it do this things.

first it run the tritonCTS by givin total information.

then it set the value of timer.

Then it goes for open the openroad.

if we check the openroad folder, at that directory "or.files" are available which was runs in the OPENROAD EDA tool.

Now les's check the "or_cts.tcl" file. inside this we can see the few switches.

image42

Now lets check what the library does. For this command is "echo 
::env(LIB_TYPICAL)"

Now checking the clock period of this by command: "echo $::env(SYNTH_MAX_TRAN)"

So, clock period seted as 2.473 nsec.

Then checking the max cap value, by command : "echo $::env(CTS_MAX_CAP)". and it is setted as 1.53169

Now checking the branch buffer cells by command :"echo $::env(CTS_CLK_BUFFER_LIST)". and these are the buffer cells are listed there "sky130_fd_sc_hd__clkbuf_1 sky130_fd_sc_hd__clkbuf_2 sky130_fd_sc_hd__clkbuf_4 sky130_fd_sc_hd__clkbuf_8".

And last cheching the root buffer by command: "echo $::env(CTS_ROOT_BUFFER)". So, we find that this "sky130_fd_sc_hd__clkbuf_16 " buffer is root buffer.

image43

Timing analysis with real clocks using openSTA

#### Setup timing analysis using real clocks

With real clock, circuit looks littel bit different then ideal clock. Here the bufferes and wires are added to the circuts.

Here, due to buffer, clock signals are not reaching the flop at t=0. it will reach at t=0+(delay of buffer 1 and 2).Now equation change to (θ+1+2)<(T+1+3+4).

![image](https://user-images.githubusercontent.com/123365615/216566264-8dd08bc7-7bae-4b0d-8b36-cd07de952e64.png)

Let's called "1+2"=∆1 and "1+3+4"=∆2 and (∆1-∆2)=skew

![image](https://user-images.githubusercontent.com/123365615/216566301-89e55494-5fc6-4e5a-9abf-6702a70b97a5.png)

And here also, we have to consider the propogation skew (s) and uncertainty delay (US). so final equaltion becomes like, (θ+∆1)<(T+∆2-S-US).

we can also say that (θ+∆1)= data arrival time and (T+∆2-S-US)=data required time.
If (Data required time)- (Data arrival time) = +ve then it is fine. If it is -Ve then it is called 'slack'.

##### Hold timing analysis
It is littel bit different then setup timing analysis. here we are sending the first pulse to the both launch FLop and capture flop.

Hold condition state that, Hold time (H)< combinational delay (θ). So, (θ>H).

![image](https://user-images.githubusercontent.com/123365615/216566429-6c1f9251-03cf-4e21-97b8-d543f55227aa.png)

Hence, finite time 'H' required for 'Qm' to reach Q i.e., internal delay of mux2= hold time.

Now, if we add the real time clock, the equation will be change. now equation becomes (θ+∆1)>(H+∆2).

i![image](https://user-images.githubusercontent.com/123365615/216566477-a92bbb4a-3bc8-4c68-9904-515cf2b00499.png)

#### Hold time analysis with real clock

Now here also adding the Unsetainty delay(UH) value due to jitter. so, equation becomes like, (θ+∆1)>(H+∆2+UH).

we can also say that (θ+∆1)= data arrival time and (H+∆2+UH)=data required time.

If (Data arrival time) -(Data required time)= +ve then it is fine. If it is -Ve then it is called 'slack'.

Now, applying all these things in out network.

![image](https://user-images.githubusercontent.com/123365615/216566551-f9321a5b-5cdd-46ba-a254-bdb157dd9234.png)

#### Lab steps to analyze timing with real clock using OpenSTA

let's open the OPENROAD tool in the flow by "openroad" command.

our objective is to do analysis of the clock tree.

we are analysin this in the OpenROAD because OpenSTA is already built in the OpenROAD. In OpenROAD the timing analysis is done in a different way. first we have to create "db" and "db" is created in a "lef" and "def" file.

Now let's create the DB. To create the DB, first we have to read the lrf file by comand "% read_lef /openLANE_flow/designs/picorv32a/runs/29-01_22-23/tmp/merged.lef".

Then we read the "def" file by command: "read_def /openLANE_flow/designs/picorv32a/runs/29-01_22-23/results/cts/picorv32a.cts.def".

NOw to create the DB write the command "write_db pico_cts.db"

NOW read this db file by command "read_db pico_cts.db"

then read the verilog file by applying the command "read_verilog /openLANE_flow/designs/picorv32a/runs/29-01_22-23/results/synthesis/picorv32a.synthesis_cts.v"

then read the library (max) by this command:"read_liberty -max $::env(LIB_FASTEST)".

similarly read the library (min) by this command: "read_liberty -min $::env(LIB_SLOWEST)".

Now read the sdc file by this command: "read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc"

now set the clocks by this command:"set_propagated_clock [all_clocks]"

there reports the checks by this command: "report_checks -path_delay min_max -format full_clock_expanded -digits 4".

so after running this we can see that the slack is positive for hold and setup both. and also we can notice the data required time and data arroval time also.

So, the Hold slack = 1.6982nsec because here we can see that (arrivel time) >(required time).

image44

NOw setup slack = 0.9457nsec because here we can see that (required time)>(arrival time).

#### Lab steps to execute openSTA with right timing libraries and CTS assignment

TritonCTS is right now built according to optimize fully according to one corner and we had bulid the clock tree for typical corner. and library also min and max. so we made tree according to typical corner but we analize it according to one corner. so, analysis become incorrect.

so, first we exits from the openroad by using "exit" command and we have to include the typical library for typical analysis. for that we have to open the "openroad" again and add this typical library. now we don't need to add lef and def file here. now commands for the adding a file is:

read_db pico_cts.db

read_verilog /openLANE_flow/designs/picorv32a/runs/29-01_22-23/results/synthesis/picorv32a.synthesis_cts.v

read_liberty $::env(LIB_SYNTH_COMPLETE)

link_design picorv32a

read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

set_propagated_clock [all_clocks]

report_checks -path_delay min_max -format full_clock_expanded -digits 4

slack for typical coirner= 0.2429nsec

image45

Now checking the branch buffer cells by command :"echo $::env(CTS_CLK_BUFFER_LIST)". and these are the buffer cells are listed there "sky130_fd_sc_hd__clkbuf_1 sky130_fd_sc_hd__clkbuf_2 sky130_fd_sc_hd__clkbuf_4 sky130_fd_sc_hd__clkbuf_8".

when openlane are making the CTS, at that time this buffers are place in the clock path to meet the skew value. and we always want skew value is maximum to the 10% of clock period.

# Day 5 -Final steps to build power distribution network

## Routing and design rule check(DRC)

### Introduction to Maze Routing A lee As Algorithm

Next stage in the physical design is the routing and DRC stage.

Routing. by the name it is says that rout means make physical contact between Din1 and FF1 od stage 1. but algorithm wise,it understand that Din1 is the source and FF1 input pin is the target. so, algorthm has to find the best possible solution to connect the Din1 and FF1.

For that we use Maze Routing and Lee's algorithm. let's try to connect Block 1 and block 2 of stage 3. there are varies mathod to connect these blocks but we need best solution for the rout or connection. To understand that, we remove all other things.

Algorithm create the routing gird at backend. here algirithm create two points. one is source and other is target. Now by using this routing grid, algorithm find the best way of routing. algorithm marks the adgecent grids of source. similarly again it will find the adgecent grids of the previos grids.similarly this process will runs continuosly.

![image](https://user-images.githubusercontent.com/123365615/216566964-4a9f768b-eb36-4dec-99f8-fd16ce51d84d.png)

#### lee's algorithm conclusion.

The extanding process of adgecent grids are contionuous till it reaches to the target.

![image](https://user-images.githubusercontent.com/123365615/216567026-606aa137-a90a-429a-8522-af792b1b3c0e.png)

there is many ways to reach the target. but the best possible way is 'L' shaped way.

![image](https://user-images.githubusercontent.com/123365615/216567054-c867ff09-b60d-4965-ab6b-befd54f7face.png)

lets take another example for this routing.

![image](https://user-images.githubusercontent.com/123365615/216567111-4d2443b8-9ea5-4061-ac44-ec72a54eee51.png)

Now, we reouted all the blocks and the circuits looks like this,

![image](https://user-images.githubusercontent.com/123365615/216567151-686506d0-a4ef-4a36-81a1-12e897790526.png)

#### DRC(Design Rule check)

While doing the routing, we need to follow certain rules for that. this is called DRC cleaning.

Lets take two parallel wires from the circuit for example,

![image](https://user-images.githubusercontent.com/123365615/216567231-6a12f00f-09b7-4bd6-a67e-d0596aa91a4c.png)

-	Rule 1

Width of the wire should be minimum that derived from the optical wavelenth of lithography technique applied.

![image](https://user-images.githubusercontent.com/123365615/216567360-df5a323c-f311-423d-acb6-999c2db25c44.png)

-	Rule 2

The minimum pitch between two wire shold be this much.

![image](https://user-images.githubusercontent.com/123365615/216567398-4ce2ec2d-559e-4e59-ad15-1b65c8c9d2c4.png)

-	Rule 3

The wire spacing between two wires should be this much.

![image](https://user-images.githubusercontent.com/123365615/216567434-51867326-5685-4a92-8a8b-71ab40c21067.png)

Let's take other part for understand the rules. basic problem in this types of wire is the signal short.

![image](https://user-images.githubusercontent.com/123365615/216567490-76c3b667-62b1-4210-bd59-23a9fea2dcbc.png)

Solution of this signal short problem is take one of the wire and put it on the other metal layer. usually upper metal is wider than the lower metal.

![image](https://user-images.githubusercontent.com/123365615/216567573-42abb479-673e-4474-aa52-b03a9e84f4aa.png)

After this solution, we add two new DRC rules should be check.

-	Rule 1

via width should be some minimum value.

![image](https://user-images.githubusercontent.com/123365615/216567695-c8c15c19-919c-4165-8a8b-9b0a625c1597.png)

-	Rule 2

Via spacing should be minimum this.

![image](https://user-images.githubusercontent.com/123365615/216567735-7aba3fc0-5af9-4de6-93fb-a5f1378b0390.png)

Next step is paracitic Extraction. so, the wire will get some resistance and capacitance value.

![image](https://user-images.githubusercontent.com/123365615/216567790-f411d8c9-b174-488b-8283-34962a296d3d.png)

### Power distribution network and routing

#### Leb steps to build power distribution network

IS in case, our terminal is delected by some cause, the if we want the previos terminal once again then this steps should be followed:

-	docker
-	./flow.tcl -interactive
-	package require openlane 0.9
-	prep -design picorv32a -tag [run file name i.e., 20-01_22-23]

Now to check the which was the last stage we perorm, the command is:"echo $::env(CURRENT_DEF)".

So, till now we have done CTS and now we are going to do the routing. but before routing we have to generate the PDN(power distribution network)file. for that command is: "gen_pdn".

image46

Here we can see the total number of nodes on the net VGND and it is also says that Grid matrix is created successfully. here total connection between all PDN nodes establish in the net VGND.

Now, till here we have picorv32a chip, and it needs the power. so it will get power from VDD and GND pads. From the pads, power goes to the tracks and from the tracks, the cells get power.

#### Lab steps from power straps to standerd cell power

To understan this, take an example here,

image47

In this figure, the green box is available is let say picorv32a chip. And the yellow, red and blue boxes which are the shown on the outside of the frame are the I/O pins and the power and ground pads. in this pads, the corner ones are called corner pads.

Red pads are the power pads and Blue pads are ground pads.

Power is transfered to the rings from the pads through Via which is shown by black dots on the cross section points of the ring and pads.

Now we need to insure that the power is transfered from the ring to the chip. for that we have vertiocal and horizontal tracks which are also shown by the red and blue color.

NOw, we need to supply power to the standerd cell (Which are shown by rectangular white boxes) from these tracks. this is done by horizontal small connections shown in the figure.

So, in a lab we have done this all the things till power distribution.

Now next and the final step is routing.

#### Basic and Global detailed routing and configure TritonRoute

Now current def.file is change to pdn.def from the cts.def. pdn.def file is now in the "runs/29-01_22-23/tem/floorplan/pdn.def".

In the routing process we are focusing on the routing strategy. there are 5 routing strategies are there. 0,1,2,3 and 14. routing is done in the TritonRoute engine. we have to specifies the strategy for the routing. for example if we ser the strategy to 14 then "TritonRoute14" strategy is used.

If we set the TritonRouting strategy to "0" then it want converge to a 0 TRC routing. but because of this we will improve in the memory requirement and run time. if we use TritonRoute14 then the run time will be approximate 1 hours. but in the TritonRoute0 it will be around the 30 minutes. here we use the "TritonRoute0". so in our flow first we check the routing strategy by the command "echo $::env(ROUTING_STRATEGY)". if it is "0" then it is fine, otherwise we have to change the strategy to "0".

Now the last thing remains is routing. for that command is :"run_routing".

The routing process is very complex.So, total routing is devided into two part.

-	Fast route (Global route)
-	Detailed route

![image](https://user-images.githubusercontent.com/123365615/216568192-36146d8f-13f8-43ec-a194-a2e311600ed7.png)

In the Global route, the routing region is devided into the rectangular grids cells as shown in the figure. and it is represented as 3D routing graph. Global route is done by FAST route engine.The detailed route is done by TritonRoute engine.

As shown in the figure, A,B,C,D are four pins which we want to connects through routing. and this whole image of A,B,C,D show the nets.

Now, the routing is successfully done.

image48

#### Tritinroute features

##### TritonRoute feature 1 -Honors pre-processed route guide

![image](https://user-images.githubusercontent.com/123365615/216568397-6be11e6b-bb7e-4647-a98a-0229bfb827d3.png)

-	It performs initial detail route.
-	It attempts as much as possible to route within route guides.

requierment of processed guides are 1)should have within unit lenth 2)should be in the preferred direction.

![image](https://user-images.githubusercontent.com/123365615/216568489-413d8be3-e563-46a7-8b2a-d60427721ec5.png)

-	Assumes route guides for each net satisfy inter-guide connectivity.
If two guides are connected then 1) they are on the same metal layer with touching edges. 2) they are on the neighbouring metal layers with a nonzero vertically overlapped area.
-	Assumes route guides for each net satisfy inter-guide connectivity.
#### TritonRoute feature 2 & 3 - inter-guide connectivity and intra-layer & inter -layer routing

Each unconnected terminal. i.e, poin of a standerd-cell instance should have its pin shape overlapped by a route guide.

![image](https://user-images.githubusercontent.com/123365615/216568716-66b2cf8c-e30d-4a71-90e4-8e63c9b79636.png)

Here we can see that black dots are pins of the cells and it is overlapped by route guide. if you have pins on the intersection of the vertical and horizontal tracks that will ensure that it will be overlapped by route guides.

#### Intra-layer parallel and inter-layer sequential panel routing

Intra layer means within the layers and inter layear means between the layers.


In this figure we can see the 4 layers of metal. each of these layers are devided in to the "--" lines. lets focus on metal 2 layer. here we assume the routing direction vertical. These "--" lines are called pannels. each pannels assigns the routing guides. here we can see the blue arrows. here routing is heppenes in the even index. it means that intra layer parallel routing. first it is heppenes in the even index and the it will heppen in the odd index. but it is heppening in the parallel in this perticular layer.

So, the (a) figure shows the parallel routing of panels on M2.In (b) figure, we can see the parallel routing of even panels on M3 and (c) shows the parallel routing of odd panels on M3.

#### TritonbRoute method to handle connectivity

-	INPUTS: LEF
-	OUTPUTS:detailed routing solution with optimized wore-length and via count
-	CONSTRAINTS:Route guide honouring, connectivity constraonts and design rukes

Now we have to defined the space where detailed routing take spaced.

#### Handling connectivity

![image](https://user-images.githubusercontent.com/123365615/216569007-3df511bc-49f0-445b-8173-a6185fab35bb.png)

To handle the connectivity, two concepts comes into the picture,

-	Access point:An on-gride point on the metal layer of the route guide, and is used to connect to lower-layer segments, upper-layer segments, pins or IO ports.
-	Access point cluster (APC): A union of all access points derived from same lower-layer segment,upper-layer guide, a pin or an IO port

Here in the figure shown above, the illustration of access points:

(a)To a lower-layer segment

(b)To a pin shape

(c)To upper layer

#### Routing topology algorithm and final files list post-route

![image](https://user-images.githubusercontent.com/123365615/216569184-0781b922-d32f-4efa-87cd-45f82f10b717.png)

The algorithm says that for each APCs we have to find the cost associated with it and we have to do minimum spaning tree betweem the APCs and the cost. finally the conclusion of the algorithm is that we have to find the minimul and the most optimal poits between two APCs.

Now, remaning things is the post routing STA analysis. for that the first goal is to extract the perasetic (SPEF). This SPEF extraction is done outside the openlane because openlane does not have SPEF extraction tool.

The .spef file can be found under the routing folder under the results folder.

image49

The following command can be used to stream in the generated GDSII file.

"run_magic"

Now the gds file will be generated and it is stored in the magic folder under results folder.

image50

And the generated layout is,

image51

All commands to run the openlane flow
docker

./flow.tcl -interactive

package require openlane 0.9

prep -design picorv32a

set ::env(SYNTH_STRATEGY) "DELAY 0"

run_synthesis

init_floorplan

place_io

global_placement_or

detailed_placement

tap_decap_or

detailed_placement

run_cts

gen_pdn

run_routing

run_magic

References



 


















































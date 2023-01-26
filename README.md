# miit_fdp
# Day 1 : Introduction to Verilog RTL design and Synthesis 
# Introduction to open-source simulator iverilog

RTL : Register-transfer level (RTL) is a representation at the abstraction level that expresses a synchronous digital circuit in terms of the flow of digital signals (data) between hardware registers along with the logical operations performed on those signals.Register-transfer-level abstraction is used in hardware description languages (HDLs) like Verilog and VHDL to create high-level representations of a circuit, from where we can derive lower-level representations. Simulator : It is a tool for checking whether our RTL design meets the required specifications or not. Icarus Verilog is a simulator used for simulation and synthesis of RTL designs written in verilog which is one of the many hardware description languages.
Design : It is the code or set of verilog codes that has the intended functionality to meet the required specifications.
Testbench : Testbench is a setup which is used to apply stimulus (test_vectors) to the design to check it's functionality.It tests whether the design provides the output that matches the specifications.The RTL Design gets instantiated in the testbench.

![image](https://user-images.githubusercontent.com/123365615/214894163-9bbe8ed4-35af-40a5-b431-e2a49a9379bd.png)

# Iverilog Based Design Flow :
1. The iverilog simulator takes RTL design and Testbench as inputs.
2. It produces a VCD file(Value change dump format) as output. Only changes in the input are dumped to changes in the output.
3. We use Gtkwave to see these output changes graphically.

# Labs using iverilog and gtkwave

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

![pic 5](https://user-images.githubusercontent.com/123365615/214267910-d28b349e-49be-4949-ad5f-832c71f0de2e.PNG)

 4. We can also view our Verilog RTL design and testbench code using

          gvim tb_good_mux.v -o good_mux.v

![pic 6](https://user-images.githubusercontent.com/123365615/214267970-0e58d783-ae22-47da-97e1-dd832577626d.PNG)

Add vcode and testbench

# Introduction to Yosys and Logic synthesis
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

# WHAT HAPPENS DURING SYNTHESIS? During Logic Synthesis an RTL code is transformed into a gate level netlist.

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

# Understanding .lib
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
# But why do we need different flavours of gate? Consider the below circuit:

![image](https://user-images.githubusercontent.com/123365615/214899581-9c8bf3a9-3209-4400-a7e3-95b626737a74.png)

# Setup time Analysis
It should take 1 CLK cycle for the signal to propagate from the Launch DFF-A to the Capture DFF-B through the combimational circuit.
While this propagation all delays : Propagation delay of DFF A (Tcq-a)+ Propagation delay of combinational circuit(Tcomb)+Setup time of DFF-B(Tsetup_b). Thus ,the constraint becomes:

        Tclk>Tcq-a+Tcomb+Tsetup_b

Setup time is the time for which data should arrive before the launch clock edge to be reflected at the output reliably.
For high performance we need high speed gates.So, The frequency of a gates should be high or the Tclk should be less . So all the above mentioned delay should be less. We need cells that work fast to make the combinational small.

# Hold up time Analysis
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

#How do we select cells?

 - We need to guide the synthesizer to select the flavour of cells for optimum implementation of our logic circuits.
 - More use of faster cells results in bad circuit in terms of area and power and potentially may result in hold time violations.
 - More use of slower cells may result in a sluggist circuit and may not meet the performance. It is required for us to offer guidance to the synthesizer to pick correct set of cells .This guiding parameters to the synthesizer are called as CONSTRAINTS.

# Labs using Yosys and Sky130 PDKs

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

# Commands to write a netlist

        write_verilog -noattr good_mux_netlist.v

        !gvim  good mux_netlist.v

Note: We use the switch -noattr i.e. no attributes for a more simplistic view of our netlist as shown below

![pic 11](https://user-images.githubusercontent.com/123365615/214268277-f7e77afe-d4b8-464c-8867-cbacade01b1d.PNG)



# DAY2 : TIMING LIBS, HIERARCHICAL Vs FLAT SYNTHESIS AND EFFICIENT FLOP CODING STYLES

        gvim ../my_lib/lib/SKY130_fd_sc_hd__tt_025C_1v80.lib

![c1](https://user-images.githubusercontent.com/123365615/214469650-4432376c-9e3a-4f40-8df9-e02774e739b7.PNG)

#  off the syntax color red

  Command used : syn off
  
# Enabling the line numbers

  Command used : se nu
  
# The voltage process and temperature conditions are also specified.

![c2](https://user-images.githubusercontent.com/123365615/214469704-904e2ed5-c157-47fc-b5dc-0265b810741b.PNG)

![c3](https://user-images.githubusercontent.com/123365615/214469737-5271f59a-2394-4cba-9579-f5fc043be768.PNG)

# The lib contains different flavors of this same as well as different types of cells.

![c4](https://user-images.githubusercontent.com/123365615/214470489-daab5614-5a75-4e03-8fb7-d2eef07613b5.PNG)

![c5](https://user-images.githubusercontent.com/123365615/214470914-b589af3b-85ee-4182-a344-ffc5f74053b3.PNG)


# The library also represents the different features of the cell like its leakage power,the various input's combinations and the operations between them.

![C6](https://user-images.githubusercontent.com/123365615/214477178-26d9a54f-40cd-4dfa-ab3f-a84694e3722e.PNG)

![C7](https://user-images.githubusercontent.com/123365615/214477409-7a126f53-84ae-4b33-b98c-a9fde7ef3b39.PNG)

![C8](https://user-images.githubusercontent.com/123365615/214478703-e0499855-643e-46f0-a511-03005fbc64fb.PNG)

# HIERARCHIAL VS FLAT SYNTHESIS

vim multiple_modules.v

![c9](https://user-images.githubusercontent.com/123365615/214479914-b901e696-26eb-447e-8e93-44cff5c7422c.PNG)

yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog multiple_modules.v

synth -top multiple_modules

![c10](https://user-images.githubusercontent.com/123365615/214482340-20b879ab-d13f-4f9d-8d4f-32a8bd3484c9.PNG)

![c11](https://user-images.githubusercontent.com/123365615/214482371-a2b8f37c-9294-4693-88ca-bf2c548ee748.PNG)

show multiple_modules

![c12](https://user-images.githubusercontent.com/123365615/214482920-55f2dfaa-c060-4642-b608-3375f217ebbb.PNG)

write_verilog multiple_modules.hier.v

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 

!vim multiple_modules.hier.v 

flatten
show

![c13](https://user-images.githubusercontent.com/123365615/214483629-bab5ae03-a318-4c14-a046-d617db135419.PNG)


![c14](https://user-images.githubusercontent.com/123365615/214485987-9eeadce9-f389-4ae2-b49e-79e9b2ae08b6.PNG)


![c15](https://user-images.githubusercontent.com/123365615/214489124-ee75cd60-3435-4acf-8634-631ffbf1580d.PNG)

# GLITCHES

# Asynchoronous and Synchronous resets

![c16](https://user-images.githubusercontent.com/123365615/214490495-78aa07a6-0804-4d94-bd80-a871ce710f0f.PNG)

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

!vim dff_asyncres.v -o dff_async_set.v 

./a.out

gtkwave tb_dff_asyncres.vcd

![c17](https://user-images.githubusercontent.com/123365615/214492166-21a1245d-ed4d-44aa-85b2-1123a838ef60.PNG)

![c18](https://user-images.githubusercontent.com/123365615/214492195-5b473fb1-0537-4f73-85b9-fbac4c20e179.PNG)

# Synthesis implementation results :

# asynchoronous reset :

![image](https://user-images.githubusercontent.com/123365615/214512361-aca28e93-0901-4a53-8923-72d01c9c63f4.png)

# asynchoronous set :

![image](https://user-images.githubusercontent.com/123365615/214512486-1eaf8094-db86-4215-aa98-5ba78692cb45.png)

# Synchronous Reset and set : 

![C19](https://user-images.githubusercontent.com/123365615/214514170-8116a50d-af67-4c75-a2e6-2f8a61de10f7.PNG)


# Synthesis Results:

![image](https://user-images.githubusercontent.com/123365615/214513823-68d5f342-cab2-4359-af3a-3105949d128a.png)


# RTL CODE:

![image](https://user-images.githubusercontent.com/123365615/214513880-21584d67-18dd-4fe7-9432-59570fa9964a.png)


# Synthesis Results:

![image](https://user-images.githubusercontent.com/123365615/214513923-b23ae7a0-2305-4264-9802-ef76258ba4e5.png)

# OPTIMISATIONS

![C20](https://user-images.githubusercontent.com/123365615/214514537-bfd5be37-c759-42c5-aaed-19ca24f8e568.PNG)

![image](https://user-images.githubusercontent.com/123365615/214514618-e804157a-58e5-45c3-9bba-f459f136847e.png)

![image](https://user-images.githubusercontent.com/123365615/214515256-1f9783c8-9215-4a03-b7d3-e675c239143d.png)

# DAY 3 : Combinational and Sequential Optimisations

# Combinational Logic Optimisations

# Example 1: opt_check.v

vim opt_check.v

![a0](https://user-images.githubusercontent.com/123365615/214607368-b60555a2-dedb-416a-8429-7945c2572304.PNG)

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog opt_check.v

synth -top opt_check

opt_clean -purge

![a1](https://user-images.githubusercontent.com/123365615/214541758-cf805f8b-fc9a-4f52-a4f0-d6f0024dd911.PNG)

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

write_verilog -noattr opt_check_netlist.v 

show

![a2](https://user-images.githubusercontent.com/123365615/214564585-9bf2d5b2-ea87-4298-b1fd-bd042c96e0b9.PNG)

# Example 2: opt_check2.v

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

![a3](https://user-images.githubusercontent.com/123365615/214611377-d77f5fb9-d027-4d36-a896-bc55314809ec.PNG)

# Example 3:opt_check3.v

vim opt_check3.v

![a02](https://user-images.githubusercontent.com/123365615/214609125-790d24a7-931c-42b9-9407-0544ac923d65.PNG)

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog opt_check3.v

synth -top opt_check3

opt_clean -purge

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

write_verilog -noattr opt_check3_netlist.v 

show

![a4](https://user-images.githubusercontent.com/123365615/214565792-aa70ae43-0b33-433f-86c8-fa69fde12daf.PNG)


# Example 4: opt_check4.v

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

![a5](https://user-images.githubusercontent.com/123365615/214567264-9ddad9b2-c009-4f04-8f6c-4205224b445e.PNG)

# Example 5: multiple_module_opt.v

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

![a8](https://user-images.githubusercontent.com/123365615/214621734-42f4972e-7973-4e9b-9758-0c03ba0fac98.PNG)

# Example 6: multiple_module_opt2.v

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

![a9](https://user-images.githubusercontent.com/123365615/214624095-99f4d7e2-69c4-4a00-bfbf-d96a6f108d71.PNG)

# Sequential Logic Optimisations

# Example 1: dff_const1.v

vim dff_const1.v

![s1](https://user-images.githubusercontent.com/123365615/214625720-3c82b6fb-b609-4e95-ab58-4538d48f9a1e.PNG)

iverilog dff_const1.v tb_dff_const1.v

./a.out

gtkwave tb_dff_const1.vcd

![s5](https://user-images.githubusercontent.com/123365615/214633091-69cc4cbe-a229-44ba-aebb-3f4e5ce4acaa.PNG)

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog dff_const1.v

synth -top dff_const1

dfflibmap -liberty ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib 

write_verilog -noattr dff_const1_netlist.v 

show

![s6](https://user-images.githubusercontent.com/123365615/214635875-20af3330-ae11-47c5-a9da-fc107468cd35.PNG)

# Example 2: dff_const2.v

vim dff_const2.v

![s2](https://user-images.githubusercontent.com/123365615/214628722-4ab54c77-6bd8-4180-99c9-6def16c0a917.PNG)

iverilog dff_const2.v tb_dff_const2.v

./a.out

gtkwave tb_dff_const2.vcd

![s8](https://user-images.githubusercontent.com/123365615/214639650-b0aed62f-e315-42f1-90ee-c8529d96c710.PNG)

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog dff_const2.v

synth -top dff_const2

dfflibmap -liberty ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib 

write_verilog -noattr dff_const2_netlist.v 

show

![s7](https://user-images.githubusercontent.com/123365615/214638439-e8913f4c-e388-47a2-a417-29dc11a71b9c.PNG)

# Example 3: dff_const3.v

vim dff_const3.v

![s3](https://user-images.githubusercontent.com/123365615/214628873-2fe56a6e-4035-4c4e-8475-c4df4c294c5d.PNG)

iverilog dff_const3.v tb_dff_const3.v

./a.out

gtkwave tb_dff_const3.vcd

![s9](https://user-images.githubusercontent.com/123365615/214641311-237fa492-dc26-4f3d-81cf-bf8a8d5d5d98.PNG)

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog dff_const3.v

synth -top dff_const3

dfflibmap -liberty ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib 

write_verilog -noattr dff_const3_netlist.v 

show

![s11](https://user-images.githubusercontent.com/123365615/214643434-aa3f2864-499b-4b4b-81e9-07f0e17613db.PNG)

# Example 4: dff_const4.v

vim dff_const4.v

![s4](https://user-images.githubusercontent.com/123365615/214628960-adb1600f-3ae4-431a-ac71-22f14e4a964f.PNG)

iverilog dff_const4.v tb_dff_const4.v

./a.out

gtkwave tb_dff_const4.vcd

![s10](https://user-images.githubusercontent.com/123365615/214642332-c4b552be-da6a-4f14-a19f-6c1ece260fbd.PNG)

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog dff_const4.v

synth -top dff_const4

dfflibmap -liberty ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib

abc -liberty ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib 

write_verilog -noattr dff_const4_netlist.v 

show

![s12](https://user-images.githubusercontent.com/123365615/214644050-6c26ea6e-2fae-498b-8f6f-2912f5f702e9.PNG)

# Day 4: Gate Level Simulations,Blocking vs Non Blocking assignments,Synthesis-Simulation Mismatch

# Example 1: A mux designed with the help of ternary operator

vim ternary_operator_mux.v

![f1](https://user-images.githubusercontent.com/123365615/214810313-125798d0-fffc-4c81-9fae-e9e85f75ddb5.PNG)



# Example 2: bad_mux.v

vim bad_mux.v

![f3](https://user-images.githubusercontent.com/123365615/214887908-00428702-c357-4d64-8697-db08dc22859b.PNG)


# Example 3: blocking_caveat.v

vim blocking_caveat.v

![f4](https://user-images.githubusercontent.com/123365615/214887961-7175fe34-905a-4c5e-bcf8-afb222bb5708.PNG)









































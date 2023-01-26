# miit_fdp
# Day 1 : Introduction to Verilog RTL design and Synthesis 

# Labs using iverilog and gtkwave

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
 
 ![pic1](https://user-images.githubusercontent.com/123365615/214267684-e71c0ed4-e9ca-4c5c-a034-1d92c7c23d7b.PNG)

 ![pic 2](https://user-images.githubusercontent.com/123365615/214267577-b00347a0-7829-49c1-b9d9-7a3aca0166f2.PNG)
 
 iverilog good_mux.v tb_good_mux.v

![pic 3](https://user-images.githubusercontent.com/123365615/214267764-316bdd57-075f-4634-b25f-0b774102ea65.PNG)

./a.out

gtkwave tb_good_mux.vcd

![pic 4](https://user-images.githubusercontent.com/123365615/214267848-6f917d3e-4cf7-4280-a218-d406e7cd3bfb.PNG)

![pic 5](https://user-images.githubusercontent.com/123365615/214267910-d28b349e-49be-4949-ad5f-832c71f0de2e.PNG)

gvim tb_good_mux.v -o good_mux.v

![pic 6](https://user-images.githubusercontent.com/123365615/214267970-0e58d783-ae22-47da-97e1-dd832577626d.PNG)

Labs using Yosys and Sky130 PDKs

yosys

read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog good_mux.v

synth -top good_mux

abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show

![pic 7](https://user-images.githubusercontent.com/123365615/214268038-cdb72106-c621-4449-a791-247138b61229.PNG)

![pic 8](https://user-images.githubusercontent.com/123365615/214268103-8f3664c3-f5f7-4d59-b534-f74146f3ae55.PNG)

![pic 9](https://user-images.githubusercontent.com/123365615/214268179-d188de38-476c-4408-a722-99d6c517fafb.PNG)

![pic 10](https://user-images.githubusercontent.com/123365615/214268357-011b448c-11cb-4a30-88ea-01e791dec386.PNG)

write_verilog -noattr good_mux_netlist.v

!gvim  good mux_netlist.v

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

iverilog ternary_operator_mux.v tb_ternary_operator_mux.v

./a.out

gtkwave tb_ternary_operator_mux.vcd

![f2](https://user-images.githubusercontent.com/123365615/214814123-4ad085f5-b652-4664-8a30-b2bb5aa2e225.PNG)

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

read_verilog ternary_operator_mux.v

synth -top ternary_operator_mux

abc -liberty ../lib/sky130_fd_sc_hd_tt_025C_1v80.lib 

write_verilog -noattr ternary_operator_mux.v

show







































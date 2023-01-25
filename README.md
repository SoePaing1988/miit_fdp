# miit_fdp
Day 1 : Introduction to Verilog RTL design and Synthesis 

Labs using iverilog and gtkwave

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

DAY2 : TIMING LIBS, HIERARCHICAL Vs FLAT SYNTHESIS AND EFFICIENT FLOP CODING STYLES

gvim ../my_lib/lib/SKY130_fd_sc_hd__tt_025C_1v80.lib

![c1](https://user-images.githubusercontent.com/123365615/214469650-4432376c-9e3a-4f40-8df9-e02774e739b7.PNG)

Switching off the syntax color red

  Command used : syn off
  
Enabling the line numbers

  Command used :se nu
  
The voltage process and temperature conditions are also specified.

The lib contains different flavors of this same as well as different types of cells.

![c2](https://user-images.githubusercontent.com/123365615/214469704-904e2ed5-c157-47fc-b5dc-0265b810741b.PNG)

![c3](https://user-images.githubusercontent.com/123365615/214469737-5271f59a-2394-4cba-9579-f5fc043be768.PNG)






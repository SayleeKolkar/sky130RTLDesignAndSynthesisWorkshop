# sky130RTLDesignAndSynthesisWorkshop












Day-2: Timing Libs,Hierarchical vs flat synthesis and efficient flop coding style

Part-1:Introduction to Timing .libs
      
Subpart-1:Introduction to Dot Lib 
        
Subpart-2:Introduction to Dot Lib 

Subpart-3:Introduction to Dot Lib 

Part-2:Hierarchical vs Flat Synthesis

Subpart-1:Hier synthesis flat synthesis

Subpart-2:Hier synthesis flat synthesis

To know about the hier synthesis and flat synthesis we open the multiple modules file.
To open this file we use the command "gvim multiple_modules.v"
It shows the program of multiple_modules program.

![image](https://user-images.githubusercontent.com/104482957/165769676-3af29f16-614b-416c-a415-4f9459c4c496.png)

To synthesize the multiple module we have to read the library using command "read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib"

"read_verilog multiple_modules.v" to read the design.

after getting program succesfully finished and no error should be present.like this

![read_verilog mul m](https://user-images.githubusercontent.com/104482957/165774985-9fb74c8d-8abb-4987-8073-cc3d448fd08c.png)

use "synth -top multiple_modules" to synthesize the top module

we get the info about submodule 1 & 2 and no. of gates used in it.

![synth1](https://user-images.githubusercontent.com/104482957/165776717-3a0db459-92ea-4917-ae39-e3e196f233ee.png)
![synth2](https://user-images.githubusercontent.com/104482957/165776726-adf6c09a-3a76-4169-9980-62c07d96bae7.png)

to link design with library use "abc -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib"

to realize the logic use commmand "show multiple_modules"

![mm show](https://user-images.githubusercontent.com/104482957/165786658-2266580e-8535-48d4-90d5-517df844a28e.png)

to write the netlist " write_verilog -noattr multiple modules_hier.v" this command use for the heirarchical.

"!gvim multiple modules_hier.v" to see the netlist.

![mm netlist](https://user-images.githubusercontent.com/104482957/165790766-0448e9a4-c0d7-4307-832c-f75ab40c379d.png)

here we see hierarchies are preserved.

Here we are getting nand gate with inverter instead of or gate.cells are designed to have similar drive strength of pull up and pull down structures to have comparable rise and fall time. NAND gate has better ratio of output high drive and output low drive as compared to NOR gate. Hence NAND gate is preferred over NOR.

Subpart-2: Hier synthesis flat synthesis 

flatten is command to write flat netlist.

"write_verilog multiple_modules_flat.v"
flatten show is look like this using "show" command

![flatten show](https://user-images.githubusercontent.com/104482957/165815322-b605cbe7-edc8-4e3f-8e68-ce8c8d4283fa.png)

netlist is look like this.

![mm flat netlist](https://user-images.githubusercontent.com/104482957/165812641-f64a3deb-6b73-4b0d-be0e-b262650b6195.png)
hierarchies are flattened out from here.








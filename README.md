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

For synthesization of submodule we read the library first.

"read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib"

read the design usiing command "read_verilog multiple_modules.v"

sythesize the submodule using command "synth -top sub_module1"

we get result like this. It just infer submodule1 from the multiple modules.

![synth submodule1](https://user-images.githubusercontent.com/104482957/165882697-ab453bac-08e2-478d-aec3-b74d083cce06.png)

link the design using "abc -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib"

![abc submodule1](https://user-images.githubusercontent.com/104482957/165883434-be691784-0530-4b6b-b570-0e8fd7a58b24.png)

find the result of submodule1 only.use command "show"

![show submodule1](https://user-images.githubusercontent.com/104482957/165883596-239568dd-69ff-4b5f-b87f-13c7f815eee8.png)

When we have multiple instance of same module or we have massive design, then the tool is not working properly.so we can give the some small portions one by one to getting the good netlist.so we can do this using synth -top <module name> to control the required module synthesize.
      
Part-3: Various Flop Coding Style And Optimization
      
 Subpart-1.1: Why Flops and Flop coding styles  
      
 A glitch is any unwanted pulse at the output of a combinational gate. A glitch happens generally, if the delays to the combinational gate output are not balanced.To avoid the glitch, put a flip flop after the output of combinational logic because we want to store that element.so we use flop. 
      
 Subpart-1.2: Why Flops and Flop coding styles 
      
D flip-flops can have asynchronous reset, which can be independent of the clock. Regardless of the clock, the reset can change the output Q to zero, which can cause asynchronous output.In asynchronous reset the Flip Flop does not wait for the clock and sets the output right at the edge of the reset.The Asynchronous implementation is fast, as it does not has to wait for the clock signal to be applied.
      
Synchronous resets are based on the premise that the reset signal will only affect or reset the state of the flip-flop on the active edge of a clock. The reset can be applied to the flip-flop as part of the combinational logic generating the d-input to the flip-flop.The coding style to model the reset should be an if/else priority style with the reset in the if condition and all other combinational logic in the else section.      

![async sync reset](https://user-images.githubusercontent.com/104482957/165895354-e30318fd-ac05-4052-acef-8e72b78d1cd0.png)

 Subpart-2.1: flop synthesis simulations
      
 For simulation use "iverilog dff_asyncres.v tb_dff_asyncres.v" command.
      
"./a.out" to execute this file. it will dump into vcd file.
      
![asyncres](https://user-images.githubusercontent.com/104482957/165898757-68fca8c7-8df4-42ef-bb52-72c006f01678.png)
  
use "gtkwave tb_dff_asyncres.vcd" for waveforms.
      
![asyncres wave](https://user-images.githubusercontent.com/104482957/165900373-de333f9f-263d-4da7-95ea-38beb36bdd29.png)
      
for asynchronous set simulation use "iverilog dff_async_set.v tb_dff_async_set.v" command.      

"./a.out" to execute this file. it will dump into vcd file.

![async set](https://user-images.githubusercontent.com/104482957/165933965-3cf0f306-ded2-4051-aa9f-8fd092ef2690.png)
 
use "gtkwave tb_dff_async_set.vcd" for waveforms.      
      
![async set wave](https://user-images.githubusercontent.com/104482957/165935849-e23c4446-4f1e-4595-b5f4-c2ae7c0cc346.png)

for synchronous reset "iverilog dff_syncres.v tb_dff_syncres.v" command.
      
"./a.out" to execute this file. it will dump into vcd file.      

use "gtkwave tb_dff_syncres.vcd" for waveforms.
      
![syncres wave](https://user-images.githubusercontent.com/104482957/165938481-1ef1bd95-9949-4fe3-b195-009955061265.png)
      
Subpart-2.2: flop synthesis simulations 
      
For synthesization "read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib"

read the design using command "read_verilog dff_asyncres.v"

sythesize the asynchronous reset using command "synth -top dff_asyncres"
      
![async synth](https://user-images.githubusercontent.com/104482957/165943160-585fbb36-2142-43f0-a421-77c1fe5c4aa9.png)

link the design using "abc -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib"

![abc asyncres](https://user-images.githubusercontent.com/104482957/165947301-8f308da8-8786-438a-b893-50b3d212db45.png)
      
 use command "show"
      
![show asyncres](https://user-images.githubusercontent.com/104482957/165947689-6b2c8a33-7d0d-4280-803c-95f7523b9a37.png)
      
for the synthesization asynchronous set.     
read the design using command "read_verilog dff_async_set.v"

sythesize the asynchronous set using command "synth -top dff_async_set"
      
![async set synth](https://user-images.githubusercontent.com/104482957/165949175-d882f201-0cd4-4012-a06b-ea43d42a9d42.png)
      
To see only dff files "dfflibmap -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib"  
 
link the design using "abc -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib"
      
use command "show"
      
![async sets show](https://user-images.githubusercontent.com/104482957/165951528-424c14e9-c96f-43d5-afad-a41fa4796514.png)
      
for the synthesization synchronous reset.     
read the design using command "read_verilog dff_syncres.v"

sythesize the synchronous reset using command "synth -top dff_syncres"  
      
![syncres synth](https://user-images.githubusercontent.com/104482957/165952783-5003c771-b6fa-48fd-befd-db9a626fe2fc.png)

To see only dff files "dfflibmap -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib"  
 
link the design using "abc -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib"
      
use command "show"
      
![sync res show](https://user-images.githubusercontent.com/104482957/165955569-37dee941-dab7-4928-86c7-053281e6e4cf.png)
      
Subpart-3.1: Interesting optimisations 
      
for the synthesization mult2  "read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib"
      
read the design using command "read_verilog mult_2.v"

sythesize the mul2 using command "synth -top mul2"
      
![synth mul2](https://user-images.githubusercontent.com/104482957/165958984-bc8cfca4-bbd3-4861-accd-221acaa57313.png)
      
Cells are not present so no need to mapping.
      
use command "show"
      
![mul2 show](https://user-images.githubusercontent.com/104482957/165959511-a73480c4-3e08-4a59-8285-e35566e7d4b7.png)

Subpart-3.2: Interesting optimisations 
      
to write the netlist using command "write_verilog -noattr mul2_net.v" 
      
"!gvim mu2_net.v"
      
![gvim mul2](https://user-images.githubusercontent.com/104482957/165963250-e96da5f8-3f75-4056-bad1-b7013940dd3a.png)

for the synthesization mult  "read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib"
      
read the design using command "read_verilog mult_8.v"

sythesize the mul8 using command "synth -top mul8"     
      
![synth mult8](https://user-images.githubusercontent.com/104482957/166088746-435755c8-18f8-4943-9f22-b7c96a514905.png)
      
Cells are not present so no need to mapping.
      
use command "show"
      
![show mul8](https://user-images.githubusercontent.com/104482957/166088927-b9652f7f-03ae-4a1b-a455-c4a5eefb0c99.png)

To write the netlist using command "write_verilog -noattr mult8_net.v" 
      
"!gvim mut8_net.v" to see the netlist.
      
![mult8 netlist](https://user-images.githubusercontent.com/104482957/166089120-0edd3a74-f614-4f62-91ad-4395c178c501.png)
      
 Day - 3: combinational and sequential Optimization
      
 Part-1: Introduction to Optimization
    
 There are two types of optimization 1. combinational logic 2. sequentional logic
      
 1.combinational logic optimization: squeezing the logic to get optimised design.it is depend on area and power.
      
 Techniques used for optimization :- 1.constant prapogation(direct optimization)  2.Boolian logic optimization
      
 2.sequentional logic Optimization: 1.Basic(1.1. Sequential constant prapogation) 2.Advanced(State opt., retiming,Sequential logic cloning) 
      
State optimizaion-optimizing the new state.Cloning is done at the time of physical aware synthesis. Retiming is a technique that consists of moving flip-flops
and latches across logic for the purpose of improving timing, and so increasing clock frequency. Constant propagation eliminates cases in which values are copied from one location or variable to another.the optimization of a complex boolean expression is a process of finding a simpler one.   
      
 Part-2: Combinational logic optimization:
      
Synthesize the Opt_check.
      
![opt_check synth](https://user-images.githubusercontent.com/104482957/166135124-a4d1df3e-5935-4ab5-bd83-f67a7013a9aa.png)
      
"opt_clean -purge" command use for remove unused cells and wire.
      
![optcheck show](https://user-images.githubusercontent.com/104482957/166135298-33009ad7-3138-456a-8efa-ca887358e46c.png)
     
Opt_check2:

![optcheck2](https://user-images.githubusercontent.com/104482957/166135438-0c3b9748-8317-4bfc-bff3-64b64f1b0b82.png)
      
Opt_check3:
      
![Opt_check3](https://user-images.githubusercontent.com/104482957/166135636-a4f4c862-8a50-4818-906f-79d2d8e5867e.png)
      
Opt_check4:

![optcheck4 synth](https://user-images.githubusercontent.com/104482957/166135734-6554c67f-e81c-45a4-8d23-636ae94b8624.png)

![optcheck4](https://user-images.githubusercontent.com/104482957/166135771-5feccd49-5633-4e88-84df-340ca6188c4e.png)
      
Multiple module opt:
      
      
      
      

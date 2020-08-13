c-# Fault Tolerant RI5CY core<br>
Ensemble of documentation on RISCV, PULPissimo and Fault Tolerant architecture.
## Pulp core cv32e40p
Pulp core cv32e40p could be find at [cv32e40p](https://github.com/openhwgroup/cv32e40p) while core documentation is available in repository [gitDocumentation](https://github.com/openhwgroup/core-v-docs/tree/master/cores/cv32e40p). Here there are all documents that are rendered to html using Sphinx and can be read at [htmlDocumentation](https://core-v-docs-verif-strat.readthedocs.io/projects/cv32e40p_um/en/latest/intro.html) 
### General useful property 
* **Endianness** : all core is Little-Endian so vectors are initialized as \[7:0\] and LSB is in smalles address. 
### System Verilog core file
This core file can be found at [cv32e40p/rtl](https://github.com/openhwgroup/cv32e40p/tree/master/rtl) and describe core architecture in verilog:

#### *cv32e40p/rtl*

 * **cv32e40p_alu_div.sv** :  <br>
 * **cv32e40p_alu.sv** : [Doc](https://core-v-docs-verif-strat.readthedocs.io/projects/cv32e40p_um/en/latest/apu.html) Implement the Auxiliary Processing Unit (APU) used by apu_tracer.<br>
 * **cv32e40p_apu_disp.sv** : <br>
 * **cv32e40p_compressed_decoder.sv** : This decoder implement the *Standard Extension for Compressed Istruction* called "C" or "RVC" which reduces (about -30%) static and dynamic code size by adding short 16-bit instruction encodings for common operations. <br>This technique is described in Chapter 16 of [risc-spec](https://riscv.org/specifications/isa-spec-pdf/). It is also to remember that C extension is not designed to be a stand-alone ISA, and is meant to be used alongside a base ISA.<br> In order to understand the conversion you could see at page 107 and 130 of  [risc-spec](https://riscv.org/specifications/isa-spec-pdf/) for the compressed operation *c.addi4spn* defined at line 61 of cv32e40p_compressed_decoder.sv file.
Remember that RV32I reserves a large encoding space for HINT instructions, which are usually used to communicate performance hints to the microarchitecture.
In this SV file is described the complete combinational decoder that have this behavior:
     * The instruction defined compressed only if *istr_i\[1:0\] != 11*, in this case *is_compressed_o* is asserte and the instruction is decodified using a series of switch and saved in *instr_o*.
     * If instead *istr_i\[1:0\] == 11* the instruction isn't compressed and it is simply copied to output *instr_o*.
     * Finally is a compressed instruction have an invalid or non implemented *funct code (instr_i\[15:13\])*, the *illegal_instr_o* is asserted and the instruction is condisered invalid. 
     * **Fault Tolerant consideration:** since this block is only cmbinatory the easiest way is to use a TMR approach.<br>
 * **cv32e40p_controller.sv** : <br>
 * **cv32e40p_core.sv** : [Doc](https://core-v-docs-verif-strat.readthedocs.io/projects/cv32e40p_um/en/latest/integration.html) Contain main module <span style="color:green">cv32e40p_core</span>. This module is configurable with some parameter.<br>
 * **cv32e40p_cs_registers.sv** : [Doc](https://core-v-docs-verif-strat.readthedocs.io/projects/cv32e40p_um/en/latest/control_status_registers.html) CV32E40P does not implement all control and status registers specified in the RISC-V privileged specifications, but is limited to the registers that were needed for the PULP system. The reason for this is that we wanted to keep the footprint of the core as low as possible and avoid any overhead that we do not explicitly need.<br>
 * **cv32e40p_decoder.sv** : <br>
 * **cv32e40p_ex_stage.sv** : <br>
 * **cv32e40p_fetch_fifo.sv** : <br>
 * **cv32e40p_ff_one.sv** : <br>
 * **cv32e40p_hwloop_controller.sv** :[Doc](https://core-v-docs-verif-strat.readthedocs.io/projects/cv32e40p_um/en/latest/pulp_hw_loop.html) To increase the efficiency of small loops, CV32E40P supports hardware loops optionally. They can be enabled by setting the **PULP_XPULP** parameter. <br>Hardware loops make *executing a piece of code multiple times possible*, without the overhead of branches or updating a counter. Hardware loops involve zero stall cycles for jumping to the first instruction of a loop.<br>
 * **cv32e40p_hwloop_regs.sv** : [Doc](https://core-v-docs-verif-strat.readthedocs.io/projects/cv32e40p_um/en/latest/pulp_hw_loop.html) <br>
 * **cv32e40p_id_stage.sv** : <br>
 * **cv32e40p_if_stage.sv** : <br>
 * **cv32e40p_int_controller.sv** : <br>
 * **cv32e40p_load_store_unit.sv** : [Doc](https://core-v-docs-verif-strat.readthedocs.io/projects/cv32e40p_um/en/latest/load_store_unit.html) The Load-Store Unit (LSU) of the core takes care of accessing the data memory. Load and stores on words (32 bit), half words (16 bit) and bytes (8 bit) are supported. <br>
 * **cv32e40p_mult.sv** : <br>
 * **cv32e40p_obi_interface.sv** : <br>
 * **cv32e40p_pmp.sv** : <br>
 * **cv32e40p_popcnt.sv** : <br>
 * **cv32e40p_prefetch_buffer.sv** : <br>
 * **cv32e40p_prefetch_controller.sv** : <br>
 * **cv32e40p_register_file_ff.sv** : [Doc](https://core-v-docs-verif-strat.readthedocs.io/projects/cv32e40p_um/en/latest/register_file.html#register-file) Positive-edge-triggered flip-flops,  CV32E40P has 31 32-bit wide registers which form registers x1 to x31. Register x0 is statically bound to 0 and can only be read, it does not contain any sequential logic.<br>
The register file has **three read ports and two write ports**, Register file reads are performed in the ID stage. Register file writes are performed in the WB stage.<br>
 * **cv32e40p_register_file_latch.sv** : [Doc](https://core-v-docs-verif-strat.readthedocs.io/projects/cv32e40p_um/en/latest/register_file.html#register-file) latch based register file, same consideration used for _ff, these RF is recommended in ASICs and are smaller.<br>
 * **cv32e40p_register_file_test_wrap.sv** : <br>
 * **cv32e40p_sleep_unit.sv** : [Doc](https://core-v-docs-verif-strat.readthedocs.io/projects/cv32e40p_um/en/latest/sleep.html) The Sleep Unit contains and controls the instantiated clock gate, that gates *clk_i* and produces a gated clock for use by the other modules inside CV32E40P. The Sleep Unit is the only place in which *clk_i* itself is used; all other modules use the gated version of *clk_i*.<br>

#### *cv32e40p/rtl/include*
These instead are include file in *rtl/include* directory:
 * **cv32e40p_pkg.sv** : Here there are many definition:
       * **OPCODES** : The first opcode are RISCV specified in [isa-spec](https://riscv.org/specifications/isa-spec-pdf/) at page 129 instea the latter opcode are pulp specific.
       * **ALU Operation** :
       * **CS Register** : <br>
       
#### *cv32e40p/bhv*

These file instead can be found in [cv32e40p/bhv](https://github.com/openhwgroup/cv32e40p/tree/master/bhv) and are module, these cells are usually specific to the selected target technology and thus not provided as part of the RTL design:  

 * **cv32e40p_apu_tracer.sv** : [Doc](https://core-v-docs-verif-strat.readthedocs.io/projects/cv32e40p_um/en/latest/apu.html) The module <span style="color:green">cv32e40p_apu_tracer</span> can be used to create a log of the APU interface. It is a behavioral, non-synthesizable, module instantiated in the example testbench that is provided for the cv32e40p_core. It can be enabled during simulation by defining CV32E40P_APU_TRACE. The APU trace is written to a log file which is named apu_trace_core_HARTID.log, with HARTID being the 32 digit hart ID of the core being traced.<br>
 * **cv32e40p_core_log.sv** : <br>
 * **cv32e40p_sim_clock_gate.sv** : [Doc](https://core-v-docs-verif-strat.readthedocs.io/projects/cv32e40p_um/en/latest/getting_started.html#getting-started) A simulation-only version of the clock gating. Inside there is a module called <span style="color:green">cv32e40p_clock_gate</span> that perform clock gating. Clock gating is used both in *cv32e40p_sleep_unit.sv* and *cv32e40p_register_file_latch.sv*<br>
 * **cv32e40p_tracer.sv** : <br>
 * **cv32e40p_wrapper.sv** : <br>
 
 #### *cv32e40p/bhv/include*
 * **cv32e40p_tracer_pkg.sv** : <br>
 

### Block diagram generation
* [Block diagram maker](https://www.smartdraw.com/block-diagram/block-diagram-maker.htm)
* [Yosys](http://www.clifford.at/yosys/documentation.html) It is as qurtus but is opensource and seem to have a better rtl viewer that can be used to create **RTL of cb32e40p core**. This is the repository of Yosys [YosysRepo](https://github.com/YosysHQ/yosys), instead [Here](https://electronics.stackexchange.com/questions/269114/how-do-i-generate-a-schematic-block-diagram-from-verilog-with-quartus-prime) and [Here](https://electronics.stackexchange.com/questions/13995/how-can-i-generate-a-schematic-block-diagram-image-file-from-verilog/269121#269121) you can find an example of rtl generated by Yosys.


## Fault Tolerant 


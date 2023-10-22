# UVM-Test-Bench-Environment-ALU
The verification engineer will be given a spec, and they need to start working on the verification plan and testbench. 

## Simple Athematic and Logic Unit Spec
![image](https://github.com/srsapireddy/UVM-Test-Bench-Environment-ALU/assets/32967087/d267b301-03f8-476a-ae27-b803ebf6832b)

## DUT: testbench.sv
```
// ALU Verification
// Date: October 2023

`timescale 1ns/1ns

import uvm_pkg::*;
`include "uvm_macros.svh"

// ----------------
// Include Files
// ----------------
`include "interface.sv"

module top;
  // ----------------
  // Instanstation
  // ----------------
  
  logic clock;
  
  // connecting the clock in the top module to the interface
  // here we are passing the clock in the top module to the interface and from interface we are passing the clock to the DUT
  alu_interface intf(.clock(clock));
  
  alu dut(
    .clock(intf.clock),
    .reset(intf.reset),
    .A(intf.a),
    .B(intf.b),
    .ALU_Sel(intf.op_code),
    .ALU_Out(intf.result),
    .CarryOut(intf.carry_out)
  );
  
  initial begin
    clock = 0;
    #5;
    forever begin
      clock = ~clock;
      #2;
    end
  end
  
  initial begin
    #5000;
    $display("Sorry! Ran out of clock cycles!");
    $finish();
  end
  
  initial begin
    $dumpfile("d.vcd");
    $dumpvars();
  end
  
endmodule: top
```

## Interface: interface.sv
```
// clock is coming from top module so we have to define clock as a port
interface alu_interface(input logic clock);
    /*
    .clock(),
    .reset(),
    .A(),
    .B(),
    .ALU_Sel(),
    .ALU_Out(),
    .CarryOut()
    */
  logic reset;
  
  logic [7:0] a, b;
  logic [3:0] op_code; // select line
  logic [7:0] result;  // output
  bit carry_out;
endinterface: alu_interface
```

## Test Class: test.sv
```
class alu_test extends uvm_test;
  `uvm_component_utils(alu_test)
  
  // constructor
  function new(string name = "alu_test", uvm_component parent);
    super.new(name, parent);
    `uvm_info("TEST_CLASS", "Inside Constructor!", UVM_HIGH)
  endfunction: new
  
  // build phase
  // here super will call the parent build phase method
  function void build_phase(uvm_phase phase);
    super.build_phase(phase);
    `uvm_info("TEST_CLASS", "Build Phase!", UVM_HIGH)
  endfunction: build_phase
  
  // Connect Phase
  function void connect_phase(uvm_phase phase);
    super.connect_phase(phase);
    `uvm_info("TEST_CLASS", "Connect Phase!", UVM_HIGH)
  endfunction: connect_phase
  
  // Run Phase
  task run_phase(uvm_phase phase);
    super.sun_phase(phase);
    
    // Run Phase Logic
  endtask: run_phase
  
  // All the other phases are functions but the run phase is a task because run phase can consume time and it can have time consuming statements. And function cannot include any time consuming statements
endclass: alu_test
```

## Environment Class: env.sv
```
class alu_env extends uvm_env;
  `uvm_component_utils(alu_env)
  
  // constructor
  function new(string name = "alu_env", uvm_component parent);
    super.new(name, parent);
    `uvm_info("ENV_CLASS", "Inside Constructor!", UVM_HIGH)
  endfunction: new
  
  // build phase
  // here super will call the parent build phase method
  function void build_phase(uvm_phase phase);
    super.build_phase(phase);
    `uvm_info("ENV_CLASS", "Build Phase!", UVM_HIGH)
  endfunction: build_phase
  
  // Connect Phase
  function void connect_phase(uvm_phase phase);
    super.connect_phase(phase);
    `uvm_info("ENV_CLASS", "Connect Phase!", UVM_HIGH)
  endfunction: connect_phase
  
  // Run Phase
  task run_phase(uvm_phase phase);
    super.sun_phase(phase);
    
    // Run Phase Logic
  endtask: run_phase
  
  // All the other phases are functions but the run phase is a task because run phase can consume time and it can have time consuming statements. And function cannot include any time consuming statements
endclass: alu_env
```

## Aagent Class: agent.sv
```
class alu_agent extends uvm_agent;
  `uvm_component_utils(alu_agent)
  
  // constructor
  function new(string name = "alu_agent", uvm_component parent);
    super.new(name, parent);
    `uvm_info("AGENT_CLASS", "Inside Constructor!", UVM_HIGH)
  endfunction: new
  
  // build phase
  // here super will call the parent build phase method
  function void build_phase(uvm_phase phase);
    super.build_phase(phase);
    `uvm_info("AGENT_CLASS", "Build Phase!", UVM_HIGH)
  endfunction: build_phase
  
  // Connect Phase
  function void connect_phase(uvm_phase phase);
    super.connect_phase(phase);
    `uvm_info("AGENT_CLASS", "Connect Phase!", UVM_HIGH)
  endfunction: connect_phase
  
  // Run Phase
  task run_phase(uvm_phase phase);
    super.sun_phase(phase);
    
    // Run Phase Logic
  endtask: run_phase
  
  // All the other phases are functions but the run phase is a task because run phase can consume time and it can have time consuming statements. And function cannot include any time consuming statements
endclass: alu_agent
```

## Driver Class: driver.sv
```
class alu_driver extends uvm_driver;
  `uvm_component_utils(alu_driver)
  
  // constructor
  function new(string name = "alu_driver", uvm_component parent);
    super.new(name, parent);
    `uvm_info("DRIVER_CLASS", "Inside Constructor!", UVM_HIGH)
  endfunction: new
  
  // build phase
  // here super will call the parent build phase method
  function void build_phase(uvm_phase phase);
    super.build_phase(phase);
    `uvm_info("DRIVER_CLASS", "Build Phase!", UVM_HIGH)
  endfunction: build_phase
  
  // Connect Phase
  function void connect_phase(uvm_phase phase);
    super.connect_phase(phase);
    `uvm_info("DRIVER_CLASS", "Connect Phase!", UVM_HIGH)
  endfunction: connect_phase
  
  // Run Phase
  task run_phase(uvm_phase phase);
    super.sun_phase(phase);
    
    // Run Phase Logic
  endtask: run_phase
  
  // All the other phases are functions but the run phase is a task because run phase can consume time and it can have time consuming statements. And function cannot include any time consuming statements
endclass: alu_driver
```

## Monitor Class: monitor.sv
```
class alu_monitor extends uvm_monitor;
  `uvm_component_utils(alu_monitor)
  
  // constructor
  function new(string name = "alu_monitor", uvm_component parent);
    super.new(name, parent);
    `uvm_info("MONITOR_CLASS", "Inside Constructor!", UVM_HIGH)
  endfunction: new
  
  // build phase
  // here super will call the parent build phase method
  function void build_phase(uvm_phase phase);
    super.build_phase(phase);
    `uvm_info("MONITOR_CLASS", "Build Phase!", UVM_HIGH)
  endfunction: build_phase
  
  // Connect Phase
  function void connect_phase(uvm_phase phase);
    super.connect_phase(phase);
    `uvm_info("MONITOR_CLASS", "Connect Phase!", UVM_HIGH)
  endfunction: connect_phase
  
  // Run Phase
  task run_phase(uvm_phase phase);
    super.sun_phase(phase);
    
    // Run Phase Logic
  endtask: run_phase
  
  // All the other phases are functions but the run phase is a task because run phase can consume time and it can have time consuming statements. And function cannot include any time consuming statements
endclass: alu_monitor
```

## Sequencer Class: sequencer.sv
```
class alu_sequencer extends uvm_sequencer;
  `uvm_component_utils(alu_monitor)
  
  // constructor
  function new(string name = "alu_sequencer", uvm_component parent);
    super.new(name, parent);
    `uvm_info("SEQR_CLASS", "Inside Constructor!", UVM_HIGH)
  endfunction: new
  
  // build phase
  // here super will call the parent build phase method
  function void build_phase(uvm_phase phase);
    super.build_phase(phase);
    `uvm_info("SEQR_CLASS", "Build Phase!", UVM_HIGH)
  endfunction: build_phase
  
  // Connect Phase
  function void connect_phase(uvm_phase phase);
    super.connect_phase(phase);
    `uvm_info("SEQR_CLASS", "Connect Phase!", UVM_HIGH)
  endfunction: connect_phase
  
  // Run Phase
  task run_phase(uvm_phase phase);
    super.sun_phase(phase);
    
    // Run Phase Logic
  endtask: run_phase
  
  // All the other phases are functions but the run phase is a task because run phase can consume time and it can have time consuming statements. And function cannot include any time consuming statements
endclass: alu_sequencer
```



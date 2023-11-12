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
// driver class is a parameterized class so we need to pass the name of the sequence item class that we are going to use. And same for the sequencer class. So that the driver knows what type of sequence item we are going to drive on the sequencer and driver.

class alu_driver extends uvm_driver#(alu_sequence_item);
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
class alu_sequencer extends uvm_sequencer#(alu_sequence_item);
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
  
  
  // All the other phases are functions but the run phase is a task because run phase can consume time and it can have time consuming statements. And function cannot include any time consuming statements
endclass: alu_sequencer
```

## Sequence Item: sequence_item.sv
```
// This is an object class and not a component class 

class alu_sequence_item extends uvm_sequence_item;
  `uvm_object_utils(alu_sequence_item);
  
  // Instantiation of variables and constructor
  rand logic reset;
  
  rand logic [7:0] a, b;
  rand logic [3:0] op_code; // select line
  
  logic [7:0] result;  // output
  bit carry_out;	   // output
  
  function new(string name = "alu_sequence_item");
    super.new(name);
  endfunction: new
endclass: alu_sequence_item
```

## Base Sequence Class: sequence.sv
```
// Object Class

class alu_base_sequence extends uvm_sequence;
  `uvm_object_utils(alu_base_sequence)
  
  function new(string name = "alu_base_sequence");
    super.new(name);
    `uvm_info("BASE_SEQ", "Inside Constructor!", UVM_HIGH);
  endfunction: new
  
  // what happens in the sequence class will be defined in the body class
  task body();
    `uvm_info("BASE_SEQ", "Inside Body Task!", UVM_HIGH);
  endtask: body
endclass: alu_base_sequence
```

## Bringing it together: Creating a Top Module
### testbench.sv
Here, we have defined the containers. But we also need to determine what logic we need to run for our design. We can simulate up to here to see if there are any major compiler errors.
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
`include "sequence_item.sv"
`include "sequence.sv"
`include "sequencer.sv"
`include "driver.sv"
`include "monitor.sv"
`include "agent.sv"
`include "scoreboard.sv"
`include "env.sv"
`include "test.sv"


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
  
  // Starting the test
  initial begin
    run_test();
  end
  
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

### Writing the main logic
We have to set the interface to be available in every component. Interface is a physical entity we cannot instantiate in dynamic entities like classes. So we need to pass a handle to the interface. </br>
Which are the components that require the virtual interface </br>
1. Driver
2. Monitor
## interface.sv
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
`include "sequence_item.sv"
`include "sequence.sv"
`include "sequencer.sv"
`include "driver.sv"
`include "monitor.sv"
`include "agent.sv"
`include "scoreboard.sv"
`include "env.sv"
`include "test.sv"


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
  
   // ----------------
   // Interface Setting
   // ----------------
   initial begin
     uvm_config_db #(virtual alu_interface)::(null, "*", "vif", vif);
   end
  // here the first and the second parameter will determine the path where the alu_interfce handles are available. (Here we can also specify a specific path)
  // Then we name the interface handle as vif.
  // Then we set the object interface name as vif.
  // -> Thus in the config_db we have an handle called as vif that can be accessed by name vif which is available to every component.
  
  // Starting the test
  initial begin
    run_test();
  end
  
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

## We have to get the interface in the driver and monitor
The best way to do it is to get the interface handle in the test class. Ideally, it is better to get the interface in the build phase </br>
test -> env -> agent -> Then to the driver and monitor component. 
## driver.sv
```
// driver class is a parameterized class so we need to pass the name of the sequence item class that we are going to use. And same for the sequencer class. So that the driver knows what type of sequence item we are going to drive on the sequencer and driver.

class alu_driver extends uvm_driver#(alu_sequence_item);
  `uvm_component_utils(alu_driver)
  
  // to use the vif_driver we have to declare a handle for it
  virtual alu_interface vif;
  
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
    
    if(!(uvm_config_DB #(virtual, alu_interface)::get(this, "*", "vif", vif))) begin
      `uvm_error("DRIVER_CLASS", "Failed to get VIF from config DB!")
    end
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

## monitor.sv
```
class alu_monitor extends uvm_monitor;
  `uvm_component_utils(alu_monitor)
  
  // to use the vif_monitor we have to declare a handle for it
  virtual alu_interface vif;
  
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
    
    if(!(uvm_config_DB #(virtual, alu_interface)::get(this, "*", "vif", vif))) begin
      `uvm_error("MONITOR_CLASS", "Failed to get VIF from config DB!")
    end
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

## sequence class changes: sequence.sv
The sequence item will generate the sequence item packets and send those packets to the driver. 
```
// Object Class

class alu_base_sequence extends uvm_sequence;
  `uvm_object_utils(alu_base_sequence)
  
  alu_sequence_item reset_pkt;
  
  function new(string name = "alu_base_sequence");
    super.new(name);
    `uvm_info("BASE_SEQ", "Inside Constructor!", UVM_HIGH);
  endfunction: new
  
  // what happens in the sequence class will be defined in the body class
  task body();
    `uvm_info("BASE_SEQ", "Inside Body Task!", UVM_HIGH);
    
    reset_pkt = alu_sequence_item::type::create("reset_pkt");
    start_item(reset_pkt);
    reset_pkt.randomize() with {reset==1;};
    finish_item(reset_pkt);
    // when sequence call the start_item() it will get a permission from the sequencer that we can start the sequence in the driver.
    
  endtask: body
endclass: alu_base_sequence


class alu_test_sequence extends alu_base_sequence;
  `uvm_object_utils(alu_test_sequence)
  
  alu_sequence_item item;
  
  function new(string name = "alu_base_sequence");
    super.new(name);
    `uvm_info("TEST_SEQ", "Inside Constructor!", UVM_HIGH);
  endfunction: new
  
  // what happens in the sequence class will be defined in the body class
  task body();
    `uvm_info("TEST_SEQ", "Inside Body Task!", UVM_HIGH);
    
    item = alu_sequence_item::type::create("item");
    start_item(item);
    item.randomize() with {reset==0;};
    finish_item(item);
    // when sequence call the start_item() it will get a permission from the sequencer that we can start the sequence in the driver.
    
  endtask: body
endclass: alu_test_sequence
```

## Adding default constraints to sequence_item.sv
The constraints defined here are from the spec.
```
// This is an object class and not a component class 

class alu_sequence_item extends uvm_sequence_item;
  `uvm_object_utils(alu_sequence_item);
  
  // Instantiation of variables and constructor
  rand logic reset;
  
  rand logic [7:0] a, b;
  rand logic [3:0] op_code; // select line
  
  logic [7:0] result;  // output
  bit carry_out;	   // output
  
  // Default Constraints
  constraint inputs1_c {a inside {[10:20]};}
  constraint inputs2_c {b inside {[1:10]};}
  constraint op_code_c {op_code inside {[0,1,2,3]};}
  
  // constructor
  function new(string name = "alu_sequence_item");
    super.new(name);
  endfunction: new
endclass: alu_sequence_item
```

According to the block diagram the test component will contain the environment class. The environment will contain the Agent class. The agent will contain the driver and monitor and sequencer. We need to instantiate the class accordingly.
# Time: 59:13

















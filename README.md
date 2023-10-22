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


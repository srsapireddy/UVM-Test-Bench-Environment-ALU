# UVM-Test-Bench-Environment-ALU
The verification engineer will be given a spec, and they need to start working on the verification plan and testbench. 

## Simple Athematic and Logic Unit Spec
![image](https://github.com/srsapireddy/UVM-Test-Bench-Environment-ALU/assets/32967087/d267b301-03f8-476a-ae27-b803ebf6832b)

## testbench.sv
```
// ALU Verification
// Date: October 2023

`timescale 1ns/1ns

import uvm_pkg::*;
`include "uvm_macros.svh"

// ----------------
// Include Files
// ----------------

module top;
  // ----------------
  // Instanstation
  // ----------------
  
  logic clock;
  
  alu dut(
    .clock(),
    .reset(),
    .A(),
    .B(),
    .ALU_Sel(),
    .ALU_Out(),
    .CarryOut()
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

## 4_bit_Random_Number_Generation_using_SystemVerilog_PG_DD
## REG NUM :25015534
## NAME :R.Surjini
## EXPERIMENT – 7 4-bit Random Number Generation using SystemVerilog Constraints and Distribution Verification

## Aim
To design a SystemVerilog class using constraint–based randomization to generate 4-bit random numbers, and to verify the distribution of generated values using a testbench in EDA Playground

## Tool Used

Xilinx Vivado Simulator

## Theory
✦ Constrained Random Verification (CRV)

SystemVerilog supports randomization using the rand keyword.
To limit the range of generated values, constraints are used.

Example:

rand bit [3:0] num;
constraint c_range { num inside {[0:15]}; }


This ensures that the random variable num always lies between 0 and 15 (4-bit range).

✦ Distribution Verification

To check whether the random generator is uniform, a frequency counter array is used.
Each time a random value is produced, its frequency count is recorded.
After many trials, the distribution of values is displayed.

CRV is commonly used in functional verification for:
  Coverage improvement
  Auto-test generation
  Exhaustive scenario testing

  ## SYSTREM VERILOG PROGRAM 
  ```
  class rand_gen;
  rand bit [3:0] num;
  constraint c_range { num inside {[0:15]}; }
endclass

`timescale 1ns/1ps

module tb_random_dist;
  rand_gen rg;
  int dists[16];   // frequency counter for 0–15
  int trials = 1000;

  initial begin
    rg = new();
    
    // Initialize distribution
    foreach (dists[i]) 
        dists[i] = 0;

    // Generate random numbers
    repeat (trials) begin
      if (rg.randomize()) begin
        dists[rg.num]++;  // count occurrence
        $display("Generated number = %0d", rg.num);
      end else begin
        $display("Randomization failed");
      end
    end

    // Display distribution
    $display("---- Random Distribution of 4-bit Numbers ----");
    foreach (dists[i]) begin
      $display("Value %0d : %0d times", i, dists[i]);
    end
  end

endmodule
```

## OUTPUT 
![Uploading Screenshot 2025-11-18 090440.png…]()


## RESULT 
A SystemVerilog class was successfully designed with range constraints to generate 4-bit random numbers.
The testbench verified distribution across multiple trials.

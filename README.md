# 4-bit-Ripple-counter-using-Function-and-4-bit-Ripple-Adder-using-task

# Aim
To design and simulate a 4-bit-Ripple-counter-using-Function-and-4-bit-Ripple-Adder-using-task using Verilog HDL, and verify its functionality through a testbench in the Vivado 2023.1 environment. 

# Apparatus Required
Vivado 2023.1
# Procedure
1. Launch Vivado 2023.1
Open Vivado and create a new project.
2. Design the Verilog Code
Write the Verilog code for the seven-segment display, defining the logic that maps a 4-bit binary input to the corresponding segments (a to g) of the display.
3. Create the Testbench
Write a testbench to simulate the seven-segment display behavior. The testbench should apply various 4-bit input values and monitor the corresponding output.
4. Create the Verilog Files
Create both the design module and the testbench in the Vivado project.
5. Run Simulation
Run the behavioral simulation to verify the output. 
6. Observe the Waveforms
Analyze the output waveforms in the simulation window, and verify that the correct segments light up for each digit.
7. Save and Document Results
Capture screenshots of the waveform and save the simulation logs. These will be included in the lab report.

# Verilog Code
# 4 bit Ripple Adder using Task
// 4-bit Ripple Carry Adder using Task
module ripple_adder_task (
    input [3:0] A, B,
    input Cin,
    output reg [3:0] Sum,
    output reg Cout
);
    reg c;
    integer i;

    // Task for 1-bit full adder
    task full_adder;
        input a, b, cin;
        output s, cout;
        begin
            s = a ^ b ^ cin;               // sum
            cout = (a & b) | (b & cin) | (a & cin); // carry
        end
    endtask

    always @(*) 
    begin
        c = Cin;  // initial carry-in
        for (i = 0; i < 4; i = i + 1) begin
            full_adder(A[i], B[i], c, Sum[i], c);
        end
        Cout = c; // final carry-out
    end
endmodule


# Test Bench
`timescale 1ns / 1ps

module RCA_tb;

    // Testbench signals
    reg [3:0] A, B;
    reg Cin;
    wire [3:0] Sum;
    wire Cout;

    // Instantiate the DUT (Device Under Test)
    ripple_adder_task uut (
        .A(A),
        .B(B),
        .Cin(Cin),
        .Sum(Sum),
        .Cout(Cout)
    );

    initial begin
        // Apply test cases
        A = 4'b0000; B = 4'b0000; Cin = 0; #10;
        A = 4'b0101; B = 4'b0011; Cin = 0; #10;
        A = 4'b1111; B = 4'b0001; Cin = 0; #10;
        A = 4'b1010; B = 4'b0101; Cin = 1; #10;
        A = 4'b1111; B = 4'b1111; Cin = 1; #10;

        // End simulation
        #10 $finish;
    end

endmodule
# Output Waveform

# 4 bit Ripple counter using Function
// 4-bit Ripple Counter using Function
module ripple_counter_func (
    input clk, rst,
    output reg [3:0] Q
);

    // Function to increment the counter
    function [3:0] count;
        input [3:0] q_in;
        begin
            count = q_in + 1;   // increment by 1
        end
    endfunction

    // Sequential block
    always @(posedge clk or posedge rst) begin
        if (rst)
            Q <= 4'b0000;       // reset counter
        else
            Q <= count(Q);      // increment using function
    end

endmodule

# Test Bench
`timescale 1ns / 1ps

module ripple_counter_tb;

    reg clk, rst;
    wire [3:0] Q;

    // Instantiate DUT (Device Under Test)
    ripple_counter_func uut (
        .clk(clk),
        .rst(rst),
        .Q(Q)
    );

    // Clock generation: toggle every 5 ns → 10 ns period (100 MHz)
    always #5 clk = ~clk;

    initial begin
        // Initial values
        clk = 0;
        rst = 1;   // start with reset active
        #12;       // hold reset for some time

        rst = 0;   // release reset

        // Let counter run
        #100;

        // Assert reset again mid-way
        rst = 1; #10;
        rst = 0;

        // Run more cycles
        #50;

        // Finish simulation
        $finish;
    end

endmodule




# Output Waveform 


# Conclusion
In this experiment, a 4-bit-Ripple-counter-using-Function-and-4-bit-Ripple-Adder-using-task was successfully designed and simulated using Verilog HDL.

# EXPERIMENT 3B: Simulation of All Flip-Flops using Non Blocking Statement

## AIM
To design and simulate basic flip-flops (SR, D, JK, and T) using **Non blocking statements** in Verilog HDL, and verify their functionality through simulation in Vivado 2023.1.

## APPARATUS REQUIRED
- Vivado 2023.1
- Computer with HDL Simulator

## DESCRIPTION
Flip-flops are the basic memory elements in sequential circuits.  
In this experiment, different types of flip-flops (SR, D, JK, T) are modeled using **behavioral modeling** with **Non blocking assignment (`<=`)** inside the `always` block.  
Non Blocking assignments execute sequentially in the given order, which makes it easier to describe simple synchronous circuits.

## PROCEDURE
1. Open **Vivado 2023.1**.  
2. Create a **New RTL Project** (e.g., `FlipFlop_Simulation`).  
3. Add Verilog source files for each flip-flop (SR, D, JK, T).  
4. Add a testbench file to verify all flip-flops.  
5. Run **Behavioral Simulation**.  
6. Observe waveforms of inputs and outputs for each flip-flop.  
7. Verify that outputs match the truth table.  
8. Save results and capture simulation screenshots.

---

## VERILOG CODE

### SR Flip-Flop (Non Blocking)
```verilog
module SR_ff(clk,S,R,Q);
    input clk,S,R;
    output reg Q;
    
    always @(posedge clk) 
        begin
            case ({S,R})
                2'b00: Q <= Q;      
                2'b01: Q <= 0;      
                2'b10: Q <= 1;      
                2'b11: Q <= 1'bx;   
                default: Q <= Q;
            endcase
        end
endmodule

```
### SR Flip-Flop Test bench 
```verilog
module SR_ff_tb;
  reg clk, S, R;
  wire Q;
  SR_ff dut(.clk(clk),.S(S),.R(R),.Q(Q));
  initial begin
   clk = 0;
  forever #10 clk = ~clk; 
  end
  initial begin
    S = 0; R = 0;
    #100 S = 1; R = 0;   
    #100 S = 0; R = 0;   
    #100 S = 0; R = 1;   
    #100 S = 1; R = 1;  
    #100 S = 0; R = 0;
 end
endmodule
```
#### SIMULATION OUTPUT

<img width="1917" height="1075" alt="image" src="https://github.com/user-attachments/assets/53efb9f8-7668-4bff-a734-98235093c888" />

---

### JK Flip-Flop (Non Blocking)
```verilog
module JK_ff(clk,rst,J,K,Q);
    input clk,rst,J,K;
    output reg Q;
    always@(posedge clk)
      begin
        case({J,K})
            2'b00 : Q <= Q;
            2'b01 : Q <= 0;
            2'b10 : Q <= 1;
            2'b11 : Q <= ~Q;
            default : Q <= Q;
         endcase
     end
endmodule
```
### JK Flip-Flop Test bench 
```verilog
module JK_ff_tb;
    reg clk_t,rst_t,J_t,K_t;
    wire Q_t;
    
    JK_ff dut(.clk(clk_t),.rst(rst_t),.J(J_t),.K(K_t),.Q(Q_t));
    
    initial
      begin
        clk_t = 1'b0;
        rst_t = 1'b1;
      #20
        rst_t = 1'b0;
        J_t = 1'b0;
        K_t = 1'b0;
      #20
        J_t = 1'b0;
        K_t = 1'b1;
      #20
        J_t = 1'b1;
        K_t = 1'b0;
      #20
        J_t = 1'b1;
        K_t = 1'b1;
     end
     
     always 
        #10 clk_t = ~clk_t;  
endmodule
```
#### SIMULATION OUTPUT
<img width="1915" height="1069" alt="image" src="https://github.com/user-attachments/assets/c9824739-1324-4c49-85f3-503bd1def7cb" />

---
### D Flip-Flop (Non Blocking)
```verilog
module D_ff(clk,rst,d,dout);
    input clk,rst,d;
    output reg dout;
    always@ (posedge clk)
    begin
        if(rst)
            dout <= 1'b0;
        else
            dout <= d;
     end
endmodule

```
### D Flip-Flop Test bench 
```verilog
module D_ff_tb;
    reg clk_t,rst_t,d_t;
    wire dout_t;
    
    D_ff dut(.clk(clk_t),.rst(rst_t),.d(d_t),.dout(dout_t));
    
    initial
      begin
        clk_t = 1'b0;
        rst_t = 1'b1;
     #20
        rst_t = 1'b0;
        d_t   = 1'b0;
     #20
        d_t = 1'b1;
     end
     
     always 
        #10 clk_t = ~clk_t;
endmodule
```

#### SIMULATION OUTPUT
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/915c66ef-f95c-44fc-a53c-34c8a2296e08" />

---
### T Flip-Flop (Non Blocking)
```verilog
module T_ff(clk,rst,T,Tout);
    input clk,rst,T;
    output reg Tout;
    
    always@(posedge clk) 
      begin
         if(rst)
            Tout <= 1'b0;
         else if(T)
            Tout <= ~Tout;
         else
            Tout <= Tout;
   end
endmodule
```
### T Flip-Flop Test bench 
```verilog
module T_ff_tb;
    reg clk_t,rst_t,T_t;
    wire Tout_t;
    
    T_ff dut(.clk(clk_t),.rst(rst_t),.T(T_t),.Tout(Tout_t));
    
    initial
     begin
        clk_t = 1'b0;
        rst_t = 1'b1;
     #20
        rst_t = 1'b0;
        T_t = 1'b0;
     #20
        T_t = 1'b1;
     end
     
     always 
        #10 clk_t = ~clk_t;
endmodule
```

#### SIMULATION OUTPUT
<img width="1919" height="1077" alt="image" src="https://github.com/user-attachments/assets/06ed45c4-9cba-48d8-a7a3-60621fcc93bb" />

---

### RESULT

All flip-flops (SR, D, JK, T) were successfully simulated using Non blocking statements in Verilog HDL.
The outputs matched the expected truth table values, demonstrating correct sequential behavior.

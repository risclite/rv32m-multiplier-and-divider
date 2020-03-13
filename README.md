# rv32m-multiplier-and-divider
a multiplier/divider verilog RTL file for RV32M instructions 

Division is a basic calculation of verilog RTL design. Most design needs 32 cycles to finish a 32bit/32bit division. I have a simple method to  decrease cycles, which depends on how many "1" bits of the quotient.  Let's have a simple example :

* Dividend:  'b100011

* Divisor: 'b1011

1. Firstly, check whether the dividend is less than the divisor. If yes, calculation is over, otherwise, go on. 

2. Get the highest "1" positon of both. Assume m is for dividend: 5 (100011), n is for divisor: 3 (1011).   

3. Get two numbers: x =  divisor<<(m-n); y = x>>1. Here x is 101100, y is 10110

4. Two subtractions:  p = Dividend - x and  q =Dividend - y. Assume "t" is an overflow of  "Dividend - x". Here p is overflow, t is "1"; q is "1101".

5. If t is zero, we get one bit of quotient: 1'b1<<(m-n), and the new dividend or remainder is "p". 

6. If t is one, we get one bit of quotient: 1'b1<<(m-n-1), which is "10". the new dividend is "q", which is "1101".  

7. Go to the first step. The new dividend "1101" is more than the divisor "1011".  So there is a new iteration. After that, we get the quotient: "11", and the remainder is "10".

Only the valid bit of quotient triggers an iteration.  There is no redundant cycles. 

This multiplier&divider RTL file is dedicated to RISCV CPU core. It implements RV32M instructions. 

The cycles of the multiplier function depends on how many "1" bits of the lesser of two multiplicators. It is also efficient.

Try to do simulation on the two .v files: tb.v and mul.v. If you know how the tb.v file works, you know how to use this multiplier/divider.

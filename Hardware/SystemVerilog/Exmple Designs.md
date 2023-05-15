## barrel shifter

Systemverilog has build-in shift functions, sometimes they cannot synthesized automatically. This exmple shows the 8-bit barrel shifter. this is a (combinational circuit)[[FPGA prototyping#common errors in combinational logic circuit]]

```systemverilog
module barrel_shifter(
	input  logic [7:0] a,
	input  logic [2:0] amt,
	output logic [7:0] y
);
	always_comb begin
		case(amt)
			3'o0:    y = a;
			3'o1:    y = {a[0], a[7:1]};
			3'o2:    y = {a[1:0],a[7:2]};
			3'o3:    y = {a[2:0],a[7:3]};
			3'o4:    y = {a[3:0],a[7:4]};
			3'o5:    y = {a[4:0],a[7:5]};
			3'o6:    y = {a[5:0],a[7:6]};
			default: y = {a[6:0],a[7]};
		endcase
	end
endmodule
```

## Simplified floating-point adder

Floating point is another format to represnt a number. SystemVerilog has a build-in floating point data type, it is too complex to be synthesized automatically.this is a (combinational circuit)[[FPGA prototyping#common errors in combinational logic circuit]]

this example uses 13-bit format and ignore the round-off error.

___s___ - which indicates the sign of the number (1 for negative)
___e___ - exponent field (unsigned) (4 bit)
___f___ -  the significand or the fraction (unsigned) (8 bit)

number represntaion =>  $(-1)^s*.f*2^e$

Assumption:
- Both exponent and significand fields are in unsigend format
- The representation has to be either normalized or zero. Normalized representation means that the MSB of the significand field must be '1'. If the magnitude of the computation result is smaller than the smallest normalized nonzero magniture, $0.10000000*2^0000$ it must be converted to zero

Under these assumptions, the largest and smallest nonzero magnitudes are $0.11111111*2^{1111}$ and $0.10000000*2^{0000}$

floating-point adder has 4 major steps show below

![[Screenshot from 2023-05-10 21-29-16.png]]

1. _Sorting_: puts larger magnitude on the top and the smaller magnitude on the bottom (called sorted numbers "big number" and "small number")
2. _Alignment_ : aligns the two numbers so that they have the same exponent. this done by adjusting the small number to match the exponent of the big number.
3. _Addition/substration_
4. _Normalization_: adjust the result to normalized format. There types of normalization procedures may be needed
			- After a subtraction result may contain leading zeros in front
			- After a subtraction result may too small to be normalized so need to converted to zero
			- After addition result may generate carry-out bit

coding suffixs 
b :- "big number"
s :- "small number"
a :- "allign number"
r :- "result of addition/subtraction"
n :- "normalized number"


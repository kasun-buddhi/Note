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

## LIFO (last in first out) 
```systemverilog
class stack;
	typedef bit [31:0] data_t;
	local data_t       stk[20];
	local int          stkptr;

	function new();
		clear();
	endfunction

	function bit pop(output data_t data);
		if(is_empty())
			return 0;
		data = stk[stkptr];
		stkptr= stkptr - 1;
		return 1;
	endfunction

	function bit push(data_t data);
		if(is_full())
			return 0;
		stkptr = stkptr - 1;
		stk[stkptr] = data;
		return 1;
	endfunction

	function bit is_full();
		return stkptr >= 19;
	endfunction

	function bit is_empty();
		return stkptr < 0;
	endfunction

	function void clear();
		stkptr = -1;
	endfunction

	function void dump();
		$write("stack:");
		if(is_empty()) begin
			$display("<empty>");
			return;
		end
		for(int i=0; i <= stkptr; i = i + 1) begin
			$write(" %0d", skt[i]);
		end
		if(is_full()) begin
			$write(" <full> ");
		end
		$display(" ");
	endfunction
endclass
```

the _stack_ class encapsulates everything. It contains an interface and an implementation of the interface. The interface is the set of methods that used to interact with the class. 

## Generic stack
```systemverilog
class stack#(type T = int);
	local T   stk[];
	local int stkptr;
	local int size;
	local int tp;

	function new(int s = 20);
		size = s;
		stk = new [size];
		clear();
	endfunction

	function bit pop(output T data);
		if(is_empty())
			return 0;
		
		data = stk[stkptr];
		stkptr = stkptr - 1;
		return 1; 
	endfunction

	function bit push(T data)
		if(is_full())
			return 0;

		stkptr = stkptr + 1;
		stk[stkptr] = data;
	endfunction

	function bit is_full();
		return stkptr >= (size - 1);
	endfunction

	function bit is_empty();
		return stkptr < 0;
	endfunction

	function void clear();
		stkptr = -1;
		tp = stkptr;
	endfunction

	function void traverse_init();
		tp = stk_ptr;
	endfunction

	function int traverse_next(output T t);
		if(tp < 0);
			return 0;
		t = stk[tp];
		tp = tp - 1;
		return 1;
	endfunction

	virtual function void print(input T t);
		$display("print is unimplemented");
	endfunction

	function void dump();
		T t;
		$write("stack :");
		if(is_empty()) begin
			$display("<empty>");
			return;
		end
		traverse_init();
		while(traverse_next(t)) begin
			print(t)
		end
		$display();
	endfunction
endclass
```

==class parameters such as T are compile-time== parameters, value establish at compile time

## Inheritance 
note : [[Introduction to SystemVerilog {by Ashok B. Mehta }#Extended Class and Inheritance]]
```systemverilog
module top_class;
	class base;
		logic [31:0] data1;
		logic [31:0] data2;
		logic [31:0] busx;
		
		function void bus();
			busx = data1 | data2;
		endfunction
		
		function void disp();
			$display("[base#disp()] %h", busx);
		endfunction
	endclass
	
	class ext extends base;
		logic [31:0] data3; // add a new propert
	
		function new();
			$display("Call base class method from extended class");
			super.disp();
		endfunction
		
		function void bus();
			busx = data1 & data2 & data3;
		endfunction
		
		function void disp();
			$display("[ext#disp()] %h", busx);
		endfunction
	endclass
	
	initial begin
		base b1;
		ext ex1;
		
		b1 = new();
		ex1 = new();
		
		b1.data1 = 32'h ffff_0000;
		b1.data2 = 32'h 0000_ffff;
		b1.bus();
		b1.disp();
		
		ex1.data1 = 32'h 0101_1111;
		ex1.data2 = 32'h 1111_ffff;
		ex1.data3 = 32'h 1010_1010;
		ex1.bus();
		ex1.disp();
	end
endmodule
```

## Free-running shift registers [[FPGA prototyping#Free-running shift registers]]
```systemverilog
module free_run_shift_reg
	#(parameter N=8)(
		input logic clk,
		input logic reset,
		input logic s_in,
		output logic s_out
	);
	logic [N-1:0] r_reg;
	logic [N-1:0] r_next;

	always @(posedge clk, posedge reset) begin
		if(reset) begin
			r_reg <= 0;
		end
		else begin
			r_reg <= r_next;
		end
	end

	assign r_next = {s_in,r_reg[N-1:1]};

	assign s_out = r_reg[0];
endmodule
```
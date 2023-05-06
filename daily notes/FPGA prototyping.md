## FPGA

field-programmerble gate arrays contain two-dimentional array of generic logic cells and programmable switches

### FPGA development flow

### routing structors
there is 2 types of typing routing structures

			1) priority routing structures
			2) multiplexing network

### common errors in combinational logic circuit
1) variable assigned in varies always blocks
2) Incomplete sensitivity list
3) Incomplete branch and incomplete output assignment
			:  *example
```systemverilog
always_comb 
	if (a > b)
		gt = 1'b1;
	else if (a == b)
		eq = 1'b0;
```
	(may inferred unintented laches)

add a else branch and explicitly assign all output variable
``` systemverilog
always_comb begin
	if ( a > b ) begin
		gt = 1'b1;
		eq = 1'b0;
	end 
	else if (a == b) begin
		gt = 1'b0;
		eq = 1'b1;
	end
	else  begin
		gt = 1'b0;
		eq = 1'b0;
	end
end
```
or add continues assignment in the begining 
```systemverilog
always_comb begin
	gt =  1'b0;
	eq = 1'b0;
	if( a > b)
		gt = 1'b1;
	else if (a == b)
		eq = 1'b1;
end
```

same as about in the case statement we can use default case and continues assignment in the begining for to fix the problem for unintented laches

## Guidelines
- Use **always_comb** and blocking assignment for a combinational circuit
- assign a variable in a single always block
- think the code as a hardware part
- make sure all the branch in if and case statement are included
- make sure output are assigned in all branches
- be aware of the type of routing network inferred by different control construct
- use a loop statement as shorthand to describe replicated structure 


### Parameter and constant

it can make code scalable and reusable

## <u>Replicated Stucture</u>

there are two well-patterned structure, cascadine chain or two-dimentional mesh. replicated structure can be desctribed by generated-for statement and procedural-for statemen

### Generate-for statement


```systemverilog
generate
	genvar [initial variable]
	for([initial_statement]; [expression]; [step_assignment]) begin
		[concurrent_constructs];
		[concurrent_constructs];
	end
```

loop boundaries has to be static. because it has to be determined for the time of synthesis. usually boundary values derived from parameters created for ==**scalable** and **reusable** code== 

genvar - like integer

loop body can contains a collection of concurrent construct which means continuous statements, module instantiations, always blocks

### Procedural-for Structure
```systemverilog
for ([initital_assignment]; [expression]; [step_assignment]) begin
			[procedural statement];
			[procedural statemetn];
	end
```


procedural-for can only be used within an **always block** 

```systemverilog
module reduced_xor_loop #(
	parameter N = 8
)(
	input  logic [N-1:0] a,
	output logic         y
);
	always_comb begin
		logic temp;
		temp = a[0];
		for (int i = 1; i < N ; i = i + 1) begin
			temp = a[i]^temp;
		end
		y = temp;
	end
endmodule
```

[[Exmple Designs]]

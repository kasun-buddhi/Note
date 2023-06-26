## Introduction
[[2023.05.31]]
SystemVerilog standard was developed to meet increasing usage of the language in specification, design, and verification of hardware
SystemVerilog language has 4 distinct languages under a single simulation kernel
 1. SystemVerilog for design (like : [[FPGA prototyping]]) - synthesizable subset
 2. SystemVerilog for verification(like:  [[OVM Cookbook]], [[UVM Cookbook]]) - which includes the OOP subset
 3. SystemVerilog Assertions - Check combinational / sequential logic response from the DUT  
 4. SystemVerilog Functional coverage

Those 4 language concepts are orthogonal to each other 

![[Screenshot from 2023-05-31 10-15-23.png]]

## Data Types (chapter 2)
[[2023.06.01]]
### Real data types
real          - 64-bit
shortreal - 32-bit
realtime   - This is treated synonymously with "real" declarations and can be used                             interchangeably
Those 3 types is not allowed on following situations
- cannot use in Edge event controls (posedge, negedge, edge)
- cannot Bit-select or part-select
- cannot index expression of vectors

|conversion function | Description|
|---------------------|------------|
| integer $rtoi(real_val) | converts real values to am integer type by truncating the real value to nearest integer (ex: 123.45 becomes 123) . this is differ from casting a real value to an integer or other integral types, in that casting will perform rounding instead of truncation|
| real $itor(int_val) | $itor converts integer values to real values (ex: 123 to 123.0) |
|  \[63:0] $realtobits | Coverts values from real type to a 64-bit vector representation of the real number |
| real $bitstoreal(bit_val) | Converts a bit pattern creatd by $realtobits to a value of the real type |
| \[31:0] $shortrealtobits( shortreal_val) | Converts values from s shortreal type to the 32-bit vector representation of the real number |
| shortreal $bitstoshortreal( bit_val) | Converts a bit pattern created by %shortrealtobits to a value of the shortreal type |

### Nets 
Nets are used to connect elements such as logic gates. they do not store any values.
they simply make a connection from the driving logic to the receiving logic or simply pull a net high or low- or high-impedance state.

#### wire and tri
- wire and tri nets connect elements. They do not store any value
- wire and try are identical in their syntax and functions
- wire net can be used for nets that are driven by a single gate or continuous assignment. Note that this is simulator/methodology dependent. A "wire" that is driven by multiple nets may be caught by a lint type tool to catch a multiply driven "wire"
- tri net type can be used where multiple drivers drive a net.

## Packages (chapter 7) - 
[[2023.05.25]]
SystemVerilog packages provide encapsulation of many different data types, nets, variables, tasks, functions, assertion sequences, properties, and checker declarations.
Can be shared among multiple modules, interfaces, programs, and checkers.
They have explicitly named scopes(wildcard scope - assessing all members of a scope). that allow referencing of data in that scope.
==Items within packages cannot have hierarchical references to identifiers except those created within the package or made visible by the import of another package==

## Class (chapter 8)
A class is the basis for OOP.  and class is a namespace and a scope. It contain members and methods. Classes are always constructed dynamically, which means that the memory for  the class is not allocated until procedurally calling a routine that constructs a class object.

### Base class
Base class is the topmost class in a class hierarchy. You derive extended classes from a base class. 
if you not declare constructor, an implicit new() function will be called automatically, when the class is instantiated, In this case, the variables will be initialized to their default values. '0' for 2-state and 'x' for 4-state.

Declaring a class variable does not create a class object, only the space to hold the object handles. 
This contrasts with other data types where the declaration of a variable creates an object of that type and gives you a symbolic name to reference those objects.

```systemverilog
typedef struct {bit [7:0] member1; bit member2} Mystruct;
Mystruct StructVar1. StructVar2;
```
This creates and allocates the space for two Mystruct-type objects, and I can use StructVar1.member1 to access one of its member. On the other hand:

```systemverilog
MyClass classVar1, classVar2;
```
This creates and allocates the space for two MyClass variable but only allocates the space to hold handles to MycClass objects. not the objects themselves. If try to access classvar1.member1, you would get a null handle reference error because the initial value of a class variable is the special value null. One of the nice things about handles they remove the possiblity of corrupt object references that access uninitialized or de-allocated memory.

__Object Handle__ : When you instantiate a class, the class variable is then said to hold the object handle.

Once you have a class variable then call new() method to construct a class object
```systemverilog
classvar1 = new();
```

### Extended Class and Inheritance
The extended class _inherits_ all the properties and methods of the base class. When you construct an extended class type. you are constructing a single object that has all the properties of the inherited types. 
Both the properties and methods can be overridden in an extended class. It can be added more properties and methods
[[Exmple Designs#Inheritance]]


#### Inheritance Memory Allocation

```systemverilog
module class_TOP();
	class aa;
		int i1;
		function funAA();
		endfunction
	endclass: aa

	class bb extends aa;
		int i1;
		function funBB();
		endfunction
	endclass: bb

	initial begin
		bb b;
		b = new();
	end
endmodule
```

![[Screenshot from 2023-05-24 17-00-23.png]]

### Class Constructor

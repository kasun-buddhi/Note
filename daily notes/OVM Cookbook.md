
## Layered organization of Testbench

Testbench is a network of verification component. OVM defines verification components. their structure, and the interfaces
![[Screenshot from 2023-05-15 07-08-16.png]]

### Transactors
The role of a transactor in a testbench is to convert a stream opf transactions to pin-level activity or vice versa. Transactors are characterized by having at least one pin-level interface and at least one transaction-level interface.

_monitor_ :- A monitor, as the name implies, monitors a bus. It watches the pins and converts their wiggles to a stream of transactions/ Monitors are passive, meaning they do not affect the operation of the DUT in any way.

_Driver_ :- A driver converts a stream of transactions( sequence items) into pin-level activity.

_Responder_(driver with slave side) :- A responder is much like a driver, it responds to activity on pins rather than initiating activity

### Operational Components

The _operational components_ are the set of components that provide all the things the DUT needs to operate. 

__Stimulus Generator__ : which creates a stream of transactions for exercising the DUT. 

__Master__ : A master is a bidirectional component that sends requests and received responses. 

__Slave__ : Slaves. like master, are bidirectional components. They respond to requests and return responses

### Analysis components
Analysis components receive information about what's going on in the restbench and use that information to make some determination about the correctness or completeness of the test. 

__Scoreboard__ : Scoreboard are used to determine correctness of the DUT.(Does it work question)

__Coverage collector__ : The purpose is to determine verification completeness by answering are-we-done question.

### Controller
Controller form the main thread of a test. Typically, controllers receive information from scoreboard and coverage collectors and send information to environment components. 
A controller might start a stimulus generator running and then wait for a signal from a coverage collector to notify it when the test is complete. The controller ==in turn, stops the stimulus generator ,more elaborate variations, possible configurations,  a controller supplies a stimulus generator with an initial set of constraints and starts the stimulus generator running.== 

## Two Domains
We can view the two separate sets of components in testbench

_operational domain_ - DUT/ stimulus generators/ BFM/ similar components that generate stimulus and provide responses that drive the simulation

_analysis domain_ - monitor transaction/ scoreboards/ coverage collectors/ controllers

==Data must be moved from the operational domain to the analysis domain in a way that does not interfere with the operation of the DUT preserves event timing==. This is accomplished by ==analysis port== 

One of the key features of analysis ports is that they have a single interface function, write(). 
![[Screenshot from 2023-05-15 17-32-51.png]]


# Fundamentals of OOP

```systemverilog
class register;
	local bit[31:0] contents;
	
	function void write(bit [31:0] b);
		contents = b;
	endfunction

	function bit[31:0] read();
		return contents;
	endfunction
endclass
```
let's see how it calls
```systemverilog
module top;
	register r;
	bit [31:0] d;

	initial begin
		r = new();
		r.write(32'h00ff5432);
		d = r.read();
	end
endmodule
```
__local__ - attribute on class member tells the compiler to strictly enforce the boundaries of the class. 

You can use classes to create data types, encapsulate mathematical computations or to create dynamics data structures like stacks, lists, queues. 

LIFO exmaple [[Exmple Designs#LIFO (last in first out)]]

## Object Relationship
The true power of OOP becomes apparent when objects are connected in various relationships . Most fundamental relationships are HAS-A and IS-A

### HAS-A
This concept refers to the concept of one object contained or owned by another.
Object **A** contains a reference or a pointer to object **B**
![[Screenshot from 2023-05-16 15-06-53.png]]

Object A owns an instance of object B. Coding a HAS-A relationship in SystemVerilog involves instantiating one class inside another or in some other way providing a handle to one class that is stored inside another
```systemverilog
class A;
endclass

class B;
	local B b;
	function new()
		b = new();
	endfunction
endclass
```

**IS-A**
This is referred as _inheritance_ . A new class is derived from a previous existing object and inherits its characteristics.  

```systemverilog
class A;
	int i;
	float f;
endclass
class B extends A;
	string s;
endclass
```

## Virtual Functions and Polymorphism
One of the reasons for composing objects through inheritance is to establish different behaviors for the same operation. The behavior defined in a derived class overrides behavior defined in a base class. This is done by using _virtual functions_ 
```systemverilog
class generic_packet;
	addr_t src_addr;
	addr_t dest_addr;
	bit m+header [];
	bit m_trailer [];
	bit m_body [];

	virtual function void set_header();
	virtual function void set_trailer();
	virtual function void set_body();
endclass
```

Above code has 3 virtual functions to set the content of the packet. Different kind of packets require different kinds of components. We use generic_packet as a base class and derive different kinds of packets from it.
```systemverilog
class packet_A extends generic_packet;
	virtual function void set_header();
	endfunction
	virtual function void set_trailer();
	endfunction
	virtual function void set_body();
	endfunction
endclass
```

```systemverilog
class packet_B extends generic_packet;
	virtual function void set_header();
	endfunction
	virtual function void set_trailer();
	endfunction
	virtual function void set_body();
	endfunction
endclass
```

## Generic Programming

Making programs generic is consists with the OOP goal of separating concerns. Thus OOP languages provide facilities for building generic code. (template - in C++, parameterized classes - in SystemVerilog)
```systemverilog
class param #(type T=int, int R = 16)
endclass
```
T - type parameter , R - integer type

parameters applied 
```systemverilog
param #(real, 25) z;
param #(int unsigned, 14) q;
```

Generic Stack -> [[Exmple Designs#Generic stack]]

## Classes and Modules

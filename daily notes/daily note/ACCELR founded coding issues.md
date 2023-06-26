github (#git) config file format
.ssh/config file format 

```
Host github.com
    User git
    HostName github.com
    PreferredAuthentications publickey
    IdentityFile /home/kasun-accelr/.ssh/id_ed25519
```

SystemVerilog reseved words :- sequence, checker

- packet is a set of a field not a jsut a buffer
- pre randomize and post randomizeT
- reusable code based with data_packet

### Systemverilog casting
This means to conversion of a one datatype to another datatype. 

Static casting
- SystemVerilog static casting is not applicable to OOP
- Static casting converts one data type to another compatible data type

Dynamic casting

[[2023.05.22]]
```
serialize() - it will give the single buffer from data_field
derialize() - it will give the data_fields from a buffer
```

get to know forward declaration 

[[2023.05.23]]
### class

Class object declaration may not work outside the procedural block 
-  user defined data type and must declare a class inside of a module or a package or an interface
- dynamically create and destroy our class objects
- not static

```systemverilog
module module1;
	class myclass;
		int number;
	endclass
	
	myclass c1; // <- Handle
	initial begin
		c1 = new; // <= instance
	end

	myclass c2 = new; // <- handle and instance
endmodule 
```

[[2023.05.24]]
```
xvlog -sv -L axi4stream_vip_v1_1_13 -L xilinx_vip -L xil_defaultlib  --include "../../../../axi4stream_vip_1_ex.gen/sources_1/bd/ex_sim/ipshared/8713/hdl" --include "/data/Xilinx/Vivado/2022.2/data/xilinx_vip/include" ../../../../axi4s_vip_base/axi4s_vip_base.sv 
```
This will compile a sv file = '../../../../axi4s_vip_base/axi4s_vip_base.sv' 
This method is useful to notice some syntax errors 

[[2023.05.25]]

Arguments in a pure virtual function should match with the derived class functions arguments name

[[2023.06.01]]
can not run tight loop in forever block ^51619d

[[2023.06.22]]
pulp-sdk step guide
```
export PULP_RISCV_GCC_TOOLCHAIN=/opt/riscv
export PATH=$PULP_RISCV_GCC_TOOLCHAIN/bin:$PATH
source configs/pulp-open.sh
make clean all run
```
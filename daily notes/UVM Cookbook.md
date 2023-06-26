## Testbench Basics
[[2023.06.14]]
Each component in a UVM testbench has a specific purpose and a well-defined interface to the rest of the testbench to enhance productivity and facilitate reuse. 
The DUT is connect to a layer of transactors (driver, monitors, responders). These transactors communicate with the DUT at the pin level by driving and sampling DUT signals.

## UVM components
UVM testbench is composed of component objects extended from the uvm_component base class. 
![[Screenshot from 2023-06-15 15-36-34.png]]

#### classes

| class | Description | Contains sub-component? |
|------|--------------|----------------------------|
| uvm_driver | Encapsulates sub-components for sequence communication with the uvm_sequencer | No |
| uvm_sequencer | Encapsulates sub-components for sequence communication with the uvm_driver | No |
| uvm_subscriber | Encapsulates a uvm_analysis_export and associated virtual write method to implement analysis transaction processing | No |
| uvm_env | Basic for aggregating verification components around a DUT, or other envs in case of vertical sub-system integration | Yes |
| uvm_test | Basic for a concept top level test | Yes |
| uvm_monitor | Basic for a concrete top level test | Yes |
| uvm_scoreboard | Basic for a concrete scoreboard | Yes |
| uvm_agent | Basic for concrete agent including a sequencer-driver pair and a monitor | Yes |

## UVM factory

the purpose of factory is change one type of object to derived typed object without changing the structure or testbench code.

### factory coding convension

1) registrering                  - component or object must contain factory registration
2) constructor defaults   
3) component and object creation

## UVM  phases

This is execution flow of test bench,

1) build phases
2) run-time phases
3) cleanup phases 
![[Screenshot from 2023-06-18 16-31-08.png]]
### Starting UVM phase Execution
To start a UVM testbench, the run_test() method has be called from the static part of the testbench. It is usually called from within an initial block in the top level module of the testbench.
Calling run_test() construct the UVM component root component and then initiate the UVM phasing. The run_test() method can be passed a string argument top define the default type name of an uvm_component derived class which is used as the root node of the testbench hierarchy. However, the run_test() method checks for a command line plusarg called UVM_TESTNAME and uses that plusarg string to lookup a factory registered uvm_component, but it can be derived from any uvm_component. The root node defines the test case to be derived from a uvm_test component

### Phase Descriptions
#### Build phase
The build phases are executed at the start of the simulation and their overall purpose is to construct, configure and connect the testbench component hierarchy.
All build phase methods are functions and therefore execute in zero simulation time.

##### build
Once the UVM testbench root node component is con

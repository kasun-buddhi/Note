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
A controller might start a stimulus generator running and then wait for a signal from a coverage collector to notify it when the test is complete. The controller ==in turn, stops the stimulus generator ,more elaborate variations, possible configurations, == 
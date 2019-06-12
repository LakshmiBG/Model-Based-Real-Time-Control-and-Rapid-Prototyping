# Model-Based-Real-Time-Control-and-Rapid-Prototyping
The aim of this exercise is to design and implement a real-time control system for a 4 degree-of- freedom hydraulic loader (i.e. boom) and test it in a rapid prototyping environment.

## 1. Introduction
The purpose and scope of this exercise are to design and implement a real-time control system for the given 4 degrees of freedom (DOF) hydraulic boom model and to test it in the rapid prototyping environment. 
In any product development phase, testing and validating the design is of utmost importance before the product production. Testing and validating any system for a range of parameters provides to be expensive and time-consuming when implemented on a real prototype. In these conditions, rapid prototyping systems comprising of hardware components can mimic actual behaviour of the desired system. 
One of the quickest and inexpensive way of testing, verifying and evaluating control design is by rapid prototyping. Over here, the term Rapid prototyping implies developing an executable model on software that can be continuously tested and refined to make it better. It allows creating a working implementation of signal processing or control design and verifying the same on the hardware in the early design process, before production. It is proof to validate the control designs. The continuous testing on the real-time hardware provides a platform for refining the correctness of the design. This way the model can be refined for perfection and accuracy while reducing the product development time. 
In this course project, a control system for a hydraulic loader was designed and implemented in the State flow environment of the MATLAB Simulink. Stateflow is an environment for modelling and simulating combinatorial and sequential decision logic based on state machines and flow charts. The online model is tested with the help of Simulink real time. IHA 3D is the visualization software used.
The content below provides a brief information  about the uses cases and requirements of the 
developed system. It starts with general description of the vehicle in chapter 2. Chapter 5 tells 
about the user interface and explains the graphical user interface and its elements and its uses 
in this system. The joystick model and it axes control are explained. 
Chapter 6 speaks about the system design, which includes the high-level architecture, real 
time system architecture, input and output of the system, controller, boom model and finally 
the state machine. 
Chapter 7 further deals with how the system was implemented and tested, changes made 
after post demonstration.

## 2. General Description
The equipment used is a forestry forwarder with a hydraulic boom system for loading and unloading logs. The operator controls the boom movements manually during loading and unloading by the aid of joysticks. The boom can be driven in two ways: 
  Control and command each joint of the boom individually i.e. joint angles q1, q2, q3 and telescopic joint d4. 
  Control and command the end of the boom along the planar coordinate axes i.e. joint angle q1, horizontal distance (x) and vertical distance (y) of the boom’s tip. 
In addition to the manual control, the operator has the choice of automatically driving the 
boom from transport position (idle boom resting posoition) to operation standby position (boom working position) usually at the start and end of operations. During the automatic movement, the operator controls the velocity of the boom using single axis of the joystick. 
The joints are operated in a close loop control with an exception during fault or service conditions wherein, the operator can control the boom in open loop control. The developed control system has a self-collision detection function implemented. When the joints are about to collide the control system turns off all the boom movements. Here the operator can retrieve the system in service mode/open loop control. The control system provides an option of transition to safe state form any working state of operation in situations of emergency wherein, all the boom movements are stopped. The user interface houses the switches for selecting different modes of operation as well visualizing the current operation mode. It also helps in monitoring the current joint position.

## 3. Use Case
It can be used to communicate with customers, gather requirements, in system testing as well as manuals. It is presented in form of a diagram known as use case diagram and in written descriptions. The use case diagram for the system is given below. 

![image](https://user-images.githubusercontent.com/51742367/59343975-aeeb5a00-8d15-11e9-903c-0ad0b4f5625f.png)

## 4. User Interface
The user interface acts as a toolbox in selecting and controlling the operation of the boom. The Graphical user interface is used to select the different states of operation while the joystick is used to provide input to the joints. 
### Graphical User Interface
The graphical user interface (GUI) provides a visual aid in choosing the different modes of operation. It was modelled using the instrument panel strip of the Simulink real time explorer. The modelled GUI is shown in fig
![image](https://user-images.githubusercontent.com/51742367/59344269-6bddb680-8d16-11e9-8319-69e899ea9901.png)

### Joystick
The joystick is used to provide the input values for joint control of the boom control system. The joystick used has 4 axes control and 12 buttons. The buttons communication has been terminated in the Simulink model. The axes are used for controlling the joints in joint velocity mode, boom tip in planar tip mode. In automatic mode the axis is used to control the velocity of the automatic boom movement.
![image](https://user-images.githubusercontent.com/51742367/59344459-cd9e2080-8d16-11e9-9071-60d0113e6bc8.png)

## 5. System Design
### High-Level Architecture
The high-level architecture of the control system comprises of the physical components and 
its structures, software components within these physical components, hardware, software 
and communication interfaces. 
 The joystick send and receive, UDP send and receive modules form the communication 
interface.
 IHA 3D (visualization software) and GUI Simulink real time explorer form the software 
components.
 The Joysticks, human-machine-interface PC and Real time PC form the hardware 
components.
 The interaction between the host and target PC is through Ethernet/UDP. 

# Model-Based-Real-Time-Control-and-Rapid-Prototyping
The aim of this exercise is to design and implement a real-time control system for a 4 degree-of- freedom hydraulic loader (i.e. boom) and test it in a rapid prototyping environment.

## 1. Introduction
The purpose and scope of this exercise are to design and implement a real-time control system for the given 4 degrees of freedom (DOF) hydraulic boom model and to test it in the rapid prototyping environment. 

In any product development phase, testing and validating the design is of utmost importance before the product production. Testing and validating any system for a range of parameters provides to be expensive and time-consuming when implemented on a real prototype. In these conditions, rapid prototyping systems comprising of hardware components can mimic actual behaviour of the desired system. 

One of the quickest and inexpensive way of testing, verifying and evaluating control design is by rapid prototyping. Over here, the term Rapid prototyping implies developing an executable model on software that can be continuously tested and refined to make it better. It allows creating a working implementation of signal processing or control design and verifying the same on the hardware in the early design process, before production. It is proof to validate the control designs. The continuous testing on the real-time hardware provides a platform for refining the correctness of the design. This way the model can be refined for perfection and accuracy while reducing the product development time. 

In this course project, a control system for a hydraulic loader was designed and implemented in the State flow environment of the MATLAB Simulink. Stateflow is an environment for modelling and simulating combinatorial and sequential decision logic based on state machines and flow charts. The online model is tested with the help of Simulink real time. IHA 3D is the visualization software used.
The content below provides a brief information  about the uses cases and requirements of the developed system. It starts with general description of the vehicle followed by the user interface, the system design, which includes the high-level architecture, real time system architecture, controller and finally the state machine. 

## 2. General Description
The equipment used is a forestry forwarder with a hydraulic boom system for loading and unloading logs. The operator controls the boom movements manually during loading and unloading by the aid of joysticks. The boom can be driven in two ways: 
 * Control and command each joint of the boom individually i.e. joint angles q1, q2, q3 and telescopic joint d4. 
 * Control and command the end of the boom along the planar coordinate axes i.e. joint angle q1, horizontal distance (x) and vertical distance (y) of the boom’s tip. 

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
The high-level architecture of the control system comprises of the physical components and its structures, software components within these physical components, hardware, software and communication interfaces. 
* The joystick send and receive, UDP send and receive modules form the communication interface.
* IHA 3D (visualization software) and GUI Simulink real time explorer form the software components.
* The Joysticks, human-machine-interface PC and Real time PC form the hardware components.
* The interaction between the host and target PC is through Ethernet/UDP. 
![image](https://user-images.githubusercontent.com/51742367/59345038-f672e580-8d17-11e9-9811-2c541e597606.png)

### Real-Time system Architecture
The system architecture mainly comprises of the communication module Joystick receive and UDP send, state machine modelled in the stateflow, controller and a boom model. The outline of the real time system architecture is shown in the fig.
![image](https://user-images.githubusercontent.com/51742367/59345213-54073200-8d18-11e9-9580-e503b07ce0b1.png)

### Controller
The system developed required three types of control of the system; open loop control in service mode, closed loop control in normal working mode and stop in safe state. Hence, we decided to use the multiport switch. It allows easy transition between different modes of 
control. 

The open loop control was the easiest to implement, as it has no feedback, the allocated port number was 2. The open loop control is used in the service mode. 

In the safe state mode or when the collision was detected the system had to stop all the boom 
movement. To achieve this we used a single column zero matrix to turn off all the movements. The port number is 3 for stop. 

The closed loop control utilize a Proportional controller. The output from the state machine is velocity this is integrated to obtain position before it is subtracted from measurement. The obtained output is multiplied by p-gain separately for each joint. Thus the obtained position is given to the boom control. The integrator has the 3 inputs namely velocity, initial condition which is the measurement at the start of every operation and reset. The reset function is used to reset the integrator when there is change of control method. The integrator saturation limits are the joint limits of the system.

### State Machine
The state machine was modelled in the Stateflow of Simulink using state chart and matlab functions. The transition to different states is with the aid of buttons on the GUI. MATLAB fucntions were used for automatic control, inverse kinematics for tip control in planar control mode and foward kinematics for self-collision detection.

## Implementation and Testing
The implementation and testing was done in two steps: off-line and then on-line implementation. The testing of off-line system was done by checking the display of the output joint values and visualizing the boom movements on IHA 3D software. Once the off-line model was functioning as per use cases, it was not difficult to implement the real-time model.  The communication interface module had to be configured. The GUI was made on the Simulink real time.

## Future Developments
The current model has appropriate operation logic. 
* This operation logic can be used to develop the controller for real time use. 
* The inverse kinematics function can be utilized for the tip control mode for the final product. 
* The forward kinematics function can be utilized for collision detection and works very well for the final product. 
* The GUI we have made for this project were according to final product standards, which could also be used in the control panel. 
* Automatic trajectory generation could be more improvised for the final product to reduce the vibrations when driving from the limits. 
### Recommendation
We are using P- controllers in our system, we know they do not exist in the real machines. We could use some better controllers like PID, which could solve the vibration issues and it would have given a better controllability for system when it is working in the closed loop. If we have a better controller, we could make the system work without vibrations at higher velocities. If the control panel could display messages in text format and could give voice commands, it would be great. 

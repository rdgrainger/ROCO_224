
## ROCO224 Introduction to Robotics

#### *ROSSUMOVI ROBOTI*

* Faisal FAZAL-UR-REHMAN
* Jack GELL
* Robert GRAINGER
* Spencer PERDOMO-DAVIES

## *Programming Industrial Robots*

### *Introduction*

Our task is to simulate a physical robot task using the 3D Automate Visual Components software.

This will be achieved by using a robot arm to move 2x2 blocks, included in our Lego Duplo kits. The blocks are to be transferred from one 'initial' state to another 'final' state and the arm must pass through a 'gate' of specific height in between. The pattern configuration will be unique to our group. Collision detectors will be taken into consideration during simulation as 5% of the marks will be deducted for each collision made during the task. 



### *Mitsubishi RV-2AJ and RV-SD*

The industrial robots that we are using will be from the Mitsubishi Industrial Robots RV-SD and 2AJ Series.

The applications of the RV-SD/RV-2AJ series range from straight forward pick and place tasks to complex assembly. This makes them a highly versatile tool designed for manufacturing enviroments. The specific purpose of these arms make them ideal for the completion of this assignment.

The specification of the RV-2SD and RV-2AJ series are as follows...

 
#### *Degree of Freedom*

* 6 (RV-2SD) and 5 (RV-2AJ)

#### *Installation posture*

* On floor hanging

#### *Structure*

* Vertical, multiple-joint type

#### *Drive system*

* AC Servo motor

#### *Position detection method*

* J1 to J3 : 50W brake, J4,J6 : no brake, J5 :15W with brake

#### *Type*

* Absolute Encoder

#### *Arm Length* 

* (RV-2AJ)

1. Shoulder shift - 0
1. Upper Arm - 250mm
1. Forearm - 160mm
1. 0mm
1. Wrist Length - 72mm

* (RV-SD)

1.  Upper Arm - 230 mm 
1.  Forearm - 270 mm   

#### *Motion Range (Degree)*
 
J1 -	300 (-150 to +150)(RV-2AJ)
	480 (-240 to +240)(RV-SD)

J2 - 	180 (-60 to +60)(RV-2AJ)
	240 (-120 to +120)(RV-SD)

J3 - 	230 (-110 to +120)(RV-2AJ)
	160 (-0 to +160)(RV-SD)

J4 - 	320 (-160 to +160)(RV-SD only) 
	400 (-200 to +200)(RV-SD)

J5 - 	180 (-90 to +90)(RV-2AJ)
	240 (-120 to +120)(RV-SD)

J6 - 	400 (-200 to +200)(RV-2AJ)
	720 (-360 to +360)(RV-SD)

#### *Speed Of Motion Degree/s*

J1 - 	180(RV-2AJ)
	225(RV-SD)

J2 - 	90(RV-2AJ)
	150(RV-SD)

J3 - 	135(RV-2AJ)
	275(RV-SD)

J4 - 412 (RV-SD only)
 
J5 - 	180(RV-2AJ)
	450(RV-SD)

J6 - 	210(RV-2AJ)
	720(RV-SD)
	

#### *Maximum Resultant Velocity*

* Approx. 4400mm/s (RV-SD)

* Approx. 2100mm/s (RV-2AJ)

#### *Load Capacity*

* Maximum - 3 kg (RV-SD) and 2 kg (RV-2AJ) 

* Rating  - 2 kg (RV-SD) and 1.5 kg (RV-2AJ)

Nb - The maximum load capacity is the mass with the flange posture facing downword at the ±10 degree limit.

#### *Position Repeatability*

±0.02mm

#### *Ambient Temperature*

0 to 40ºC

#### *Mass*

Approx. 19 kg (RV-SD) and Approx. 17 kg (RV-2AJ)

#### *Allowable Moment*

J4 4.17 Nm (RV-SD only) 
J5 4.17 Nm (RV-SD) and 2.16 Nm (RV-2AJ)
J6 2.45 Nm (RV-SD) and 1.10 Nm (RV-2AJ)

#### *Allowable Inertia*

J4 4.86 × 10^-2 kg m2 (RV-SD only)
J5 4.86 × 10^-2 kg m2 (RV-SD) and 3.24 × 10^-2 kg m2 (RV-2AJ)
J6 4 × 10^-2 kg m2 (RV-SD) and 8.43 × 10^-3 kg m2 (RV-2AJ)

#### *Arm Reachable Radius (front p-axis center point)*

504 mm (RV-SD) and 410 mm (RV-2AJ)


#### *Tool Wiring*

(RV-2AJ) Four input signals (Hand section),Four output signals (Base section),Motorized hand output (Hand section)

Nb - When using the 4-point hand output, the pneumatic hand interface (option) is required.

(RV-SD) Hand: 4 input points / 0 output points

#### *Tool Pneumatic Pipes*

4 × 4 (Base to hand section)

#### *Supply Pressure*

0.5 MPa (±10%) 

#### *Protection Specification*

IP30

"Ingress Protection" 

Solids Protection - (2.5mm) 

Tools, thick wires, etc.

Liquids Protection - Spraying water

Water falling as a spray at any angle up to 60° from the vertical shall have no harmful effect.
(Not Protected From Liquids)
 
[IP Rating Chart](http://www.dsmt.com/resources/ip-rating-chart/)

The industrial arm that we have been using is the RV-2AJ. The main reason that we had decided to use this model instead of the RV-2SD was due to suitability for the group, there was only one RV-2SD. We also had access to the RV-2AJ mesh in the Virtual Components software, so this really was just a matter of convenience.

<img src=https://github.com/slperdomo-davies/ROCO_224/blob/master/images/rv2.jpg alt="RV-2AJ Model"/>


### *Software*

The software that that we will be using, for the task previously mentioned, is Visual Components Premium 4.0 (previously 3D Automate).

This is a manufacturing simulation package that enables us to 'virtualize' a factory enviroment. The software allows us to import objects, such as the Mitubishi robot arm and various other components, to test a program safely and efficiently with a 3D simulation before implementing it in a real world enviroment. 

https://www.visualcomponents.com/

### *Relevance*

The simulation lets us recreate a real world task relatively quickly, moving the arm and setting key positions in order to visualise our intended objectives and detect pitfalls or unforseen problems safely before initiating our routine in a real world enviroment. This method is known as Off-Line programming.This can all be done with one person with relative ease. However, we do need to implement simulated dynamics and physics such as collision detectors etc.. in order to maintain as much accuracy as possible. This can be problematic when using variables such as friction and can cause our simulated routine to behave differently to what was intended when using the real robot.

When using the the real robot manually, known as online-Programming, we have to approach with care, which can take longer to set up and organise. This is due to human interaction with the Robot. When setting positions using the teach pendent, we need to be very aware of the enviroment surrounding the robot so we do not put anyone in danger and avoid accidents when programming it to achieve the tasks at hand. This has to be done with a minimum of two people and can never be done alone. However, when everything is prepared we found that we can get better and more accurate results using the robot in the real world as we can adjust movements in regards to physical varibles such as friction through trial and error which we can't necessarilly do with the simulation.  

Below is a list of other similar simulation OLP (Off-line Programming) software used ....

### 1. Delfoi Robotics (Finland)

This OLP "Utilises proven simulation technology platform provided by Visual components".

The software supports all major robot brands  The software is used for various industries including automotive, heavy machinery, aerospace, construction steel, and ship building. Today more than 400 industrial companies are using Delfoi Robotics applications. 

This software can simulate Arc/Laser welding, painting and cutting/milling and maching tasks.

Customers include..

Boeing, BAE Systems, Merecedes Benz, Toyota Group, SAAB, Volvo, ABB, Nokia, BT Industries.
 

### 2. FASTSUITE,CENIT (Germany)

This software specialises in robotic welding and is used by US company CROWN for the production of their forklifts.

### 3. Octopuz (Canada)

OCTOPUZ is optimized for various manufacturing applications such as welding (arc, spot, etc.), waterjet, plasma, and laser cutting, trimming, deburring, polishing, milling, machine tending, sanding, grinding, laser cladding, dispensing, inspection, painting, and robotic cell/process line simulations, in addition to custom applications that are requested by our customers. Customers with software development expertise can also do their own OCTOPUZ customization using its open API capabilities, thereby creating their own proprietary software solution.


### 4. RobotDK (Canada, Spain)

In addition to machining, welding, cutting, painting, inspection, deburring simulations, this software has an extensive library of industrial robot arms, external axes and tools from over 30 different robot manufacturers. 



### 5. Robotmaster (USA)

This software specialises in a more intuitive and user friendly approach to robotic simulation by automatically adjusting for singularities while maintaining the same functionalities as the software above. 



### 6. SprutCam Robot (Russia)

SprutCam is a powerful, high-performance, full-spectrum programming system for milling, turning, wire EDM, multi-tasking machine tools and robots.




## *Development*

Linked template code for mitsubishi arm.

https://mega.nz/#!yeAlXQAD!9WT7682P0vOglwi2xwGGpro0vAdTxhgiVgoNCQij1Kc


Before we learned how to simulate the robot arm using Visual Components, the first thing we needed to accomplish as a team was to learn how to manually use the Mitsubishi arm with the teach pendant and program it using Melpha for our individual driving tests.

We all took it in turns to manually move the arm into varying positions, create positions using the teach pendant, write code and we always had one person in charge of safety when we were using the arm.



```Final Melpha Code for building a 4 block tower```


```

'Below declares the joint position variables

'DEF POS IP
'DEF POS FP
DEF POS CAL1 (Cal stands for calibration, Each position from CAL1-4 moves the board into the correct position) 
DEF POS CAL2
DEF POS CAL3
DEF POS CAL4
DEF POS PI1 (PI stands for Initial position)
DEF POS PF1 (PF stands for Final position)
DEF POS PI2
DEF POS PF2
DEF POS PI3
DEF POS PF3
DEF POS PI4
DEF POS PF4


'Define the positions of the variables learned by the teach pendant

'IP = (293.440,2.820,423.020,90.580,177.150,0.000,0.000,0.000)(6,0)
FP=(359.630,1.030,223.940,-0.590,174.810,0.000,0.000,0.000)(6,0) 
CAL1=(409.440,-0.110,144.970,-89.840,173.380,0.000,0.000,0.000)(6,0) 
CAL2=(392.280,0.090,142.790,-89.850,173.380,0.000,0.000,0.000)(6,0)
CAL3=(286.420,231.850,139.400,0.240,177.160,0.000,0.000,0.000)(6,0)
CAL4=(286.430,188.980,139.390,0.240,177.150,0.000,0.000,0.000)(6,0) 
PI1=(337.760,133.210,166.100,0.240,177.160,0.000,0.000,0.000)(6,0) 
PF1=(293.080,3.170,167.480,90.590,177.160,0.000,0.000,0.000)(6,0)
PI2=(244.500,130.480,165.320,90.590,177.160,0.000,0.000,0.000)(6,0)
PF2=(292.660,5.210,180.480,90.590,177.160,0.000,0.000,0.000)(6,0)
PI3=(243.250,-123.460,165.780,90.590,177.160,0.000,0.000,0.000)(6,0)
PF3=(289.960,3.360,202.290,90.590,177.160,0.000,0.000,0.000)(6,0)  
PI4=(341.470,-123.340,167.210,90.590,177.160,0.000,0.000,0.000)(6,0) 
PF4=(293.000,3.430,223.940,90.590,177.160,0.000,0.000,0.000)(6,0)   

SERVO ON (Turns servos on)
OVRD 20 (Sets speed)

'Initial Position
MOV FP
'HOPEN 1 (Opens gripper)

'Calibration Top
MVS CAL1, -50 (MVS moves arm using cartesian coordinates 'straight movements' enabling us to use offsets e.g -50)
HOPEN 1
DLY 0.5 (DLY (Delay allows us to adjust movement)
MOV CAL1 (MOV moves the arm using joint angles)
DLY 0.5
OVRD 5
MOV CAL2 
DLY 0.5
MVS CAL1, -50
OVRD 20
DLY 0.5

'Calibration Side
HOPEN 1
MVS CAL3, -50
DLY 0.5
MOV CAL3
DLY 0.5
OVRD 5
MOV CAL4
DLY 0.5
MVS CAL3, -90
OVRD 20
DLY 0.5


MVS PI1, -50
HOPEN 1
DLY 0.5
MOV PI1
OVRD 1
DLY 0.5
HCLOSE 1 (Closes gripper)
DLY 0.5
MVS PI1, -20
OVRD 20
DLY 0.5
MVS PF1, -50
OVRD 5
DLY 0.5
MOV PF1
DLY 0.5
HOPEN 1
OVRD 20
MVS PF1, -80
DLY 0.5

MVS PI2, -50
HOPEN 1
DLY 0.5
MOV PI2
OVRD 1
DLY 0.5
HCLOSE 1
DLY 0.5
MVS PI2, -20
OVRD 20
DLY 0.5
MVS PF2, -50
OVRD 5
DLY 0.5
MOV PF2
DLY 0.5
HOPEN 1
OVRD 20
MVS PF2, -50
DLY 0.5

MVS PI3, -50
HOPEN 1
DLY 0.5
MOV PI3
OVRD 1
DLY 0.5
HCLOSE 1
DLY 0.5
MVS PI3, -20
OVRD 20
DLY 0.5
MVS PF3, -50
OVRD 5
DLY 0.5
MOV PF3
DLY 0.5
HOPEN 1
OVRD 20
MVS PF3, -50
DLY 0.5

MVS PI4, -50
HOPEN 1
DLY 0.5
MOV PI4
OVRD 1
DLY 0.5
HCLOSE 1
DLY 0.5
MVS PI4, -20
OVRD 20
DLY 0.5
MVS PF4, -50
OVRD 5
DLY 0.5
MOV PF4
DLY 0.5
HOPEN 1
OVRD 20
MVS PF4, -90
DLY 0.5




'///////////////////////////////////////////////////////////


SERVO OFF (Turns servos off)
END (End program)

```

[!](https://www.youtube.com/watch?v=bEZeHT-B7rM)



### *Visual Components*

The next step was to simulate our arm using Visual components.

This part of the process was a little tricky, as in earlier years, 3D automate was used, so part of the challenge was learning how to navigate around Visual Components as this is a newly acquired program and not a lot of people had used it before.

However, with the help and guidance from Jake we were able to make sense of the program.

First thing we did was open up the template file which included the RV-2AJ model and virtual Lego blocks and started to configure the positions. We opted to use initial state i1 and final state f1. 

<img src=https://github.com/slperdomo-davies/ROCO_224/blob/master/images/IntialState_1.JPG alt="I1"/>

<img src=https://github.com/slperdomo-davies/ROCO_224/blob/master/images/Final%20State_1.JPG alt="F1"/>

There were 10 states to choose from. The i1/f1 configuration was chosen at random as there was no real advantage between the states, as we were to find out at a later stage.

We set up our enviroment to match the initial states of the blocks, seen in the pictures above, by first selecting the Model tab in visual components and then moving all the blocks into that configuraion ready for simulation.

Originally we had created an extra piece of geometry to act as the foam support to replicate our real world base for the Lego blocks with a height of 32mm. However, this became problematic and we encountered an issue with the arms reach. The end effector could not properly gravitate towards the corners of the board. The reach of the arm appeared to be inhibited within certain parameters as seen from the screen shots below...

<img src=https://github.com/slperdomo-davies/ROCO_224/blob/master/images/Max_dist_Yneg.JPG alt="Max Distance"/>

<img src=https://github.com/slperdomo-davies/ROCO_224/blob/master/images/Max_dist_Ypos.JPG alt="Max Distance"/>

At first, we thought this to be part of the challenge and tried other state configurations from the 10 provided. Realising that there was no advantage to which state we picked, we reverted back to our original I1 block configuration. We then proceeded to move the board into a reachable position using the gripper before picking and placing the blocks. Unfortunately, this affected the functionality of the gripper. 

After reading and watching various tutorials from visualcomponents.com and solicad.com  we had a good understanding as to how the signal functions worked for the gripper. This is achieved by selecting the gripper/end effector and setting the signal for the gripper tool to a value of 1 and selecting it to be true or false which will emulate the gripper opening and closing. We then adjusted all the geometry, excluding the arm itself, to have physical properties and made them into collision objects so that the blocks and the board could interact with each other when they collide. 

This approach worked to a certain degree. The blocks would move with the base of the board when the end effector moved it into position. However, we found that the gripper would not release the blocks as it did before. So after a lot of online research we found that this was not supposed to happen and should have worked. Our conclusion to this problem was that there may be an error with our installed version of Visual Components.

### *Results*

We all decided to start from scratch and remove the foam layer from the simulation and moving the board closer to the base of the RV-2AJ. We refrained from using collision physics and opened a fresh template of our enviroment. To our amazement, we found that there no longer was an issue with the reach of the arm. It could now reach all four corners of the base. We then proceeded to set the positions of the arm to meet the challenge of the task. After we had finished setting the positions, we simulated it to make sure that we met all the conditions. 

The next step was to make sure the blocks were not touching and the gripper was not being obstructed in any way. This is quite difficult to observe by eye, so we decided to include collision detectors into our simulation. Straight away we discovered that the gripper was being obstructed as the detectors turn to objects that are colliding yellow and stop the simulation. 

<img src=https://github.com/slperdomo-davies/ROCO_224/blob/master/images/Collision%20detectors.JPG alt="Collision Detectors"/>

Luckily for us,this was a lot easier to amend than originally thought. We simply adjusted and fine tuned the positions by using the touch up function, which is an option when selecting our original arm positions.

As you can see, in the video below, the simulation works perfectly. However, we learned that we need to go through the task in a certain sequence of steps in order for the simulation to work properly, as stated earlier, making sure all the positions and signals are set and configured before implementing the collision detectors. We also needed to completely disregard the instructions for 3d automate due to the interface being completley different.


### Final Simulation Of The Task

After we were happy with our simulation we exported it out as an .AVI file which you can see in the link below...

[!](https://www.youtube.com/watch?v=qKohdm0BTBY)

### *Conclusion*

Although we had initial problems with Visual components, we were finally able to create a smooth simulation for the task.

The simulation itself is very useful if one wants to create, visualise and demonstrate a concept for a particular task quickly and safetly. However, there is always a need, in our opinion, to physically test a robot and the code in a real world enviroment. This is due to there being challenges that cannot always be picked up on the simulation i.e friction, weight of objects etc... The simulation is still an important tool if used as a template as it increases your time efficency when setting up an enviroment for any given task and implementing any "online line programming". So in short, we found that in order to achieve any task of this nature it is more effective to use a strategy of incoprporating both methods to compliment each other making best use of the robot arm.

#### *References*

http://www.mitsubishirobots.com
https://www.delfoi.com
http://www.fastsuite.com
https://octopuz.com
https://robodk.com
http://www.robotmaster.com
https://www.sprutcam.com


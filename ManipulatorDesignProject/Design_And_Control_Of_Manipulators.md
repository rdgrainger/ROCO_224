#### |---------These are some useful HTML/Markdown commands that we can use while we are writting this report and delete before submission-----------|
_These commands are only visible in the raw format or in gedit_

This command puts two pictures side by side, change percentage for size but always keep height and width the same:  
<img src="" height="0%" width="0%"/><img src="" height="0%" width="0%"/>

For single image:  
![name](image link)

For video:  
[![](picture link)](video link)

Use this to add blank space if required: (each <br> adds one line of space use as many as like)
<br><br>

#### |--------------------------------------------------------------------------------------------------------------------------------------|




## Design and Control of Manipulators 

Design and control of manipulators (approximately 3000 words)

## *Introduction*

The Design and Control of Manipulators is one section the roco224 coursework, alongside ‘Programming Industrial Robots’ It consists of a student-driven group design project, where the goal is to fulfil a chosen task related to the field of robotic manipulators. Our group is named ‘Rossumovi Roboti’ and consists of four members:

Spencer Perdomo-Davies

Robert Grainger

Jack Gell

Faisal Fazal-Ur-Rehman

There are 8 project challenges to choose from, though the group could design a new challenge if desired. After some discussion, our group has settled on challenge 4 – The Redundant Freedom Arm. However, due to our large group size we have also decided to incorporate elements of 2 – Pick and Place. This will allow us to allocate a suitable quantity of work amongst ourselves.


### Description of hardware 

### Description of task

Our manipulator will consist of two segments. 

Section A – the end effector, a claw to grip and position objects. 

Section B – the redundant arm, a series of RX-28 servos to showcase redundancy and position the end effector.

We therefore have two main features to showcase – the redundancy and the ability to pick and object up. The idea is to have the arm manoeuvre into an enclosed area or around an obstacle. The claw will pick up an object, and then section B of the arm will move whilst keeping the end effector in place, demonstrating redundancy 

### An overview of your approach to the problem

## *Design Process*

Our design process began with sketching and discussing ideas for ‘section B’ of the arm. We decided early on to have the servos attached together with short brackets rather than limb-like links. Our reasoning was to reduce weight, length and overall complexity. With a shorter arm weight and length, the servos will experience less force and can operate more efficiently.

Once we had a general idea of section B, we split the group in two – a pair working on section A and another to continue section B.

For section A, the end effector, we investigated different claw designs, sketching these ideas on paper. Once we had a design we felt was feasible, we created parts in fusion 360 and made an assembly of these parts to model the claw as one unit.
The end effector was to include a 9g servo and the claw would be 3D printed as separate parts then assembled.


Since the arm has 8 servos, the idea behind the design was to keep it as light as possible. We decided to directly attach each servo on to the previous one without any links between them except for one link that goes after the shoulder joint. After we had a rough idea of our design, the first thing we did was to make sure that the RX-28 servos (especially the servo that will need maximum ammout of torque) will be able to deliver the required torque for the design, for this we derived the moment formulas for our design.


_|------------ Final maths for moments ------------|_

word document for torque equation at 90 degrees. how to calculate work for the servo which will be under the most
stress. The second page account for when not at 90 degrees.

_|----------------------------------------------------|_

### What design iterations have you gone through?

End Effector Design Iterations (very rough ideas):
Sliding gripper model – 

![Sliding Gripper](https://github.com/rdgrainger/ROCO_224/blob/master/images/Manipulator%20-%20Sliding%20Claw%20design.jpg)

Early Arm design - 

![Arm ideas](https://github.com/rdgrainger/ROCO_224/blob/master/images/Manipulator%20-%20Early%20Arm%20designs.jpg)

Spur gear design -  The right hand drawing is the basic principle we decided to take forward to the real design. A 9g servo will rotate one cog and thus operate one side of the claw. Another cog has interlocking teeth with the first and so will also spin, rotating the second claw part.

![2 gear desgin](https://github.com/rdgrainger/ROCO_224/blob/master/images/Manipulator%20-%20Spur%20Gear%20Claw%20design.jpg)

Top left - an idea on how to test redundancy by having the arm investigate through a hole. Right - An idea for a way to have 3 degrees of freedom in one segment

![Experiment](https://github.com/rdgrainger/ROCO_224/blob/master/images/Manipulator%20-%20%20Experiment%20idea%20and%20joint%20design.jpg)

One idea was to use the end effector to scan a area from left to right, then move down one space and keep on repeating to build an image of the area below, made of diffrent colour characters. Each colour pixel would represent the distance of the surface detected. Essentially this would capture an image and send it to a window in the terminal. By utilising a serial input you could let the end-effector know which pixel to move towards. This did manage to successfully work but it took a lengthy time to capture a image. It was aslo bottle necking processing speed and we decided it was far too taxing to keep it included within the code.


### Who worked on what and when?

### What problems did you encounter, and how did you solve them?

Our original design for the redundant arm had 6 dynamixel servos in line. We realised an issue may arise with the second servo from the base, which would have to lift the weight of all the subsequent servos and the end effector. To account for this we decided to attach an additional servo in parallel to help spread the load of the rest of the arm. The first servo is not an issue because it is roatational asn will not have to act against the weight of the others. To further combat weight concerns, we streamlined he desgin to include very few printed parts.

The end effector claw is operated by a 9 gram servo. This has to be connected to the arduino at the base of the arm with wire. The issue - these wires need to be slack enough to allow the arm joints to bend, but we also wanted to avoid loose wires that could become tangled when the arm moves. Our solution to this was to join attach these wires closely to the servos, but at joints the wire would loop slightly to aacount for the joint angle changing.


## *Implementation*

### Describe the final implementation of your manipulator

### Include figures with images/drawings etc

### Include your solutions for kinematics, motion planning etc.

### Be brief, include support material in appendices, if need be

#### Part A - End Effector

The end-effector for the redundant arm is a flat - edged gripper which is to be operated by a servo. The user will be able to control this servo from the serial terminal (created using program PuTTY, interfaced with the arduino) and information will be fed back on the angle and the state of the gripper.

##### Printing
This End-effector is to be 3D printed from PLA for the following reasons.
PLA is NOT harmful to the environment.
This end-effector does not need the added strength or higher melting point advantages that ABS offers.
PLA is more aesthetically pleasing.

The only advantage we can see for having a ABS print is that the specific gravity is lower than PLA.
![PLA vs ABS](https://github.com/rdgrainger/ROCO_224/blob/master/images/specific%20gravity.jpg)

##### Control

As mentioned above the end-effector will be controlled with the serial input to putty. We will be displaying a user interface using a arduino mega which will take data from the keyboard in order to control the end-effector.

This is the End-effector portion of of the terminal is shown below and is kept towards the bottom left of the terminal As seen the object distance relates to the ultra sonic sensor. Rotation @ x degrees is changable using the Q and E key from the keyboard and the gripper and be opened and closed by using the A and D keys.

![Terminal window](https://github.com/rdgrainger/ROCO_224/blob/master/images/Terminal%20window.jpg)

Design
We wanted our end-effector to work abit like a pincer but not exactly, we have chosen this style of design so that out gripper can reach forward and grab onto a object. With the design shown below we can lay a good amount of surface area on the object but still move in a pincer like fasion. Part of the gripper is hollowed out to save on weight and it also looks nicer, another way to save weight was not to encase the servo entirely although that was the original idea i realised that it was pointless and shorted it down.

'image'

'image' 

As seen here one servo operate both of the gripper by using cogs. This ensures that both the arms are in mirror image with each other and saves weight of not have an extra servo, and the weight required from PLA to house another servo. 

'alt text' 

By looking closely there are holes so that the servo can be screwed into place. 

'alt text'

Individual components

'7 images'

'exploded view video'

#### Part B - Redundant arm



					Rough workings for moments:  
<img src="https://github.com/Faisal-f-rehman/ROCO_224/blob/master/ManipulatorDesignProject/maths/moments%20drawing%20at%200%20degrees%20redundant.jpg?raw=true" height="30%" width="30%"/><img src="https://github.com/Faisal-f-rehman/ROCO_224/blob/master/ManipulatorDesignProject/maths/moments%20calc%200%20degrees%20redundant.jpg?raw=true" height="30%" width="30%"/><img src="https://github.com/Faisal-f-rehman/ROCO_224/blob/master/ManipulatorDesignProject/maths/moments%20about%20servo%20B%20at%2090%20degrees%20redundant.jpg?raw=true" height="35%" width="35%"/>

<br><br><br><br>

**Libre writer file:**
![Jack's Torque file](https://github.com/Faisal-f-rehman/ROCO_224/blob/master/ManipulatorDesignProject/maths/JackGell_torque%20equation_%2090degres.docx)

## *Experiments* 

### What testing have you performed of the robot (or subsystems)

### How well have you been able to perform the goal task?

### Include a Method, Results, and Discussion sub-section for each experiment

## *Conclusion*

### Briefly conclude this part of the coursework

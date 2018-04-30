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

End Effector Design Iterations:
Sliding gripper model – 

![Sliding Gripper](https://github.com/rdgrainger/ROCO_224/blob/master/images/Manipulator%20-%20Sliding%20Claw%20design.jpg)

Early Arm design - 

![Arm ideas](https://github.com/rdgrainger/ROCO_224/blob/master/images/Manipulator%20-%20Early%20Arm%20designs.jpg)


### Who worked on what and when?

### What problems did you encounter, and how did you solve them?

## *Implementation*

### Describe the final implementation of your manipulator

### Include figures with images/drawings etc

### Include your solutions for kinematics, motion planning etc.

### Be brief, include support material in appendices, if need be

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

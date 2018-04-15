
# Arduinocode overview

The tasks for the arduino is to display information to a terminal, Read positions of the dynamixels, control the stepper motor
at the base of the arm and operate the end effector. While also looking presentable and easy to understand for any user.

Initially i started with a arduino uno which did not last long as the memory space was to small and causing instability issues.
So the Uno has been upgraded to a Arduino mega 2560 with 16MHz clock speed (8MHz for uno). I was also able to increase the baud rate
to 57600 up from 9600. I did test with a baud of 115200 but errors where occoring fairly frequently and i felt
it was to unreliable for presentability.

the terminal is going to be initialized using the vt100 esc sequences to position the terminal cursor, change text colours
and afew other minor things. The code will be stuck in the initialize loop untill the terminal is detected by the arduino,
Then the basic borders for the terminal setup will be printed.

For the End Effector we will be able to control both of the servos by interacting with the terminal. One servo will
control rotation so there will be two control switches to move clockwise and counter clockwise. The position of this
servo will be fed back to the terminal and displayed and degrees. The second servo will open and close the gripper
and will feed back to the terminal whether the gripper is open or closed. I did open a 9g servo and solder a wire
into its internal potentiometer so that could have feedback. and with this feedback i could determine the percentage of
how open the gripper was and the width of the object which was being grabbed. Unfortunatly they are very fragile and
the rotation became unreliable after reassembling.

Data regarding the dynamixels will also be displayed. As listed on the the RX-28 data sheet that the servos are capable of feeding
back the load of which they are under. Their is also the capability to feed more data like rotation postion and temperature but
for this project we are more concerned about the load the dynamixels are going to be under.

As the Load for the dynamixels is large we are trying to double up as many dynamixels as possible. One way to do this was
to have a stepper motor on the base of the arm where it meets the frame. however the position of which the arm is
going to be has to be sent down two lines. One being to the dynamixels and the other being to the stepper via the arduino
therefore the arduino is going to have to save its current position and calculate where to go from there. This
increases room for error and also increases latency. A good method to counter latency is rapid polling and to avoid using
delays where possible.

For each dynamixel the X, Y and Z positions in space are going to be given to the arduino and displayed to the right
hand side of the termianl.

Below I am going to list each function or block of code and give a breif explination of how they work and
how they are going to be used. At the very bottom of the page there is a full summarization of the code
amalgumated into their main sketch.

# Functions with operations and reasons explained.

### void text_colour(int colour)_

Function that will use the vt100 escape sequnces to change the colour of the text writing that is put onto a terminal. If you
call this function use a single argument from 1 - 7 and text colour will change respectivly to what is commeted by the case condition.

```c
   
void text_colour(int colour){
  
  switch(colour){
    case(0):  //black
      Serial.print("\x1b[30m");
      break;
      
    case(1):  //red
      Serial.print("\x1b[31m");
      break;
      
    case(2):  //green
      Serial.print("\x1b[32m");
      break;
      
    case(3):  //yellow
      Serial.print("\x1b[33m");
      break;
      
    case(4): //blue
      Serial.print("\x1b[34m");
      break;

    case(5): //magneta
      Serial.print("\x1b[35m");
      break;
      
    case(6):  //cyan
      Serial.print("\x1b[36m");
      break;

    case(7):  //white
      Serial.print("\x1b[37m");
      break;
  }
}

```

### void move_cursor(int row, int col)

Function also using the vt100 esc codes, this function take two arguments and (for selecting a row and a coloumn) and moves the cursor
to that location on the terminal without effecting any other text that the cursor is moving over. it does this by looping through the
rows one space a time. Then the cursor will move across the by looping through the columns.

To break this down into three sequences. Move cursor home, move cursor down x times and move cursor right x times.

```c

void move_cursor(int row, int col)
{
  int i;          //return home
  Serial.print('\x1b');   // ESC
  Serial.print('[');    
  Serial.print('H');
  for(i=0; i<row; i++)
  {
  Serial.print('\x1b');   // ESC
  Serial.print('[');    //move x amount of rows
  Serial.print(1);
  Serial.print('B');
  }
  for(i=0; i<col; i++)
  {
  Serial.print('\x1b');   // ESC
  Serial.print('[');    //move x amount of columns
  Serial.print(1);
  Serial.print('C');
  }
}

```

### void set_terminal()

This is one of the functions that sets the terminal borders. The function got very large so made some smaller ones that will also 
be used within the initialization. Most of the terminal display will only need to be printed once ie the borders and options.

For this peice of the init the servo x , y and z coordinate are go to be displayed in the column that goes down the right
hand side of the panel. And the lower left/ lower mid section are to display endeffecter information and to
show what the serial in commands to us for operating the claw. 

All of the borders are printed in magneta and variables being printed in cyan. Yellow will be used
to display static text and names. Although variables are generally printed in cyan for the x,y and z
coordinates i have chosen to print Red for X, green for Y, and blue for the Z coordinate.

The comments (numbers) above and to the right of the of the first block within this function is
so it is easy to refrence a target postion on the terminal for the move cursor function.


```c

void set_terminal(){
  
  text_colour(5);
  //                                                                                                                 
  //              0000000000111111111122222222223333333333444444444455555555556666666666777777777788888888889999999999      
  //              0123456789012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789       
  Serial.println("####################################################################################################"); //0
  Serial.println("#                              #                     #                     #                       #"); //1
  Serial.println("#                              #####################################################################"); //2
  Serial.println("#                              #                     #                     #                       #"); //3
  Serial.println("#                              #############################################                       #"); //4
  Serial.println("#                              #                     #                     #########################"); //5
  Serial.println("#                              #############################################                       #"); //6
  Serial.println("#                              #                     #                     #                       #"); //7
  Serial.println("#                              #####################################################################"); //8
  Serial.println("#                              #                     #                     #                       #"); //9
  Serial.println("#                              #############################################                       #"); //10
  Serial.println("#                              #                     #                     #########################"); //11
  Serial.println("#                              #############################################                       #"); //12
  Serial.println("#                                                    #                     #                       #"); //13
  Serial.println("####################################################################################################"); //14
  Serial.println("#                                                                          #                       #"); //15
  Serial.println("############################################################################                       #"); //16
  Serial.println("#                                     ##                                   #########################"); //17
  Serial.println("############################################################################                       #"); //18
  Serial.println("#                  ####               ##               ####                #                       #"); //19
  Serial.println("####################################################################################################"); //20
  Serial.println("#                  ==>>               ##               ==>>                #                       #"); //21
  Serial.println("#                  ==>>               ##               ==>>                #                       #"); //22
  Serial.println("####################################################################################################"); //24
/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
//Display right hand panel

  text_colour(3);
  move_cursor(1 ,76);
  Serial.print("    Servo Positions    ");
  move_cursor(3 ,76);
  text_colour(3);
  Serial.print("        Servo 1        ");
  move_cursor(4 ,76);
  text_colour(1); Serial.print("X:0.000 "); text_colour(2); Serial.print("Y:0.000 "); text_colour(4); Serial.print("Z:0.000"); text_colour(3);
  move_cursor(6 ,76);
  Serial.print("        Servo 2        ");
  move_cursor(7 ,76);
  text_colour(1); Serial.print("X:0.000 "); text_colour(2); Serial.print("Y:0.000 "); text_colour(4); Serial.print("Z:0.000"); text_colour(3);
  move_cursor(9 ,76);
  Serial.print("        Servo 3        ");
  move_cursor(10,76);
  text_colour(1); Serial.print("X:0.000 "); text_colour(2); Serial.print("Y:0.000 "); text_colour(4); Serial.print("Z:0.000"); text_colour(3);
  move_cursor(12 ,76);
  Serial.print("        Servo 4        ");
  move_cursor(13 ,76);
  text_colour(1); Serial.print("X:0.000 "); text_colour(2); Serial.print("Y:0.000 "); text_colour(4); Serial.print("Z:0.000"); text_colour(3);
  move_cursor(15 ,76);
  Serial.print("        Servo 5        ");
  move_cursor(16 ,76);
  text_colour(1); Serial.print("X:0.000 "); text_colour(2); Serial.print("Y:0.000 "); text_colour(4); Serial.print("Z:0.000"); text_colour(3);
  move_cursor(18 ,76);
  Serial.print("        Servo 6        ");
  move_cursor(19 ,76);
  text_colour(1); Serial.print("X:0.000 "); text_colour(2); Serial.print("Y:0.000 "); text_colour(4); Serial.print("Z:0.000"); text_colour(3);
  move_cursor(21 ,76);
  Serial.print("        Servo 7        ");
  move_cursor(22 ,76);
  text_colour(1); Serial.print("X:0.000 "); text_colour(2); Serial.print("Y:0.000 "); text_colour(4); Serial.print("Z:0.000"); text_colour(3);


////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
//Claw control panel

text_colour(3);
move_cursor(19 ,1);
Serial.print("   Rotate claw    ");
move_cursor(19 ,23);
Serial.print("  User control ");
move_cursor(19 ,40);
Serial.print(" H open/close  ");
move_cursor(19 ,59);
Serial.print("  user control  ");

move_cursor(21 ,1);
Serial.print("    Clockwise     ");
move_cursor(21 ,23);
text_colour(6);
Serial.print("       Q       ");
text_colour(3);
move_cursor(21 ,40);
Serial.print("    Open       ");
text_colour(6);
move_cursor(21 ,59);
Serial.print("       A        ");
text_colour(3);

move_cursor(22 ,1);
Serial.print("Counter clockwise ");
move_cursor(22 ,23);
text_colour(6);
Serial.print("       E       ");
text_colour(3);
move_cursor(22 ,40);
Serial.print("    Close      ");
move_cursor(22 ,59);
text_colour(6);
Serial.print("       D        ");
text_colour(3);

move_cursor(17 ,9);
Serial.print("Rotation @");
move_cursor(17 ,19);
text_colour(6);
Serial.print(" 000 ");
move_cursor(17 ,24);
text_colour(3);
Serial.print(" degrees");
text_colour(3);
move_cursor(17 ,47);
Serial.print("object distance ");
text_colour(6);
move_cursor(17 ,63);
Serial.print("00 cm");
move_cursor(15 ,32);
text_colour(3);
Serial.print("End Effector");

}

```

### void Print_Load()

Another serial print Function within the initialisation which pushes
text to the monitor most of this code will not be changed. All that is
to be changed is where the variables are printed. when data comes in
these characters will be printed over with current data being fed from
the dynamixels about how much load they are under at the current point in
time



```c

void Print_Load(){

  text_colour(3);
  move_cursor(1, 57);
  Serial.print("Load on servos");

  text_colour(3);
  move_cursor(3, 55);
  Serial.print("Servo 1: ");
  text_colour(6);
  Serial.print(0.000);
  Serial.print(" N/m  ");

  text_colour(3);
  move_cursor(5, 55);
  Serial.print("Servo 2: ");
  text_colour(6);
  Serial.print(0.000);
  Serial.print(" N/m  ");

  text_colour(3);
  move_cursor(7, 55);
  Serial.print("Servo 3: ");
  text_colour(6);
  Serial.print(0.000);
  Serial.print(" N/m  ");

  text_colour(3);
  move_cursor(9, 55);
  Serial.print("Servo 4: ");
  text_colour(6);
  Serial.print(0.000);
  Serial.print(" N/m  ");

  text_colour(3);
  move_cursor(11, 55);
  Serial.print("Servo 5: ");
  text_colour(6);
  Serial.print(0.000);
  Serial.print(" N/m  ");

  text_colour(3);
  move_cursor(13, 55);
  Serial.print("Servo 6: ");
  text_colour(6);
  Serial.print(0.000);
  Serial.print(" N/m  ");
 
}


```

### void Print_Proximity()

Same sort of function as the previous to be run with in the init loop. This time to
display the proximty sensor panels of the terminal.



```c

void Print_Proximity(){

  text_colour(3);
  move_cursor(1, 34);
  Serial.print("proximity sensors");

  text_colour(3);
  move_cursor(3, 35);
  Serial.print("Sensor 1: ");
  text_colour(6);
  Serial.print(00);
  Serial.print("  cm ");

  text_colour(3);
  move_cursor(5, 35);
  Serial.print("Sensor 2: ");
  text_colour(6);
  Serial.print(00);
  Serial.print("  cm ");

  text_colour(3);
  move_cursor(7, 35);
  Serial.print("Sensor 3: ");
  text_colour(6);
  Serial.print(00);
  Serial.print("  cm ");

  text_colour(3);
  move_cursor(9, 35);
  Serial.print("Sensor 4: ");
  text_colour(6);
  Serial.print(00);
  Serial.print("  cm ");

  text_colour(3);
  move_cursor(11, 35);
  Serial.print("Sensor 5: ");
  text_colour(6);
  Serial.print(00);
  Serial.print("  cm ");
}

```

### void stepper(int xw)

There are 8 diffrent combinations of setting the magnetic direction of this four phase motor.
This function chooses which of these coils will be activited. The switch statement for steps
will be a value from 0-7 and will ascend or decend respectivly to the direction the motor will
will be rotating in. therfore if you put a in put value to the function it will take that many steps
in the direction of which it is asking.

This Stepper has a gear ratio of 64 and a stride angle of 5.625 degrees. For a full revoluion we can use this equation.

![alt text](https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/30656934_545292452552383_6157043265399106207_n.jpg?_nc_cat=0&oh=16d0d8dd009644d46dec0f4491f9af12&oe=5B5960E3 "Logo Title Text 1")

[Datasheet 28BYJ-48 - 5V Stepper Motor](http://robocraft.ru/files/datasheet/28BYJ-48.pdf)



```c

void stepper(int xw){
  for (int x=0;x<xw;x++){
    switch(Steps){
      
       case 0:
         digitalWrite(IN1, LOW); 
         digitalWrite(IN2, LOW);
         digitalWrite(IN3, LOW);
         digitalWrite(IN4, HIGH);
       break; 
       
       case 1:
         digitalWrite(IN1, LOW); 
         digitalWrite(IN2, LOW);
         digitalWrite(IN3, HIGH);
         digitalWrite(IN4, HIGH);
       break; 
       
       case 2:
         digitalWrite(IN1, LOW); 
         digitalWrite(IN2, LOW);
         digitalWrite(IN3, HIGH);
         digitalWrite(IN4, LOW);
       break; 
       
       case 3:
         digitalWrite(IN1, LOW); 
         digitalWrite(IN2, HIGH);
         digitalWrite(IN3, HIGH);
         digitalWrite(IN4, LOW);
       break; 
       
       case 4:
         digitalWrite(IN1, LOW); 
         digitalWrite(IN2, HIGH);
         digitalWrite(IN3, LOW);
         digitalWrite(IN4, LOW);
       break; 
       
       case 5:
         digitalWrite(IN1, HIGH); 
         digitalWrite(IN2, HIGH);
         digitalWrite(IN3, LOW);
         digitalWrite(IN4, LOW);
       break; 
       
       case 6:
         digitalWrite(IN1, HIGH); 
         digitalWrite(IN2, LOW);
         digitalWrite(IN3, LOW);
         digitalWrite(IN4, LOW);
       break; 
       
       case 7:
         digitalWrite(IN1, HIGH); 
         digitalWrite(IN2, LOW);
         digitalWrite(IN3, LOW);
         digitalWrite(IN4, HIGH);
       break; 
       
       default:
         digitalWrite(IN1, LOW); 
         digitalWrite(IN2, LOW);
         digitalWrite(IN3, LOW);
         digitalWrite(IN4, LOW);
       break; 
    }
SetDirection();
  }
} 

```

### void SetDirection()

At the bottom of the last function we see a function call named set direction. Which is a simple
funtion as it says it will add or subtract a value from the steps integer by doing this it reverses
the sequence of how the switch statement in how the last function selects which cases to use.

So if Direction = 1 the cases run from top to bottom. If direction = 0 the case are selected in a order from bottom
ascending to the top.

Whenever the function ascends to exceed 7 or decends below 0 this will revert it back to the start of its
loop respectivly of which direction the motor is rotating.


```c

void SetDirection(){
  if(Direction==1){ Steps++;}
  if(Direction==0){ Steps--; }
  if(Steps>7){Steps=0;}
  if(Steps<0){Steps=7; }
}

```

### void Rotate_Arm()

At the very start we call Map_steps(); which will be explained in the next function explination but
essentially it takes a value and configures it into somthing more meaningfull for the Rotate_Atm()
function. So once that is done the function will check whether or not steps are needed to be taken.
If so then it will test against a timer to make sure that the stepper is not spinning to fast. Then
we save a value for time that will be used as a argument in the next cycle of the loop and then
subtract one from the number of steps to account for the step that is has just taken.

```c

void Rotate_Arm(){
  
  Map_steps();
  
  while(steps_remaining>0){
    current_time = micros();
    if(current_time-Saved_time>=1000){
      stepper(1); 
      time=time+micros()-Saved_time;
      Saved_time=micros();
      steps_remaining--;
    }
  }
}

```

### void Map_steps()

Map_Steps will take a postion which we are told to go to and calculate how many steps to get
theirs using our current position and in which direction to travel in. The first statement
simply asks if the new postion is a lower position than the current degree. So that
the stepper know this we know which direction to go
so if this argument is true it will be put into a reverse direction.

Then next we work out what the diffrence is between the two positions we are travels from and to.
By doing this we need to make sure that we dont get a negative value so multiply it by negative 1
to revert it back to a positive as we have already solved for which direction to send it to in the 
last if statement. After this we now have a direction of travel and how far to travel in but the
variable value is still in degrees so if their are 4096 steps in 360 degrees we can then simply
multiply any value of degrees by 11.75 and then we would of made a conversion of degrees to steps. 

```c

void Map_steps(){
  //4095 for a 360 degree rotation
  //Target postion will a value inbetween 0 and 360 degree
  
  if(Target_Position<Current_Position){
    Direction=!Direction;
  }
  
  Target_Position = Target_Position - Current_Position;

  if(Target_Position<=-1){
    Target_Position = Target_Position*-1;
  }
  
  steps_remaining = Target_Position*11.75f;
}

```

### void Update_Stepper_Position()

I would like the position in degrees to be fed back to the terminal for the user to see.
 So i made this function which would move the cursor to the bottom right of the terminal and
print the position of which the redundant arm has been rotated at the base.

```c

void Update_Stepper_Position(){

  move_cursor(22 ,82);
  Current_Position = Target_Position;
  Serial.print(Current_Position);
  
}

```

### void Update_Servo_Rotation()

Also updates information to be displayed onto the terminal, This is specifically
for the end-effector and will display any position between 0° and 180°. This is not designed
to go outside of that range and display false data. The function where the end-effector is actually
being controlled will also not allow it to go beyond these perameters.

```c

void Update_Servo_Rotation(){

  if(pos>=0||pos<=180){
    text_colour(6);
    move_cursor(17 ,20);
    Serial.print(pos);
    Serial.print("  ");
    move_cursor(17 ,24);
    text_colour(3);
    Serial.print(" degrees");
  }
}

```

### void Operate_EndEffector()

In the main function i use Rapid polling constantly opposed to using interrepts and allowing the mcu to sleep.
This function gets polled every other function check. Reason being is the other functions are alot more time consuming than this one.
The vast majority of the times this function is called it will not get past the first if statement and will exit the function after then.
Also this need to be check more often as i would like the user to be using the operations of the end-effector to feel frictionless, and not to feel
like a long lag between the commands being entered and the commands being executed. If this was to be called at the same ratio as the other functions
in the main function there would be a long delay as there are alot of cursor movement around the terminal. Even though the mcu can now work comfitably
with a baud rate of 115200 the time delay is deffinently noticable. If this function does get through the first condition it is because a character has
been entered into the keyboard. Therefore we need to figure out the action to take dependent of the which character was entered.
entering either an "a" or a "d" into the switch statement will determine that the gripper of the end-effector needs to opened or closed.
Aternativly for entering either a "q" or  an "e" this will rotate the end-effector. For the gripper its binary a in the sense that it is either oper or it
is closed, For the rotation of the end effector it will rotate 20 degrees into the specified direction and is restricted betwen the angles
of 0° and 180°. To remove the data within the serial i had to use a crude method of reseting the serial as many other methods i researched didnt not
work the way i was expecting.

```c

void Operate_EndEffector(){
  
  if(Serial.available()>0) {
    Servo_cmd = Serial.read();
    switch(Servo_cmd){
      
      case('a'):
        pos = 90;
        for (pos = 90; pos >= 0; pos -= 1) {
          myservo1.write(pos);              
          delay(15);                      
        }
        break;
        
      case('d'):
        pos = 0;
        for (pos = 0; pos <= 90; pos += 1) { 
          myservo1.write(pos);              // tell servo to go to position in variable 'pos'
          delay(15);                             
        }
        break;
        
      case('q'):
        if(pos>19);
          y = 0;
          while(y<20) { 
            myservo2.write(pos-y);              // tell servo to go to position in variable 'pos'
            delay(15); 
            y++;                                
          }
          pos -= y;
          Update_Servo_Rotation();
        break;
        
      case('e'):
        if(pos<161);
          y = 0;
          while(y<20) { 
            myservo2.write(pos+y);              // tell servo to go to position in variable 'pos'
            delay(15); 
            y++;                       
          }
          pos += y;
          Update_Servo_Rotation();
        break;
    }
    Serial.end(); 
    Serial.begin(115200); 
  }
}

```

### void Update_Proximity-SensorX()_

The reason for splitting into several functions is for effenciecy while rapid polling. If all of these where to be called
in one function we ould be neglecting other more crucial parts of the program, you will be able to see with in the main loop
exactly what i mean.

Along the redundant arm we will have proximity sensors decting distance so that we
can have canadd obsticle avoidance as another functionality. Here is where we detect a distance
and and turn into a value that is more meaningful for a person to understand before calling a function which will display it to the terminal.


As the speed of sound is relitivley constant we can call velocity 340 meters per second and using the time between transmit and
receive the only unknown is displacment. The time between transmit and receive needs to be divided in half as the sound is traveling double the
distance of what we are trying to find out.

![alt text](https://howtomechatronics.com/wp-content/uploads/2015/07/Ultrasonic-Sensor-Equasions.png "Refrence https://howtomechatronics.com/tutorials/arduino/ultrasonic-sensor-hc-sr04/")

![alt text](https://howtomechatronics.com/wp-content/uploads/2015/07/Ultrasonic-Sensor-Diagram.png "Refrence https://howtomechatronics.com/tutorials/arduino/ultrasonic-sensor-hc-sr04/")


```c

 void Update_Proximity_Sensor1(){

  digitalWrite(trigPin1, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin1, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin1, LOW);
  duration = pulseIn(echoPin1, HIGH);
  distance= duration*0.034/2;
  Print_Proximity_Distance(3);

 }

void Update_Proximity_Sensor2(){
  digitalWrite(trigPin2, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin2, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin2, LOW);
  duration = pulseIn(echoPin2, HIGH);
  distance= duration*0.034/2;
  Print_Proximity_Distance(5);

}

void Update_Proximity_Sensor3(){
  digitalWrite(trigPin3, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin3, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin3, LOW);
  duration = pulseIn(echoPin3, HIGH);
  distance= duration*0.034/2;
  Print_Proximity_Distance(7);

}

void Update_Proximity_Sensor4(){
  digitalWrite(trigPin4, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin4, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin4, LOW);
  duration = pulseIn(echoPin4, HIGH);
  distance= duration*0.034/2;
  Print_Proximity_Distance(9);

}

void Update_Proximity_Sensor5(){
  digitalWrite(trigPin5, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin5, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin5, LOW);
  duration = pulseIn(echoPin5, HIGH);
  distance= duration*0.034/2;
  Print_Proximity_Distance(11);
  
}

```

### void Print_Proximity_Distance(int col)

Called at the end of void Update_Proximity_Sensor5() we call this function. The input argument is so it 
knows what position to move the cursor terminal to. As ultra sonic sensors can detect out to 3 meters and i
feel they become less reliable at this range I set a upper distance limit to 99 cm.

```c

void Print_Proximity_Distance(int col){
  
  if(distance<100){
    text_colour(6);
    move_cursor(col, 45);
    Serial.print(distance);
    Serial.print(" ");
    move_cursor(col, 48);
    Serial.print("cm");
  }
}

```

### void EndEffector_Proximity_Distance()

This was a earlier prototype where it was not split into 2 functions. i decided to kepp it none the less
ass it is worked to the same effect. This sensor is for the end effector only.

```c

void EndEffector_Proximity_Distance(){

  digitalWrite(trigPin6, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin6, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin6, LOW);
  duration = pulseIn(echoPin6, HIGH);
  distance = duration*0.034/2;

  if(distance<100){
    text_colour(6);
    move_cursor(17 ,63);
    Serial.print(distance);
    Serial.print(" ");
    //move_cursor(col, 48);
    //Serial.print("cm");
  }
}

```

### void loop()

The main loop uses rapid polling rather than NVIC sub priorities. Operate_EndEffector()
gets polled everyother function as to reduce delay between entering a serial value and
being executed. If there is no serial the function exits very quickly so the time to call it
this often does not make much diffrence to the speed of updating the sensors. this is why the 
sensors are split rather than nested. The if statement will only trigger if i command comes in
saying that the stepper motor is in the wrong position.

```c

void loop() {

  Operate_EndEffector();
  Update_Proximity_Sensor1();
  Operate_EndEffector();
  Update_Proximity_Sensor2();
  Operate_EndEffector();
  Update_Proximity_Sensor3();
  Operate_EndEffector();
  Update_Proximity_Sensor4();
  Operate_EndEffector();
  Update_Proximity_Sensor5();
  Operate_EndEffector();
  EndEffector_Proximity_Distance();
  
  if(Target_Position!=Current_Position){
    Rotate_Arm();
    Current_Position=Target_Position;
    Update_Stepper_Position();
  }
}

```

### void setup()

I will break this function down into sections.

```c

void setup() {

  pinMode(IN1, OUTPUT); 
  pinMode(IN2, OUTPUT); 
  pinMode(IN3, OUTPUT); 
  pinMode(IN4, OUTPUT); 
  pinMode(trigPin1, OUTPUT); // Sets the trigPin as an Output
  pinMode(echoPin1, INPUT); // Sets the echoPin as an Input
  pinMode(trigPin2, OUTPUT); // Sets the trigPin as an Output
  pinMode(echoPin2, INPUT); // Sets the echoPin as an Input
  pinMode(trigPin3, OUTPUT); // Sets the trigPin as an Output
  pinMode(echoPin3, INPUT); // Sets the echoPin as an Input
  pinMode(trigPin4, OUTPUT); // Sets the trigPin as an Output
  pinMode(echoPin4, INPUT); // Sets the echoPin as an Input
  pinMode(trigPin5, OUTPUT); // Sets the trigPin as an Output
  pinMode(echoPin5, INPUT); // Sets the echoPin as an Input
  pinMode(trigPin6, OUTPUT); // Sets the trigPin as an Output
  pinMode(echoPin6, INPUT); // Sets the echoPin as an Input
  
  myservo1.attach(6);  // attaches the servo on pin 9 to the servo object
  myservo2.attach(9);  // attaches the servo on pin 9 to the servo object
  
  while (!Serial){;} //wait for serial port to connect
  Serial.begin(115200);
  Serial.print("\eS");  // tell to use 7-bit control codes
  Serial.print("\e[?25l"); // hide cursor
  Serial.print("\e[?12l"); // disable cursor highlighting
  
  Set_Terminal();
  Print_Proximity();
  Print_Load();
}

```

Defining the 4 pins for the stepper motor driver

![alt text](https://arduino-info.wikispaces.com/file/view/SmallStepperBoard2.jpg/229496680/400x300/SmallStepperBoard2.jpg)
![alt text](http://arduino-info.wikispaces.com/file/view/Stepper-400px-Schematic.jpg/609544227/Stepper-400px-Schematic.jpg)
![alt text](https://arduino-info.wikispaces.com/file/view/SmallStepperDiagram.jpg/342345564/333x251/SmallStepperDiagram.jpg)

[Reference](https://arduino-info.wikispaces.com/SmallSteppers)

Setting all pins as outputs when activated in combination these pins will set the phases
of the motor.

```c

  pinMode(IN1, OUTPUT); 
  pinMode(IN2, OUTPUT); 
  pinMode(IN3, OUTPUT); 
  pinMode(IN4, OUTPUT);

```


Inputs and outputs fot each ultra sonic sensor.


```c

  pinMode(trigPin1, OUTPUT); // Sets the trigPin as an Output
  pinMode(echoPin1, INPUT); // Sets the echoPin as an Input
  pinMode(trigPin2, OUTPUT); // Sets the trigPin as an Output
  pinMode(echoPin2, INPUT); // Sets the echoPin as an Input
  pinMode(trigPin3, OUTPUT); // Sets the trigPin as an Output
  pinMode(echoPin3, INPUT); // Sets the echoPin as an Input
  pinMode(trigPin4, OUTPUT); // Sets the trigPin as an Output
  pinMode(echoPin4, INPUT); // Sets the echoPin as an Input
  pinMode(trigPin5, OUTPUT); // Sets the trigPin as an Output
  pinMode(echoPin5, INPUT); // Sets the echoPin as an Input
  pinMode(trigPin6, OUTPUT); // Sets the trigPin as an Output
  pinMode(echoPin6, INPUT); // Sets the echoPin as an Input

```

Defining which pins to use for operation of the endeffector servos.

```c

  myservo1.attach(6);  // attaches the servo on pin 9 to the servo object
  myservo2.attach(9);  // attaches the servo on pin 9 to the servo object

```

The while loop will spin infinatly until the putty terminal is dectected by the serial so
that the arduino doesnt start executing the putty setup before the terminal is opened.

Setting the baud rate.

vt100 escape sequences use ascii codes, remove terminal cursor and removes  cursor highlighting.

```c

  while (!Serial){;} //wait for serial port to connect
  Serial.begin(115200);
  Serial.print("\eS");  // tell to use 7-bit control codes
  Serial.print("\e[?25l"); // hide cursor
  Serial.print("\e[?12l"); // disable cursor highlighting

```

Already explained above but are only ever to be used once so they could within the setup

```c

  Set_Terminal();
  Print_Proximity();
  Print_Load();

```

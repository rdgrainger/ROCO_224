
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







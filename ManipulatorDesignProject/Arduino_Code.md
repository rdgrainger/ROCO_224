
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









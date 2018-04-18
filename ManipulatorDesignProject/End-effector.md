# End-effector

### Description

The end-effector for the redundant arm is going to be a pincer style gripper which
is to be operated by two servos. One servo will close the gripper whilst the other
will control the rotation of the end effector. The user will be able to control the end
effector from the serial terminal (putty) and information will be fed back on the angle
and the state of the gripper.

### Printing

This End-effector is to be 3D printed from PLA for the following reasons.

* PLA is NOT harmful to the environment.

* This end-effector does not need the added strength or higher melting point advantages that ABS offers.

* PLA is more aesthetically pleasing.

The only advantage we can see for having a ABS print is that the specific gravity is lower than PLA.

![alt text](https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/30708623_547522195662742_6546727641184689882_n.jpg?_nc_cat=0&oh=23712a9503886b7722c71799319327bf&oe=5B6AFEB7 "reference google")

### Control

As mentioned above the end-effector will be controlled with the serial input to putty.
We will be displaying a user interface using a arduino mega which will take data from
the keyboard in order to control the end-effector. 

We shall be using a ultra sonic sensor so that we can see the distance of a object from the
gripper. The sensor has been limit to 99 cm as distances beyond that get less accurate. One idea
was to use the end effector to scan a area from left to right move down one space and keep on repeating to build
I kind a picture, made of diffrent colour characters each colour determining the range of what it detected.
Essentially capturing an image and sending it to a window in the terminal, although this would look ugly
(which it did) each pixel would have a saved distance and by a serial input you could let the end-effector know
which pixel you are targeting and the arduino will know how to get their and be able to grab it all with one
click of a button. I did manage to successfully make this work but it took so long to capture a image it was bottle necking
processing speed for keeping other data current and decided it was far to taxing to keep it included within the sketch.

This is the End-effector portion of of the terminal is shown below and is kept towards the bottom left of the terminal
As seen the object distance relates to the ultra sonic sensor. Rotation @ x degrees is changable using the Q and E key from
the keyboard and the gripper and be opened and closed by using the A and D keys.

![alt text](https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/30706812_547542192327409_3959721976600957218_n.jpg?_nc_cat=0&oh=78a7f4ded3b07da744d80771ed834f22&oe=5B567E7C
 "Putty Terminal")

### Design

![alt text](https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/30226284_543637989384496_7946924284049508048_n.jpg?_nc_cat=0&oh=3ba45e0a495fbf95b6ef5cb8e7c13418&oe=5B73D877 "reference google")

![alt text](https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/30262089_543637886051173_7528792012232457465_n.jpg?_nc_cat=0&oh=737bdb1384b5d95c2cf6eb916e3c5a34&oe=5B6648B7 "reference google")

![alt text](https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/30264779_543637882717840_2496579833197552654_n.jpg?_nc_cat=0&oh=9ae41402f331d8764b39ef9b44f23d9c&oe=5B5289E4 "reference google")

![alt text](https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/30222305_543637889384506_731331599402551053_n.jpg?_nc_cat=0&oh=cfde7b64f81bce25bc485f984b735f80&oe=5B560CC9 "reference google")

![alt text](https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/30226854_543637829384512_949741924307240401_n.jpg?_nc_cat=0&oh=26f52543b65f7a27693bd73c6ebc56c5&oe=5B63C0CD "reference google")

![alt text](https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/30261695_543637942717834_7309470113158697161_n.jpg?_nc_cat=0&oh=7327cf68b40d3358da9aa6366147af19&oe=5B7036D5 "reference google")

![alt text](https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/30414781_543637832717845_1012479154870773978_n.jpg?_nc_cat=0&oh=082061b54b9a75b909990bfb8bf5adda&oe=5B7152B6 "reference google")

![alt text](https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/30440672_543637992717829_1365184750596316462_n.jpg?_nc_cat=0&oh=674d18b0d06cf88264091213ba6bfa8c&oe=5B52A0B0 "reference google")

![alt text](https://scontent-lhr3-1.xx.fbcdn.net/v/t31.0-8/30171511_543637996051162_8558167694738994149_o.jpg?_nc_cat=0&oh=c36f38785209245d7f4c5d26aed35ccc&oe=5B564F6B "reference google")

![alt text](https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/30261289_543637939384501_4974151850118603510_n.jpg?_nc_cat=0&oh=08da735408bc6eb58cc499ce70965840&oe=5B67DC4B "reference google")

![alt text](https://scontent-lhr3-1.xx.fbcdn.net/v/t1.0-9/30222345_543637826051179_6098994668176385199_n.jpg?_nc_cat=0&oh=943b0d8a780c75a4a47dbdebc597f9a6&oe=5B63FCF7 "reference google")

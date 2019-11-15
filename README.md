COSI 119A Final Project Arm Interfacing
===============

Jacob Smith, Fall 2019, Brandeis University

My project is to interface an arm to the mutant campus rover.

See the project Proposal in extras folder

using [VarSpeedServo](https://github.com/netlabtoolkit/VarSpeedServo) Library as background code, all my work is in examples and extras folder

Functionality so far

### Oct 25-31:

#### Getting Arm to Move

(provided)  Sweep Controls Servo on Port 7

(provided) *RobotArmScript* Arduino Program to control rest of the Arm

#### Getting Arduino to talk to Rasberry Pi

*(provided)  ArmArduinoV1*, lets the Arduino talk to the Pi

*(provided)  PiArduinoV1* lets teh Pi talk to the Arduino

​					Screenshot showing Message from Arduino being printed on Rasberry Pi

![V1 Where Arduino can talk to Pi](docs/V1.png)

#### **Getting Rasberry Pi to Talk to Arduino**

(provided)  ArmArduinoV2*, lets the Arduino talk to the Pi

*(provided)  PiArduinoV2* lets the Pi talk to the Arduino

​	It sends a serial command to the Arduino, which reads it and moves the servo

​	![PiAQrduinoV2 Script](docs/V2Script.png)

#### **Workflow improvements**			

​		Now that the basic premise was set up, I remounted the Servo Arm onto a robot chassis. Also, I can log into the mutant robot and execute python scripts over ssh. 

​				`ls /dev/tty*`		Lists available USb ports, look for USB 0

​				`ssh mutant@mutant.dyn.brandeis.edu 'python' < '/home/robot/catkin_ws/src/arminterface/Raspi/ArmPiV4.py'`      Executes a python file on ssh without copying that file to the mutant rasberry Pi, 40 second time lag

​		These workflow improvements should allow me to test the arm much more quickly than plugging in to the mutant and and plugging in the arm etc.

​			![Condensed Arm](docs/ArmCondensed.jpg)

​		Also, I am running the commands on the mutant robot, where the arm now has a port.

#### **Two Way Communication**

​		*ArmArduinoV3*, Opens the gripper if receives o character over serial, closes if c character

​		*PiArduinoV3* Sends the open and close characters, also two way communication

​		This is the simplest expression of the interface required for the final project.

​			First, we check that the arm shows up in hte list of USB ports as USB 0

​					![USB Ports Demonstration](docs/USBV3.gif)

​		Then, we run the ArmPiV3 script over ssh, which moves the arm and displays output

​					![Output of V3](docs/OutputV3.gif)

​			Finally, the servo opens and closes!

![Servo MovingV3](docs/ServoMovingV3.gif)

​					Here is a video of this stage:

​						 [![Shows Python file controlling gripper on  arm](http://img.youtube.com/vi/m0WW0uCtgWg/0.jpg)](http://www.youtube.com/watch?v=m0WW0uCtgWg "Two Way Arm Interfacing Video")



#### Future Work

​			The main future goal is to interface the arm to ROS. In the short term, I want to decrease the lag, enable the arm to respond to keyboard input, and get the arm fixed

### Nov 1-7:

This week, I focused on the hardware of the arm, taking it apart, putting it back together, and mounting it to the Mutant Robot. Version 4 of the Arduino and Pi scripts show updated functionality, where the whole arm can be controlled.

​	First, I replaced the broken motors on the arm, this shows the base motor rotating after I lined it up again 	absolute position matters for servos):

![The Gears of the base of the arm](docs/BaseMechanics.gif)

​	Then, I mounted the arm on the mutant, note how there is enough clearance for the lidar because of the 	washers I added:

![New Waffle level showing clearance for arm](docs/LidarMount.gif)

​	Version 4 of my scripts allow mulktiple parts of the arm to be controlled, shown here is the console output and robot motion.

![The Console inder Version 4](docs/V4Console.png)

![Arm Mounted on Robot](docs/ArmMount.gif)

A video of this week's work can be found [here](https://www.youtube.com/watch?v=U1VSVlQWPBY) 

**Future Work:**

By Next week, I will have communicated with Charlie about the battery and broken motors, and plugged the arm into a publisher/subscriber model in ROS. The deliverable will be a video of me moving the arm as I did this week but with ROS scripts.

### Nov 8-15

Update: Charlie remounted the arm, I wrote the bringup and roscore script and it works over ssh, the arm moves using rostopic publisher and subscriber, subscriber has nice error handling like if usb times out.

Sssh Script problem: It seems that running roscore in ssh command doesn't work even though it works in an interactie session. i tired allowing mutant to allow user environment variables, but the problem persists. Even though i haven't figured anything out, i am learning a lot about linux and how to use vim. I also tried specifying the exact directory location of roscore file, but I go this error https://www.howtoforge.com/vim-basics

Here is a gif of the bash script working, no more opening multiple terminals to run roscore, bringup, and the nodes!

![Terminal while excecuting user friendly bash script to move arm](docs/ROSandBash.gif)

Here is the arm top mounting that Charlie did, now the arm doesn't get in the way of the lidar's field of vision.

![Remounted Arm](docs/TopMountingArm.gif)

Future work: Upload python file remotely, implement ROS actions, implement sensor control.



**Sources**	

SSH Commands manual https://www.freebsd.org/cgi/man.cgi?sshd_config(5)

Edit read only file in vimhttps://www.geekyboy.com/archives/629

Hoe to use vim https://www.howtoforge.com/vim-basics

Links http://gazebosim.org/tutorials/?tut=ros_urdf http://wiki.ros.org/joint_trajectory_controller Pito doesn;t like https://answers.ros.org/question/38694/running-ros-across-multiple-machines/

Non interactive shell information https://www.howtoforge.com/vim-basics https://www.howtoforge.com/vim-basicsS

Permit user enviornmet https://stackoverflow.com/questions/10562722/command-not-found-via-ssh-with-single-command-found-after-connecting-to-termina

Remote run ROS https://askubuntu.com/questions/1156507/cannot-perform-command-after-remote-logging-in-via-ssh

Run remote rs node https://github.com/pandora-auth-ros-pkg/pandora_docs/wiki/Remote-Machines-Running-ROS-nodes

How turtlbebot handles erial connections https://github.com/ros-drivers/rosserial/blob/melodic-devel/rosserial_python/nodes/serial_node.py

Error with running ros I got https://answers.ros.org/question/310848/run_id-on-parameter-server-does-not-match-declared-run_id/

Trying to get positon from servos, Charly convinced me there is no way to get position because there is no position wire.

http://forums.trossenrobotics.com/tutorials/how-to-diy-128/get-position-feedback-from-a-standard-hobby-servo-3279/

Arm is Moving function eixst in varspeedservo

http://forums.trossenrobotics.com/tutorials/how-to-diy-128/get-position-feedback-from-a-standard-hobby-servo-3279/

Error with running node remotely which I had https://answers.ros.org/question/243478/getting-rosgraph-resource-not-found-when-trying-to-get-turtlebot-local_plan/

Rosremote (didn't try) http://wiki.ros.org/ROSRemote

Ros remote machine (didn't try) https://github.com/pandora-auth-ros-pkg/pandora_docs/wiki/Remote-Machines-Running-ROS-nodes

Exception message python https://stackoverflow.com/questions/9823936/python-how-do-i-know-what-type-of-exception-occurred

Add python file to path https://askubuntu.com/questions/470982/how-to-add-a-python-module-to-syspath

ssh python finding module https://stackoverflow.com/questions/6851184/python-cannot-find-module-when-using-ssh		

[1] Links fix Arduino download error https://www.arduino.cc/en/Guide/Linux

[2] Get Arduino to listen to Serial https://www.instructables.com/id/Connect-Your-Raspberry-Pi-and-Arduino-Uno/

[3]Get Pi to talk to Serial https://pyserial.readthedocs.io/en/latest/shortintro.html

[4] Run a Python File remotely https://www.shellhacks.com/ssh-execute-remote-command-script-linux/


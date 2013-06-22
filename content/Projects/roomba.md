Title: ROS on a Roomba
Date: 2013/6/5
Category: Projects
Tags:  robot, ROS, SLAM, computer vision 
Author: Jason
Summary: 
![](images/roomba/title.png)

I came across an unused iRobot Roomba at <a href="http://www.fubarlabs.org">FUBAR labs</a> and though this would be the perfect opportunity to build a robot using the Robotic Operating System <a href="http://www.wikipedia.org/wiki/ROS_%28Robot_Operating_System%29">(ROS)</a>.  ROS is basically software that's used integrate all a robot's sensors (encoders, depth camera, laser scanner ect..) with the code that's used to control it. 

I came across an unused iRobot Roomba at <a href="http://www.fubarlabs.org">FUBAR labs</a> and though this would be 
the perfect opportunity to build a robot using the Robotic Operating System <a href="http://www.wikipedia.org/wiki/ROS_%28Robot_Operating_System%29">(ROS)</a>.  ROS is basically software that's used integrate all a robot's sensors (encoders, depth camera, laser scanner ect..) with the code that's used to control it.  All of the sensors run as a node.  For example, the Kinect
sensor node publishes its depth data.  The SLAM (localization and mapping) node will uses the Kinect data to determine the 
robot's location and publish to other nodes.  Each node is separate from the others so it's easy to change or add new ones.
ROS also has a ton of built in <a href="http://www.ros.org/wiki/Sensors">libraries</a> that work with different servos, laser scanners, and other sensors.  

The Roomba is essentially the same thing as the iRobot Create, which is the base used for the 
<a href="http://www.willowgarage.com/turtlebot">Turtlebot</a> designed by Willow Garage (another ROS robot).  In the picture below, I've got the Roomba on the bottom, the laptop that will be running ROS, and a Kinect sensor on top.  Not pictured is the workstation, which is just another computer running ROS and used to render the localization data. 


![roomba](images/roomba/roomba.jpg)


After a bit of searching, I found 
out that someone had already created a ROS stack for the Roomba.  Unfortunately, I ran into a lot of trouble getting it to work
and later found out that the Roomba stack is a bit buggy.  The good thing about the similarity between the Turtlebot and Roomba is
that you can use the Turtlebot stack with just a few small changes.  From this post on <a href="http://answers.ros.org/question/47094/turtlebot-roomba-dashboard/?answer=47096#post-id-47096">ROS answers</a> I learned that a few changes to the Turtlebot launch file were
needed.  The code below is what I've got working on my Roomba right now.

<div class="row">
<div class="span1">
</div>
<div class="span10">
<script src="http://pastebin.com/embed_js.php?i=eLqFvK6d"></script>
</div>
</div>

<h2>Teleoperation and Mapping</h2>

Here are some examples of the Roomba working with the teleoperation and SLAM packages at <a href="http://www.fubarlabs.org">FUBAR labs</a>.  In the first video, I'm driving the Roomba
around with the keyboard on the workstation using the teleop package.  The Roomba is running the SLAM package, mapping, and sending its data back to the workstation for visualization.  The
white circle on the map is the Roomba and white lines moving around in front of the robot are where it thinks it's seeing an object based on the Kinect data.
There was a bit of trouble with the communication between
the two that caused the robot's localization to jump around.  I think this was the result of my workstation trying to run ROS, 
display the visualization data, and record a screen video.  This slowed the computer down almost to the point where it was unusable. I did manage to get some video though. 
<br>
<br>
        <div class="row">
        <div class="span1">
        </div>
        <div class="span9">
<iframe width="560" height="315" src="http://www.youtube.com/embed/XTUC-DoW4uQ" frameborder="0" allowfullscreen></iframe>
        </a>
        </div>
        </div>
<br>
<br> 
In the video below, I thought it would be nice to show the depth image from the Kinect along with the map.  This caused
the workstation to lag quite a bit as you can see in the video.  Still it shows some nice images of the Kinect. 
<br>
<br>
        <div class="row">
        <div class="span1">
        </div>
        <div class="span9">
<iframe width="560" height="315" src="http://www.youtube.com/embed/ynAjzHpw1XU" frameborder="0" allowfullscreen></iframe>
        </a>
        </div>
        </div>
<br>
<br>
I made a better map later on of my apartment.  No video for that one but here is an image of the map.  It's still a little
messy, but I was able to use this map to have the Roomba autonomously navigate around my apartment.
This was using the Adaptive Monte Carlo Localization <a href="http://www.fubarlabs.org">(AMCL)</a>
package which uses a particle filter to localize the robot.  
<div class="row">
<div class="span1">
</div>
<div class="span9">
![slam](images/roomba/SLAMmap.png)
</div>
</div>





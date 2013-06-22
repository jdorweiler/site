Title: Autonomous Car pt.1
Date: 2012/06/05
Category: Projects
Tags: arduino, robot, C++, autonomous car, computer vision 
Author: Jason
Summary: 

![](images/car/title_1.png) 

This post will detail my plans and progress for an autonomous car. I’ll be using what I learned from building the <a href="electronics.html#robot">balancing robot</a> and courses though <a href="http://www.udacity.com" target="_blank">Udacity</a> and <a href="http://mitx.mit.edu/" target="_blank">MITx</a> The goal is to take a small 1/10 remote controlled car and have it navigate a course on its own.  
  
This post will detail my plans and progress for an autonomous car. I’ll be using what I learned from
building the <a href="electronics.html#robot">balancing robot</a> and courses though <a href="http://www.udacity.com" target="_blank">Udacity</a> and <a href="http://mitx.mit.edu/" target="_blank">MITx</a>.
The goal is to take a small 1/10
remote controlled car and have it navigate a course on its own. To navigate the course, I plan to use an
Android phone, GPS, and an accelerometer/gyroscope. I will record a video stream through the phone
and then process it on my laptop using Python and the computer vision library <a href="http://opencv.willowgarage.com/wiki/" target="_blank">OpenCV</a>.  
<br>
<br>
<strong> The Car </strong>
<br>
I chose a 1/10 scale car so that it would be large enough to hold all of the electronics. I was worried that
the cheaper and more common 1/16 scale cars would be too small. I found this car on Amazon for about
$100. Here are some pictures of it below. I've already started working on the electronics.  
<br>
<br>
<div class="row">
<div class="span1">
</div>
<div class="span5">
![car1](images/car/car1.png)
</div>

 <div class="span5">

![car2](images/car/car2.png)

</div>
</div>


<strong>The Code</strong>

Below is a block diagram I drew that shows my preliminary design for the software portion of the car. 
<div class="row">
<div class="span1">
</div>
<div class="span8">
![code](images/car/code.png)
</div>
</div>
For the localization of the car I’ll be using a GPS sensor and accelerometer. First, the GPS will find the
starting coordinates of the car. Then the car will start moving, somewhat blindly at first. Once the car is
moving, data from the accelerometer and elapsed time can be used to calculate the velocity of the car.
These two measurements are then passed into a Kalman filter to improve the accuracy of the car’s
position.
<br>
<br>
At the same time, heading data from the GPS and gyroscope are passed into another Kalman filter to get
an accurate heading on the car. The position and heading data from the Kalman filters is then compared
to a GPS waypoint given to the sensor and motor control. Next, a heading toward the desired GPS
waypoint is calculated, and the appropriate signal is sent to the steering servo in order to point the car
in that direction.
<br>
<br>
While this process is occurring, the camera from the Android phone will stream images of the car’s path
back to my computer. On the computer, I will use Python and OpenCV to process the images. The
images will be processed looking for the edges of the road (width) and also looking for obstacles in from
of the car. Data from the OpenCV program will be sent back to the motor control, allowing the car to
deviate from its previously calculated heading and avoid objects that may get in the way.<br>
<br>
Below is the preliminary work I’ve done on the image processing. I’m using an app called Barnacle to set
up a local network between my phone and laptop and then sending the camera feed over HTTP. Then
I’m using Python to grab a snapshot of the camera feed. I’ll walk through the steps of the image
processing below. I’ll save the discussion of the theory and mathematics behind these processing
algorithms for another time and just stick with the results for now.
<br>
<br>
Here’s the original image on the left.  The first step is to convert it to grayscale, shown on the right. 
<br>
<br>
<div class="row">
<div class="span1">
</div>
 <div class="span5">
![road](images/car/road.png)
</div>

 <div class="span5">
![road2](images/car/road2.png)
</div>
</div>
<br>
<br>
From the gray image I run a <a href="http://en.wikipedia.org/wiki/Gaussian_blur" target="_blank">Gaussian filter</a> to reduce the noise and avoid finding false lines later on in the processing  (not shown).  Then the result of the Gaussian filter is passed into a <a href="http://en.wikipedia.org/wiki/Canny_edge_detector" target="_blank">Canny edge detector</a> to locate the edges.  
<br>
<br>
<div class="row">
<div class="span2">
 </div>
 <div class="span5">
![road3](images/car/road3.png)
</div>
</div>
<br>
<br>
From the Canny image, you can already see the outline of the road. But if you look closely at the Canny
image, you’ll notice that those aren’t quite continuous lines. There are spots where the line breaks,
which will make doing any useful mathematics on it later more difficult. To fix this issue and clean up the
image further, I’m applying a
<a href="http://en.wikipedia.org/wiki/Hough_transform" target="_blank">Hough transform</a> 
to the Canny output. This transform is fairly complicated,
but the quick summary is that it looks for sections in the image that aren’t continuous but that form part
of a larger line. For this part, I’m also giving it a theta angle to restrict which lines it returns.
<br>
<br>
<div class="row">
<div class="span2">
 </div>
 <div class="span5">
![road4](images/car/road4.png)
</div>
</div>
<br>
<br>
That’s it for now.  From here I can make sure that the car stays in the middle of the road. 
<br>
 Next plans are to add <a href="http://en.wikipedia.org/wiki/Blob_detection" target="_blank">obstacle detection</a>. 
 <br>
 <br>
 <br>
 <br>
 

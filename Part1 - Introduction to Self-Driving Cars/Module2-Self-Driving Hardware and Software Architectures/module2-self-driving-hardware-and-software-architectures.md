# Module2：Self-Driving Hardware and Software Architectures

> System architectures for self-driving vehicles are extremely diverse, as no standardized solution has yet emerged. This module describes both the hardware and software architectures commonly used and some of the tradeoffs in terms of cost, reliability, performance and complexity that constrain autonomous vehicle design.

### 学习目标

* Design an omni-directional multi-sensor system for an autonomous vehicle
* Describe the basic architecture of a typical self-driving software system
* Break down the mapping requirements for self-driving cars based on their intended uses.

## Lesson 1: Sensors and Computing Hardware

Welcome to the first lesson of week two. This week we will discuss many details about the hardware and software architecture used in today's self-driving cars. In particular, we will cover the various sensors that can be used for perception. The computing platforms available today. The basics of designing sensor configurations for self-driving cars. The components of a typical autonomy software stack. And finally, we will conclude by discussing the various ways which we can represent the environment for self-driving. Hopefully by the end of this week you will have a broad understanding of the equipment and software ideas that go into making a self-driving car.

Let's begin this video. In this video, we will define sensors, and discuss the various types of sensors available for the task of perception. And we will then discuss the self-driving car hardware available nowadays. Let's begin by talking about sensors.

### 1. Sensors

> Categorization:
>
> * exteroceptive:extero = surroundings
> * proprioceptive: proprio = internal

Even the best perception algorithms are limited by the quality of their sensor data. And careful selection of sensors can go a long way to simplifying the self-driving perception task. So what is a sensor? **A sensor is any device that measures or detects some property of the environment, or changes to that property over time.** Sensors are broadly categorized into two types, depending on what property they record. If they record a property of the environment they are exteroceptive. Extero means outside, or from the surroundings. On the other hand, if the sensors record a property of the ego vehicle, they are proprioceptive. Proprios means internal, or one's own. Let's start by discussing common exteroceptive sensors.

### 2. Sensors for perception : Camera

We start with the most common and widely used sensor in autonomous driving, the camera. Cameras are a passive, light-collecting sensor that are great at capturing rich, detailed information about a scene. In fact, some groups believe that the camera is the only sensor truly required for self-driving. But state of the art performance is not yet possible with vision alone. While talking about cameras, we usually tend to talk about three important comparison metrics. We select cameras in terms of their resolution. The resolution is the number of pixels that create the image. So it's a way of specifying the quality of the image. We can also select cameras on the basis of field of view. The field of view is defined by the horizontal and vertical angular extent that is visible to the camera, and can be varied through lens selection and zoom.

Another important metric is the dynamic range. The dynamic range of the camera is the difference between the darkest and the lightest tones in an image. High dynamic range is critical for self-driving vehicles due to the highly variable lighting conditions encountered while driving especially at night. There is an important trade off cameras and lens selection, that lies between the choice of field of view and resolution. Wider field of view permit a lager viewing region in the environment. But fewer pixels that absorb light from one particular object. As the field of view increases, we need to increase resolution to still be able to perceive with the same quality, the various kinds of information we may encounter. Other properties of cameras that affect perception exist as well, such as focal length, depth of field and frame rate. We'll go into much more detail on cameras and computer vision in course three on visual perception.

![1555052504792](../../.gitbook/assets/1555052504792.png)

The combination of two cameras with overlapping fields of view and aligned image planes is called the stereo camera. **Stereo cameras allow depth estimation from synchronized image pairs.** Pixel values from image can be matched to the other image producing a disparity map of the scene. This disparity can then be used to estimate depth at each pixel. Next we have LIDAR which stands for light detection and ranging sensor.

### 3. Sensors for perception : LIDAR

> Comparison metrics :
>
> * Number of beams
> * Points per second
> * Rotation rate
> * Field of view

LIDAR sensing involves shooting light beams into the environment and measuring the reflected return. By measuring the amount of returned light and time of flight of the beam. Both in intensity in range to the reflecting object can be estimated. LIDAR usually include a spinning element with multiple stacked light sources. And output a three dimensional point cloud map, which is great for assessing scene geometry. Because it is an active sensor with it's own light sources, LIDAR are not effected by the environments lighting. So LIDAR do not face the same challenges as cameras when operating in poor or variable lighting conditions.

![1555052660677](../../.gitbook/assets/1555052660677.png)

Let's discuss the important comparison metrics for selecting LIDAR. The first is the number of sources it contains with 8, 16, 32, and 64 being common sizes. And the second is the points per second it can collect. The faster the point collection, the more detailed the 3D point cloud can be. Another characteristic is the rotation rate. The higher this rate, the faster the 3D point clouds are updated. Detection range is also important, and is dictated by the power output of the light source. And finally, we have the field of view, which once again, is the angular extent visible to the LIDAR sensor. Finally, we should also mention the new LIDAR types that are currently emerging. High-resolution, solid-state LIDAR. Without a rotational component of the typical LIDARs, these sensors stand to become extremely low-cost and reliable. Thanks to being implemented entirely in silicon. HD solid-state LIDAR are still a work in progress. But definitely something exciting for the future of affordable self-driving. Our next sensor is RADAR, which stands for radio detection and ranging.

### 4. Sensors for perception : RADAR

> Comparison metrics
>
> * range
> * field of view
> * position and speed accuracy

RADAR sensors have been around longer than LIDAR and robustly detect large objects in the environment. They are particularly useful in adverse weather as they are mostly unaffected by precipitation. Let's discuss some of the comparison metrics for selecting RADAR. RADAR are selected based on detection range, field of view, and the position and speed measurement accuracy.

![1555052858494](../../.gitbook/assets/1555052858494.png)

RADAR are also typically available as either having a wide angular field of view but short range. Or having a narrow filed of view but a longer range. The next sensor we are going to discuss are ultrasonics or sonars. Originally so named for sound navigation and ranging. Which measure range using sound waves.

### 5. Sensors for perception : Ultrasoncis

Sonar are sensors that are short range in inexpensive ranging devices. This makes them good for parking scenarios, where the ego-vehicle needs to make movements very close to other cars.

![1555053021439](../../.gitbook/assets/1555053021439.png)Another great thing about sonar is that they are low-cost. Moreover, just like RADAR and LIDAR, they are unaffected by lighting and precipitation conditions. Sonar is selected based on a few key metrics. The maximum range they can measure, the detection field of view, and their cost.

### 6. Sensors for perception : GNSS/IMU

> Direct measure of ego vehicle states
>
> * position,velocity\(GNSS\)
>   * Varying accuracies : RTK,PPP,DGPS
> * angular rotation rate\(IMU\)
> * acceleration\(IMU\)
> * heading\(IMU,GPS\)

Now let's discuss the proprioceptive sensors, the sensors that sense ego properties. The most common ones here are Global Navigation Satellite Systems, GNSS for short, such as GPS or Galileo. An inertial measurement units or IMU's. GNSS receivers are used to measure ego vehicle position, velocity, and sometimes heading. The accuracy depends a lot on the actual positioning methods and the corrections used. Apart from these, the IMU also measures the angular rotation rate, accelerations of the ego vehicle, and the combined measurements can be used to estimate the 3D orientation of the vehicle. Where heading is the most important for vehicle control. And finally we have wheel odometry sensors. This sensor tracks the wheel rates of rotation, and uses these to estimate the speed and heading rate of change of the ego car. This is the same sensor that tracks the mileage on your vehicle.

So, to summarize, the major sensors used nowadays for autonomous driving perception include cameras, RADAR, LIDAR, sonar, GNSS, IMUs, and wheel odometry modules. These sensors have many characteristics that can vary wildly, including resolution, detection range, and field-of-view. Selecting an appropriate sensor configuration for a self-driving car is not trivial. Here's a simple graphic that shows each of the sensors and where they usually go on a car.

![1555053366229](../../.gitbook/assets/1555053366229.png)

We will revisit this chart again in the next video, when we discuss how to select sensor configurations to achieve a particular operational design domain.

### 7. Computing Hardware

Finally, let's discuss a little bit about the computing hardware most commonly used in today's self-driving cars. The most crucial part is the computing brain, the main decision making unit of the car. It takes in all sensor data and outputs the commands needed to drive the vehicle. Most companies prefer to design their own computing systems that match the specific requirements of their sensors and algorithms. Some hardware options exist, however, that can handle self-driving computing loads out of the box. The most common examples would be Nvidia's Drive PX and Intel & Mobileye's EyeQ.

![1555053472138](../../.gitbook/assets/1555053472138.png)

Any computing brain for self-driving needs both serial and parallel compute modules. Particularly for image and LIDAR processing to do segmentation, object detection, and mapping. For these we employ GPUs, FPGAs and custom ASICs, which are specialized hardware to do a specific type of computation. For example; the drive PX units include multiple GPUs. And the EyeQs have FPGAs both to accelerate parallalizable compute tasks, such as image processing or neural network inference. And finally, a quick comment about synchronization. Because we want to make driving decisions based on a coherent picture of the road scene. It is essential to correctly synchronize the different modules in the system, and serve a common clock. Fortunately, GPS relies on extremely accurate timing to function, and as such can act as an appropriate reference clock when available. Regardless, sensor measurements must be timestamped with consistent times for sensor fusion to function correctly.

Let's summarize. In this video, we learned about sensors and their different types based on what they measure. We covered the major sensors used in self-driving hardware systems, and discussed the advantages and comparison metrics. And then we briefly discussed the self-driving computing hardware available today. Hopefully, this solidifies some of the concepts we learned last week for doing autonomous perception. In the next lesson, we'll take a step further and look at how to select an appropriate sensor configuration for your self-driving car. See you in the next video.


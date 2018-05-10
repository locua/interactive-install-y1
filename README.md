# Title : 

Code Experiment with Colours, Shape and Sound

# Synopsis : 

Interactive audio-visual program. Facial expression and movement tracking create images and sound.

# Build Description and tech details: 

The work consists of a computer and desktop monitor opposite a cushion (seating area). It is really for one user at a time if it is to be used effectively, though more than one could use it for example; the face of one person is tracked in the background while another person’s body makes movement for motion input. The software running consists of one [processing](https://processing.org/download/) sketch, the [FaceOsc](https://github.com/kylemcdonald/ofxFaceTracker/releases) (pre-coded/compiled/built windows executable) , and [Ableton Live 10](https://www.ableton.com/en/live/) (30 day trial). These are connected through [OSC (open sound control)](http://opensoundcontrol.org/) messages. I ran all the software on one pc running windows 10 (in order to make use of the [OpenKinect](https://github.com/shiffman/OpenKinect-for-Processing) processing library). I had two inputs into computer; one webcam and one kinect v1. The camera feed from the webcam was the input for FaceOSC.exe and the kinect fed into the processing sketch.  In processing I used the [oscP5](http://www.sojamo.de/libraries/oscP5/) library which allowed me to create, receive, send osc messages. 

![Image of setup](https://github.com/C1harlieL/CPY1-code-experiment-with-colour--and-sound/blob/master/meg-sis.jpg)

I will first focus on OSC to demonstrate my understanding of OSC and the oscP5 library which connects the various applications into one communicating system. I initialised OSC communications with the two lines of processing code:

```java
oscP5 = new OscP5(this, 8338);
myRemoteLocation = new NetAddress("127.0.0.1", 8001);
```
In the first line an instance of the oscP5 object is initialised to the oscP5 variable. This starts oscP5, listening for incoming messages at port 8338. In the second line a NetAddress is created which is used as a parameter in oscP5.send(); when sending osc packets. An ‘osc plug service’ is used which automatically forwards a specific method to an object. In my program plugs were used for each of the various pre-programmed OSC data packets received from the running executable FaceOSC.exe :

```java
oscP5.plug(this, "mouthWidthReceived", "/gesture/mouth/width");
```
Which was then automatically sent to the method of an object:
```java
public void mouthHeightReceived(float h) {
  //println("mouth height: " + h);
  mouthHeight = h;
}
```
The variable `mouthHeight` was then used to the vary the size of an `ellipse();` which was being drawn to a graphics object:

```java
PGraphics db; // global variable for PGraphics object

…

db = createGraphics(400, 200); // createGraphics function initialised to variable db inside void setup() {}

…

// inside draw

db.ellipse(v1.x, v1.y, mouthHeight*200, mouthHeigh*200);
```


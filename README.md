# limbX

### Directory Structure

```
limbX
├── README.md
├── demo.py - demonstrates limb functionality
├── archived (hidden) - archived scripts
├── data - stores camera data from hmd RPi
└── limb
    ├── config.yml - config file with hardware specifications
    ├── classes.py - defines global classes
    ├── client.py - receives data from Tobii
    ├── hand.py - controls hand
    ├── servo.py - executes low level servo control
    ├── smart.py - calculates control sequences
    └── vision.py - uses ml to process camera data
```

### Machine Learning
# CONTROLLER FOR CONTINUOUS ROBOTIC LIMB

1. Given a desired (x,y) position, use the inverse neural network to predict an initial set of angles (z1,z2,z3) that will result in the robot reaching that position.

2. Use the forward neural network to predict the actual (x',y') position the robot will reach using the initial set of angles.

3. Calculate the error between the desired (x,y) position and the actual (x',y') position.

4. Adjust the initial set of angles using an optimization algorithm (such as gradient descent) to minimize the error between the desired and actual positions.

6. Repeat until the error is minimized to an acceptable threshold.


### Open Questions

- Currently all state information is stored in classes.py and passed down via mmain.py- should we use globals instead?
- Easy / automatic way to trigger release signal (e.g. grabbing coffee cup out of tentacle claw)
- Should grab and release have targetObj data inputs to help optimize process?

### Computer Vision

1. On command triggered from glasses: send image + eye gaze from Tobii to RPi.
2. Segment image into object regions.
3. Crop image to only include eye-tracked object without bg.
4. Tentacle collects images of environment at different angles.
   For each image:
5. Use SIFT to map features from cropped image to tentacle image.
6. Segment tentacle image into object regions.
7. Select the object region containing the most mapped points if number of mapped points exceeds threshold.
8. Find the x,y,z position of the center of the selected object region.
9. Average these positions as hybrid data.
10. Trigger control system to calculate and execute control sequence given targetRelPos.

### Smart Algorithm

```
INPUTS
servoDict = {
    1: {
        "lr": Servo(name="1_lr", pin=1),
        "ud": Servo(name="1_ud", pin=2)
    },
    2: {
        "lr": Servo(name="2_lr", pin=3),
        "ud": Servo(name="2_ud", pin=4)
    },
    3: {
        "lr": Servo(name="3_lr", pin=5),
        "ud": Servo(name="3_ud", pin=6)
    }
}
relativeObjPos = TargetPos(x=10,y=20,z=5)
```

```
OUTPUTS
servoTargetAngles = {
    1: {
        "lr": -10,
        "ud": 15
    },
    2: {
        "lr": 20,
        "ud": 25
    },
    3: {
        "lr": 15,
        "ud": 90
    }
}
```

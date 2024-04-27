# OdieEngine
<!---
Video Demo: <br>
[![Demo](https://img.youtube.com/vi/VIDEO_ID/0.jpg)](https://www.youtube.com/watch?v=VIDEO_ID) <br>
-->


## Overview
OdieEngine is a 3D Game Engine built with my DirectX11 Graphics API. This project aimed to make efficient systems for:
- Creating and controlling scenes
- Rendering
- Collision processing
- Key inputs
- Updates
- Alarms
- Sprite fonts
<br>
The aim was also to create debug tools to assist with creating these systems.
These systems were made with a combination of design patterns, including command pattern, visitor pattern, and more. <br/>

GameController Class Diagram:
![OdieClassDiagram](/images/ClassDiagram.png)

## Finite State Machine (Centipede)
One of the systems I implemented was the main centipede's movement. To do this, I used a finite state machine to create static state classes to decide between actions that the centipede took when going in a zig-zag motion.
<br/> 

Class Diagram: <br/>
![CentipedeGameFlow](/images/CentipedeFSM.png)

<br/>
The state machine helped me create a more computationally efficient update method for the centipede, which only checked the necessary data for the state that it was in, and also made graphical updates to the sprites easier.

## Command Pattern (Sound Manager)
Another pattern I learned to implement was the command pattern, which I used to create the centralized sound manager for the game. With the command pattern, I could avoid too many sounds combining at once by having a sound manager that would accept commands to play and stop a sound. 
This way, a sound was played only once per frame, even if multiple objects called to play the sound. <br>
![CentipedeSoundManager](/images/CentipedeSound.png)




# OdieEngine
<!---
Video Demo: <br>
[![Demo](https://img.youtube.com/vi/VIDEO_ID/0.jpg)](https://www.youtube.com/watch?v=VIDEO_ID) <br>
-->


## Overview
OdieEngine is a 3D Game Engine built with my DirectX11 [Graphics API](https://github.com/odzyoba4/DXGraphicsAPI). This project aimed to make efficient systems for:
- Creating and controlling scenes
- Rendering
- Collision processing
- Key inputs
- Updates
- Alarms
- Sprite fonts
<br>
The aim was also to create debug tools to assist with creating these systems.
These systems were made with a combination of design patterns, including command pattern, visitor pattern, and more.
 <br/>

<br/>
Class diagram including the GameObject system, which is an inheritable class for objects wishing to use collision detection, inputs, update and draw calls, or alarms:

![OdieClassDiagram](/images/ClassDiagram.png)

## Collision Processing
The collisions were detected using specialized classes for different collision volumes like a bounding sphere, axis-aligned bounding box(AABB), and the oriented bounding box(OBB).
Along with the volumes, I implemented a visualizer to test the collision accuracy, that accepted draw commands which could be called from anywhere in the code.
<br/> 

Class Diagram: <br/>
![CollisionDemo](/images/OdieCollision.PNG)

<br/>
These volumes were then tested against each other by using the visitor pattern. In this test, two unknown collision volumes call each other's Accept and Visit intersection functions, which allows the base class comparison to become a known derived class intersection test:
<br/>


<br/> ![OdieVisitorPattern](/images/OdieVisitorPattern.PNG)

<br>

## Key Input
The key input recognition system was designed to limit how much testing is done per frame for key presses. I used the command pattern to create a centralized key event manager, which was defined by a single key and an event (Pressed, Released, Held Down). This manager was responsible for registering Inputables (a sub-class of the GameObject) to Single Key Event Managers, which were responsible for detecting events for a unique key. <br>
![OdieKey](/images/KeyInput.png)

This way, while there is more registration done, the process to detect a key only happens once per frame, no matter how many objects need to know about the key event. Here is the detection that happens for a SingleKeyEventManager, after which it calls back all the necessary Inputable objects to inform them of a key event:


```C++
void SingleKeyEventManager::ProcessKeyEvent() {
	//check for press or release and callback corresponding inputables in list
	if (Keyboard::GetOdieKeyState(Key) && prevKeyState == false) { //key was up on last frame but is now down (pressed)
		prevKeyState = true;

		InputableCollection::iterator it;
		for (it = CollectionArr[KeyPressListIndex].begin(); it != CollectionArr[KeyPressListIndex].end(); ++it) {
			InputableAttorney::GameLoop::KeyPressed(Key, *it); //callback to keyPressed function in inputable
		}
	}
	else if (Keyboard::GetOdieKeyState(Key) && prevKeyState == true) { //key is down on last and current frame (held down)
		InputableCollection::iterator it;
		for (it = CollectionArr[KeyHoldListIndex].begin(); it != CollectionArr[KeyHoldListIndex].end(); ++it) {
			InputableAttorney::GameLoop::KeyHeldDown(Key, *it); //callback to keyHeldDown function in inputable
	
		}
	}
	else if (!Keyboard::GetOdieKeyState(Key) && prevKeyState == true) { //key was down on last frame but is now up (released)
		prevKeyState = false;

		InputableCollection::iterator it;
		for (it = CollectionArr[KeyReleaseListIndex].begin(); it != CollectionArr[KeyReleaseListIndex].end(); ++it) {
			InputableAttorney::GameLoop::KeyReleased(Key, *it); //callback to keyReleased function in inputable

		}
	}
}
```



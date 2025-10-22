# Ex.No: 10  Implementation of 2D/3D game  using Unity
### DATE:                                                                            
### REGISTER NUMBER :2122223040029 
### AIM: 
To develop FallingCube game in Unity where cubes fall from the top of the screen, the player avoids collisions, and the score increases over time or based on successfully avoiding cubes. The game demonstrates basic Unity physics, object spawning, collision detection, and scoring mechanics in Unity 
### Algorithm:
```
1.Initialize the Game Environment

 *Create a 3D/2D plane or background.

 *Set up the camera to view the playing area.

 *Add player object (e.g., a cube or character) with movement controls.

2.Set Player Controls

 * Capture input (keyboard, touch, or mouse).

* Move the player left or right within the screen boundaries.

Spawn Falling Cubes

Create a cube prefab.

Set a random x-position at the top of the screen.

Spawn cubes at regular intervals using InvokeRepeating or a coroutine.

Apply Physics to Falling Cubes

Use Rigidbody component or manually move cubes downward.

Optionally, increase falling speed gradually to increase difficulty.

Collision Detection

Detect collision between the player and falling cubes using OnCollisionEnter or OnTriggerEnter.

End the game or reduce lives if a collision occurs.

Score Management

Increment score over time or for each cube avoided.

Display score on UI using a Text element.

Game Over Handling

Stop spawning cubes.

Show final score on screen.

Optionally, provide a restart button.

Optional Enhancements

Vary cube sizes, colors, and falling speed.

Add power-ups or obstacles.

Add sound effects for collisions or scoring.
```  
### Program:
```
```
### Output:
<img width="1918" height="992" alt="image" src="https://github.com/user-attachments/assets/c91ae198-a6e6-4d4e-9a6a-97f83dd3f759" />


### Result:
Thus the game was developed using Unity and adopted _-----------AI technology.

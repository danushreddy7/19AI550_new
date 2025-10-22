# Ex.No: 10  Implementation of 2D/3D game  using Unity
### DATE:22-10-2025                                                                            
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

  *Capture input (keyboard, touch, or mouse).

  *Move the player left or right within the screen boundaries.

3.Spawn Falling Cubes

  *Create a cube prefab.

  *Set a random x-position at the top of the screen.

  *Spawn cubes at regular intervals using InvokeRepeating or a coroutine.

4.Apply Physics to Falling Cubes

  *Use Rigidbody component or manually move cubes downward.

  *Optionally, increase falling speed gradually to increase difficulty.

5.Collision Detection

 *Detect collision between the player and falling cubes using OnCollisionEnter or OnTriggerEnter.

 *End the game or reduce lives if a collision occurs.

6.Score Management

  *Increment score over time or for each cube avoided.

  *Display score on UI using a Text element.

7.Game Over Handling

 *Stop spawning cubes.

 *Show final score on screen.

 *Optionally, provide a restart button.

8.Optional Enhancements

 *Vary cube sizes, colors, and falling speed.

 *Add power-ups or obstacles.

 *Add sound effects for collisions or scoring.
```  
### Program:

1. PlayerController.cs
```
using UnityEngine;

public class PlayerController : MonoBehaviour
{
    public float speed = 10f;
    public float boundaryX = 8f;

    void Update()
    {
        float horizontalInput = Input.GetAxis("Horizontal");
        Vector3 movement = new Vector3(horizontalInput * speed * Time.deltaTime, 0, 0);
        transform.Translate(movement);

        // Clamp player within boundaries
        float clampedX = Mathf.Clamp(transform.position.x, -boundaryX, boundaryX);
        transform.position = new Vector3(clampedX, transform.position.y, transform.position.z);
    }
}

```
2. CubeSpawner.cs
```
using UnityEngine;

public class CubeSpawner : MonoBehaviour
{
    public GameObject cubePrefab;
    public float spawnInterval = 1.0f;
    public float spawnRangeX = 8f;
    public float spawnHeight = 10f;

    void Start()
    {
        InvokeRepeating("SpawnCube", 1f, spawnInterval);
    }

    void SpawnCube()
    {
        float randomX = Random.Range(-spawnRangeX, spawnRangeX);
        Vector3 spawnPosition = new Vector3(randomX, spawnHeight, 0);
        Instantiate(cubePrefab, spawnPosition, Quaternion.identity);
    }
}
```
3.FallingCube.cs
```
using UnityEngine;

public class FallingCube : MonoBehaviour
{
    public float fallSpeed = 5f;

    void Update()
    {
        transform.Translate(Vector3.down * fallSpeed * Time.deltaTime);

        // Destroy cube if it falls below the screen
        if (transform.position.y < -5f)
        {
            Destroy(gameObject);
            ScoreManager.instance.AddScore(1); // Add score for avoided cube
        }
    }
}
4. ScoreManager.cs

Manages score display
```
using UnityEngine;
using UnityEngine.UI;

public class ScoreManager : MonoBehaviour
{
    public static ScoreManager instance;
    public Text scoreText;
    private int score = 0;

    void Awake()
    {
        if (instance == null)
            instance = this;
    }

    public void AddScore(int points)
    {
        score += points;
        scoreText.text = "Score: " + score;
    }

    public int GetScore()
    {
        return score;
    }
}
```
5. GameOver.cs

Handles collisions and ends the game.
```
using UnityEngine;
using UnityEngine.SceneManagement;

public class GameOver : MonoBehaviour
{
    void OnCollisionEnter(Collision collision)
    {
        if (collision.gameObject.CompareTag("FallingCube"))
        {
            // Game Over
            Debug.Log("Game Over! Final Score: " + ScoreManager.instance.GetScore());
            Time.timeScale = 0f; // Pause the game
        }
    }
}
```
6. UI Setup

Create a Text UI element and link it to ScoreManager.scoreText.

Optional: Add a Game Over panel that activates when the player hits a cube.

7. Tags and Layers

Tag your falling cubes as "FallingCube".

Ensure player object is tagged appropriately.

- This setup includes:

Player movement with boundaries.

Random falling cubes.

Score system for avoided cubes.

Game over detection on collision.
```
### Output:
<img width="1918" height="992" alt="image" src="https://github.com/user-attachments/assets/c91ae198-a6e6-4d4e-9a6a-97f83dd3f759" />


### Result:
Thus the game was developed using Unity and adopted  UI -AI technology.

# Ex.No: 10  Implementation of 2D/3D game Catch the Falling Apple
### DATE:     22/10/25                                                                 
### REGISTER NUMBER :   212223230108
### AIM: 
To develop a game Catch the Falling Apple in Unity 
### Algorithm:
```
1) Create a new 2D Unity project and setup folders: Scenes, Scripts, Sprites, Prefabs.

2) Add Main Camera and Background sprite (Order in Layer = 0).

3) Create Canvas → ScoreText UI for displaying score.

4) Add Basket sprite with Rigidbody2D (Kinematic), BoxCollider2D (Is Trigger), Order in Layer = 1.

5) Create Apple sprite, add Rigidbody2D (Dynamic), CircleCollider2D, Tag = “Apple”, Order in Layer = 2.

6) Drag Apple into Prefabs folder and delete it from Scene.

7) Create PlayerController.cs to move basket horizontally and detect collisions with apples.

8) Create AppleSpawner.cs to spawn Apple prefab at random X positions at the top of the screen.

9) Create GameManager.cs to store score and update ScoreText when an apple is caught.

10) Apples fall due to physics; basket catches apples → destroy apple → increment score.

11) Optional: destroy apples that fall below screen to handle misses.

12) Optional: add sounds, lives, and difficulty scaling for polish.
```  
### Program:
##### PlayerController.cs
```c
using UnityEngine;

public class PlayerController : MonoBehaviour
{
    public float speed = 5f;
    private float screenHalfWidth;

    void Start()
    {
        float halfPlayerWidth = transform.localScale.x / 2f;
        screenHalfWidth = Camera.main.aspect * Camera.main.orthographicSize - halfPlayerWidth;
    }

    void Update()
    {
        float moveInput = Input.GetAxis("Horizontal");
        transform.Translate(Vector2.right * moveInput * speed * Time.deltaTime);

        // Keep player inside screen boundaries
        if (transform.position.x < -screenHalfWidth)
            transform.position = new Vector2(-screenHalfWidth, transform.position.y);
        if (transform.position.x > screenHalfWidth)
            transform.position = new Vector2(screenHalfWidth, transform.position.y);
    }

    void OnTriggerEnter2D(Collider2D col)
    {
        if (col.CompareTag("apple"))
        {
            Destroy(col.gameObject);
            FindObjectOfType<GameManager>().AddScore(1);
        }
    }

}

```

##### GameManager.cs
```c
using UnityEngine;
using TMPro;

public class GameManager : MonoBehaviour
{
    public TextMeshProUGUI scoreText;
    private int score = 0;

    void Start()
    {
        UpdateScoreText();
    }

    public void AddScore(int value)
    {
        score += value;
        UpdateScoreText();
    }

    void UpdateScoreText()
    {
        scoreText.text = "Score: " + score;
    }
}

```

##### AppleSpawner.cs
```c
using UnityEngine;

public class AppleSpawner : MonoBehaviour
{
    public GameObject applePrefab;
    public float spawnRate = 1.2f;
    private float screenHalfWidth;

    void Start()
    {
        // Calculate visible horizontal limit (camera-based)
        float halfAppleWidth = applePrefab.transform.localScale.x / 2f;
        screenHalfWidth = Camera.main.aspect * Camera.main.orthographicSize - halfAppleWidth;

        // Start spawning apples
        InvokeRepeating("SpawnApple", 1f, spawnRate);
    }

    void SpawnApple()
    {
        float xPos = Random.Range(-screenHalfWidth, screenHalfWidth);
        Vector2 spawnPos = new Vector2(xPos, 6f); // 6f = just above top of screen
        Instantiate(applePrefab, spawnPos, Quaternion.identity);
    }
}

```
### Output:

<img width="1919" height="1020" alt="1" src="https://github.com/user-attachments/assets/b5d89358-f204-4a90-9212-7216c2f205fc" />

<img width="1919" height="1022" alt="2" src="https://github.com/user-attachments/assets/b24a9e19-9f8a-4437-89b6-275d6c6adb50" />

<img width="1919" height="1025" alt="3" src="https://github.com/user-attachments/assets/27cfbe8e-5d32-4284-8fe2-436c0ed0142f" />

<img width="1919" height="1022" alt="4" src="https://github.com/user-attachments/assets/f411f386-a689-45b8-9f22-1e839dbd36bc" />


### Result:
Thus the game was developed using Unity and adopted Colliders, Triggers AI technology.

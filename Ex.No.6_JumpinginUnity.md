# Ex.No: 6  Implementation of Jumping  behaviour- Unity
### DATE: 10-09-2025                                                                         
### REGISTER NUMBER : 212223040029
### AIM: 
To write a program to simulate the process of jumping in Unity.
### Algorithm:
```
1. Create a new 3D Unity project
2. Add a Plane
3. Right-click Hierarchy → 3D Object → Plane → Rename to Ground
4. Add a Cube (Player)
5. Right-click Hierarchy → 3D Object → Cube → Rename to Player
6. Set Position: (0, 0.5, 0)
7. Add a Rigidbody to the Player
8. With the Player selected: Inspector → Add Component → Rigidbody
9. Set Constraints > Freeze Rotation X, Z (optional for stability)
10.Create the Jump Script and Apply the Script Player
11.Run the game
Press Play
Press Spacebar to jump
Your cube should only jump when touching the ground
```
###
**Program **
```
using UnityEngine;

public class PlayerJump : MonoBehaviour
{
    private Rigidbody rb;
    public float jumpForce = 5f;
    private bool isGrounded;

    void Start()
    {
        rb = GetComponent<Rigidbody>();
    }

    void Update()
    {
        if (Input.GetKeyDown(KeyCode.Space) && isGrounded)
        {
            rb.AddForce(Vector3.up * jumpForce, ForceMode.Impulse);
            isGrounded = false;
        }
    }

    private void OnCollisionEnter(Collision collision)
    {
        if (collision.gameObject.CompareTag("Ground"))
        {
            isGrounded = true;
        }
    }
}
```
### Output:

<img width="1212" height="606" alt="487595554-4a077f9b-3347-4aee-8da7-8724b0af72c1" src="https://github.com/user-attachments/assets/e4b1343d-d039-4901-be17-240cfc188003" />


<img width="1212" height="608" alt="487595615-e3656c8a-474a-4aa2-b0f3-67095f1dbcbe" src="https://github.com/user-attachments/assets/4d1af567-296f-4794-adf4-94f5c2e9ae37" />






### Result:
Thus the simple jumping behavior was implemented successfully.

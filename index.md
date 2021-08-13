# Game Design in Unity
My Project is to create a game using a game engone called unity. I am going to create a 3D platformer game using blender and Unity. I created the character and animations for it in blender and then uploaded them into unity. Then I created the platforms in unity that my character would be jumping on. 

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Andrew D | Trevor Day School | Engineering | Incoming Junior

![Headstone Image](![2021-08-13 (2)](https://user-images.githubusercontent.com/87190446/129377983-0c930fb8-2c53-43fe-ae39-e72ee9e2a1f1.png))

# Second Milestone
I imported the animation files and character from blender into Unity. I then opened a C# script and coded the character to be able to move fowards and backwards with the W and S keys. I then coded in gravity so the character would not be walking on air. Then I added so the player could rotate the character using their mouse. 

[![Third Milestone]   {:target="_blank" rel="noopener"}

# First Milestone 
For my first milestone I went into blender and sculpted a 3d mesh that resembled a character. Next I setup the mesh so it was attached to a rig which acted as bones. I used the rig that I had made for the character to position it in a way to be animated. I made animations for the character wich could play during the game when the character was running and jumping.
[![First Milestone]![Milestone 1 gif](https://user-images.githubusercontent.com/87190446/128534754-52e9281e-41b7-4409-b12f-c937d0700579.gif){:target="_blank" rel="noopener"}
<iframe width="560" height="315" src="https://www.youtube.com/embed/q6YkJCUyf84" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

# Reflection
One of the biggest challenges was creating player movement. Most times when I uploaded code I would get error messages that took some time to fix. I learned that in engineering perseverance is the most crucial part in achieving success.


# Movement Code
```c#
//using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerMovement : MonoBehaviour
{
    //VARIABLES
    [SerializeField] private float moveSpeed;
    [SerializeField] private float walkSpeed;
    [SerializeField] private float runSpeed;

    private Vector3 moveDirection;
    private Vector3 velocity;

    [SerializeField] private bool isGrounded;
    [SerializeField] private float groundCheckDistance;
    [SerializeField] private LayerMask groundMask;
    [SerializeField] private float gravity;

    [SerializeField] private float jumpheight;

    //REFRENCES
    private CharacterController controller;

    private void Start ()
    {
        controller = GetComponent<CharacterController>();
    }

    private void Update()
    {
        Move();
    }

    private void Move()
    {
        isGrounded = Physics.CheckSphere(transform.position, groundCheckDistance, groundMask);

        if(isGrounded && velocity.y < 0) 
        {
            velocity.y = -2f;
        }

        float moveZ = Input.GetAxis("Vertical");

        moveDirection = new Vector3(0, 0, moveZ);
        moveDirection = transform.TransformDirection(-moveDirection);

        if (isGrounded)
        {
            if (moveDirection != Vector3.zero && !Input.GetKey(KeyCode.LeftShift))
            {
                Walk();
            }
            else if (moveDirection != Vector3.zero && Input.GetKey(KeyCode.LeftShift))
            {
                Run();
            }
            else if (moveDirection == Vector3.zero)
            {
                Idle();
            }

            moveDirection *= moveSpeed;

            if(Input.GetKeyDown(KeyCode.Space))
            {
                Jump();
            }
        }

        controller.Move(moveDirection * Time.deltaTime);

        velocity.y += gravity * Time.deltaTime;
        controller.Move(velocity * Time.deltaTime);
    }

    private void Idle()
    {

    }

    private void Walk()
    {
        moveSpeed = walkSpeed;
    }

    private void Run()
    {
        moveSpeed = runSpeed;
    }
    private void Jump()
    {
        velocity.y = Mathf.Sqrt(jumpheight * -2 * gravity);
    }
}

```

# Camera Controller Code
```c#
//using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class CameraController : MonoBehaviour
{
    //VARAIBLES
    [SerializeField] private float mouseSensitivity;

    //REFERENCES
    private Transform parent;

    private void Start()
    {
        parent = transform.parent;
        Cursor.lockState = CursorLockMode.Locked;
    }

    private void Update()
    {
        Rotate();
    }

    private void Rotate()
    {
        float mouseX = Input.GetAxis("Mouse X") * mouseSensitivity * Time.deltaTime;

        parent.Rotate(Vector3.up, mouseX);
    }
}
```

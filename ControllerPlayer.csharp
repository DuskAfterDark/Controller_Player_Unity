using System.Controller.Generic;
using UnityEngine;

[RequireComponent(typeof(CharacterController))]
public class PlayerMovement : MonoBehaviour
{
    public Camera playerCamera;
    public Light flashlight; 
    public float walkSpeed = 6f;
    public float runSpeed = 12f;
    public float jumpPower = 7f;
    public float gravity = 10f;
    public float lookSpeed = 2f;
    public float lookXLimit = 45f;
    public float defaultHeight = 2f;
    public float crouchHeight = 1f;
    public float crouchSpeed = 3f;
    public float bobbingSpeed = 14f;
    public float bobbingAmount = 0.05f;

    private Vector3 moveDirection = Vector3.zero;
    private float rotationX = 0;
    private CharacterController characterController;

    private bool canMove = true;
    private bool isFlashlightOn = false; // Flashlight state
    private float defaultCameraY;
    private float bobbingTimer = 0;

    void Start()
    {
        characterController = GetComponent<CharacterController>();
        Cursor.lockState = CursorLockMode.Locked;
        Cursor.visible = false;

        if (playerCamera != null)
        {
            defaultCameraY = playerCamera.transform.localPosition.y;
        }
    }

    void Update()
    {
        Vector3 forward = transform.TransformDirection(Vector3.forward);
        Vector3 right = transform.TransformDirection(Vector3.right);

        bool isRunning = Input.GetKey(KeyCode.LeftShift);
        float curSpeedX = canMove ? (isRunning ? runSpeed : walkSpeed) * Input.GetAxis("Vertical") : 0;
        float curSpeedY = canMove ? (isRunning ? runSpeed : walkSpeed) * Input.GetAxis("Horizontal") : 0;
        float movementDirectionY = moveDirection.y;
        moveDirection = (forward * curSpeedX) + (right * curSpeedY);

        if (Input.GetButton("Jump") && canMove && characterController.isGrounded)
        {
            moveDirection.y = jumpPower;
        }
        else
        {
            moveDirection.y = movementDirectionY;
        }

        if (!characterController.isGrounded)
        {
            moveDirection.y -= gravity * Time.deltaTime;
        }

        if (Input.GetKey(KeyCode.R) && canMove)
        {
            characterController.height = crouchHeight;
            walkSpeed = crouchSpeed;
            runSpeed = crouchSpeed;
        }
        else
        {
            characterController.height = defaultHeight;
            walkSpeed = 6f;
            runSpeed = 12f;
        }

        characterController.Move(moveDirection * Time.deltaTime);

        if (canMove)
        {
            rotationX += -Input.GetAxis("Mouse Y") * lookSpeed;
            rotationX = Mathf.Clamp(rotationX, -lookXLimit, lookXLimit);
            playerCamera.transform.localRotation = Quaternion.Euler(rotationX, 0, 0);
            transform.rotation *= Quaternion.Euler(0, Input.GetAxis("Mouse X") * lookSpeed, 0);
        }

        // Camera Bobbing
        if (characterController.isGrounded && (curSpeedX != 0 || curSpeedY != 0))
        {
            bobbingTimer += Time.deltaTime * bobbingSpeed;
            float newY = defaultCameraY + Mathf.Sin(bobbingTimer) * bobbingAmount;
            playerCamera.transform.localPosition = new Vector3(playerCamera.transform.localPosition.x, newY, playerCamera.transform.localPosition.z);
        }
        else
        {
            bobbingTimer = 0;
            playerCamera.transform.localPosition = new Vector3(playerCamera.transform.localPosition.x, defaultCameraY, playerCamera.transform.localPosition.z);
        }

        // Flashlight Toggle
        if (Input.GetKeyDown(KeyCode.F))
        {
            isFlashlightOn = !isFlashlightOn;
            if (flashlight != null)
            {
                flashlight.enabled = isFlashlightOn;
            }
        }
    }
}

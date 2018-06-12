# Roll A Ball Extra Stretch Goals

## Setting Up Walls

1.  Create empty walls using the cube game object. Reset it to origin
2.  Create cube west wall and reset. Parent Walls
3.  Scale west walls by (0.5, 2, 20.5). Use translate tool to place. It should be -10
4.  Duplicate for east, north and south wall, rotate these walls.
    Test! At this point if you have any questions at all, you should be shooting them my way.

Now that as have our “game board” and barriers so we dont fall of our screen, let’s set up some targets for our player.

## Creating Collectables

1.  Create a new game object **Sphere**
2.  Rename **Sphere** game object to **PickUp**
3.  Change Scale to (0.25, 1, 1)
4.  Change position to (0, 0.5, 0)
5.  Change Rotation to (60, 25, 45)
6.  Select **PickUp** object, then in the inspector window click **Add Component** then select or search **New Script**
7.  Name the script **Rotator** and write the following code:

```
public class Rotator : MonoBehaviour {
  void Update () {
    transform.Rotate (new Vector3 (45, 45, -45) * Time.deltaTime);
    }
}
```

8.  Drag the **PickUp** game object from the Hierarchy window into the **Prefab** folder in the Project window
9.  In the Hierarchy window click on Create and select **Create Empty**
10. Let's rename this empty object to **PickUpParent**
11. Drag the **PickUp** game object into the **PickUpParent** so that it becomes its child
12. Change editor to **Global** Mode on upper left hand toolbar
13. Duplicate the **PickUp** this can be done by typing cmd+d or right click **PickUp** and select **Duplicate**
14. Place more of these around your game board
15. Edit your **PlayerMove** script and add the following:

```
	void OnTriggerEnter (Collider other) {
		if (other.gameObject.CompareTag ("Pick Up")) {
			other.gameObject.SetActive (false);
		}
	}
```

16. Click on the Prefab folder and find the **PickUp** game object. Next, find the Tag drop down and add a new tag called **Pick Up**.
17. After you create the tag, make sure you have **PickUp** selected in the prefab folder, next look in the inspector window select the **Pick Up** from the Tag drop down.
18. Highlight all of your **PickUp** objects in the Hierarchy window and check Is Trigger in the Sphere Object component in the Inspector window.

## Tracking Score & Messages

1.  Open the **PlayerMove** script
2.  Create a variable called count `private int count;`
3.  Inside of the **Start()** function add `count = 0`
4.  Inside of the **OnTriggerEvent()** add `count = count + 1` below the line that says `other.gameObject.SetActive(false);`
    Your code should now look like the following:

```
public class PlayerMove : MonoBehaviour {
	public float speed;
	private Rigidbody rb;
	private int count;
	void Start () {
		rb = GetComponent<Rigidbody> ();
		count = 0;
	}
	void FixedUpdate () {
		float moveHorizontal = Input.GetAxis ("Horizontal");
		float moveVertical = Input.GetAxis ("Vertical");
		Vector3 movement = new Vector3 (moveHorizontal, 0f, moveVertical);
		rb.AddForce (movement * speed);
	}

	void OnTriggerEnter (Collider other) {
		if (other.gameObject.CompareTag ("Pick Up")) {
			other.gameObject.SetActive (false);
			count = count + 1;
		}
	}
}
```

5.  Create a new UI text element from the Hiearchy window
6.  Rename the text element and change the text to **Count Text**
7.  Change the Anchor position by selecting a preset Anchor. To do that click on the square in the inspector window. Hold down shift and command and select the upper left square position.
8.  Change the x and y position to (10, -10)
9.  Open the **PlayerMove** script and write the following code:
10. At the top of the script add the following line of code: `using UnityEngine.UI`
11. Create the following variable: `public Text countText`
12. Add the line of code to the **Start()** function: `countText.text = "Count : " + count.ToString ();` This line must be written AFTER the line `count = 0`
13. Inside of the **OnTriggerEvent()** function, BELOW the `count = count + 1 line` add the following line of code: `countText.text = "Count : " + count.ToString ();`
14. Select the **Player** game object from the Hierarchy window. Next, in the Inspector window find the **PlayerMove** script. Drag the **Count Text** UI element from the Hierarchy window into the **Count Text** box in the Inspector window.

Your FINAL **PlayerMove** script should look like:

```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class PlayerMove : MonoBehaviour {
	public float speed;
	private Rigidbody rb;
	private int count;
	public Text countText;
	void Start () {
		rb = GetComponent<Rigidbody> ();
		count = 0;
		countText.text = "Count : " + count.ToString ();
	}
	void FixedUpdate () {
		float moveHorizontal = Input.GetAxis ("Horizontal");
		float moveVertical = Input.GetAxis ("Vertical");
		Vector3 movement = new Vector3 (moveHorizontal, 0f, moveVertical);
		rb.AddForce (movement * speed);
	}

	void OnTriggerEnter (Collider other) {
		if (other.gameObject.CompareTag ("Pick Up")) {
			other.gameObject.SetActive (false);
			count = count + 1;
			countText.text = "Count : " + count.ToString ();
		}
	}
}
```

Now your game is complete! Add Extra objects or create a maze. Be creative!

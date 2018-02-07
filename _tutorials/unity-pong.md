---
layout: notes
title: Make A Pong Game With Unity 2D
course_link: /register/
---

# Let's Make: PONG

![Classic Pong screenshot](/img/tutorials/unity-pong/classic_pong_screenshot.png)

By Will Oldham. Updates by [Nick Such](https://github.com/nicksuch/){:target="_blank"} and [Caleb Cornett](https://github.com/TheSpydog/){:target="_blank"}.

> Note, July 2017: Updated for Unity 5.6

Today, the name of the game is Pong. We're going to be using Unity v5.6 or later, and we'll use C# as our coding language. No prior experience with Unity or C# is required. It will only take you about two hours to complete the tutorial, and at the end you'll have made your own version of Pong!

Ready? It's gonna be awesome.

So, when we think about making Pong, we might think of several simple mechanics that need to be created:

1. You have a Background to play on

2. You have a set of Paddles that go up and down

3. You have a ball that bounces off walls and paddles

4. You have a set of side walls that you hit to score

5. You have a score at which you win

6. You have a rage quit button (or reset button)

Seems simple enough. Let's get started!

## Step One: The Setup

First, download this [Unity Pong assets file](/files/unity-pong-assets.zip){:target="_blank"}. It contains images and other assets that we'll be using in this tutorial.

Now, let's start with the paddles. Open up Unity and follow these steps:

1. From the welcome screen, click 'Projects'. (If the Unity editor is already open, click 'File' > 'New Project' instead.)

2. Click 'New'. Then you should see something like this:
![Unity's New Project window](/img/tutorials/unity-pong/new_project.png)

3. Name the Project something like 'Pong Game'.

4. Make sure you choose '2D' and not '3D'. I think 3D Pong is a little ambitious for our first go, so 2D is a better choice.

5. Set the slider for 'Enable Unity Analytics' to Off.

6. Hit 'Create Project'. And that's it!

Once the project has been created, you should see a 2D grid appear in the Scene view. If you don't, make sure the '2D' button is pressed along the top toolbar of Scene view. (You can see it highlighted in yellow in this image.)

![2D Toolbar Button](/img/tutorials/unity-pong/2d_button.png)

Setting up Unity in 2D mode does several things. First off, it sets up our game camera so that everything is viewed from a 2D perspective. It also tells Unity to import images as Sprites instead of Textures. You'll see how this helps us in just a bit.

Remember that [Unity Pong assets file](/files/unity-pong-assets.zip){:target="_blank"} that we downloaded? Now's a great time to unzip that file. You'll see a folder called `unitypong-assets`, which will contain a set of images, fonts, and other assets that we'll be using today. Select all, <kbd>click + drag</kbd> these files into the Project pane at the bottom of your Unity window. They should end up in your Assets folder. If your Unity Editor window looks much different, then you may want to set your editor Layout to "Default".

![Downloaded assets](/img/tutorials/unity-pong/unitypong-assets.png)

The background image is going to be in your Assets folder as "Background.jpg". You can add and use another background image you want, but by doing this, you may have to adjust some size or view settings. You should see it in your Project pane:

![Background image selected in Project pane](/img/tutorials/unity-pong/background_project_pane.png)

Drag the image into the Hierarchy pane, just below `Main Camera`. It should appear in your Scene view, and be centered by default. If it's not centered, use the Inspector pane to set its Transform Position to `(0, 0, 0)`. If this doesn't work, make sure you set that 2D perspective lock set in the menu bar above your Scene. Select your Background, now in the Hierarchy pane, and you should see it show up in the Inspector pane like this:

![Background image in hierarchy](/img/tutorials/unity-pong/background_hierarchy_pane.png)

First, up at the top of the Inspector under 'Transform', change its scale to `(0.75, 0.75, 1)`. This will make it look better in our game. Now look at the 'Sprite Renderer' component. We want to make sure the background actually renders behind our other sprites. Go to 'Sorting Layer', and click 'Add Sorting Layer'. The Inspector pane should now look like this:

![Sorting Layers](/img/tutorials/unity-pong/sorting_layer.png)

Click the + icon to add our new layer. Change its name from 'New Layer' to 'Background', then click and drag our new layer above the Default layer. This makes it so that anything on the Background sorting layer will appear behind anything on the Default layer, which is exactly what we want.

Now re-select your Background object in the Hierarchy pane so that it will show up in the Inspector again. In the 'Sprite Renderer' component, click the dropdown menu for Sorting Layer and choose our new 'Background' layer. Your Inspector should look like this:

![Background image in Background Sorting layer](/img/tutorials/unity-pong/background_sorting_layer.png)

Now we're cooking. Select the Main Camera object in your Hierarchy pane. The camera object controls how we see the game world (or 'Scene', in Unity terms). When we're playing our Pong game, it's actually possible that our background won't be big enough to cover the whole screen. We need to make two small changes in our Main Camera to take care of that.

In the Inspector, under the 'Camera' component, change the 'Size' to `3`. This will zoom in slightly on our background. Next, click on the 'Background' property of the Camera. This lets us change the color that will be visible on the edges of our screen if the background isn't big enough. In the color picker, choose the color black, with RGBA values of `(0,0,0,0)`. Then the Camera component in the Inspector should look like this:

![MainCamera properties](/img/tutorials/unity-pong/main_camera_inspector.png)

All right! Now is a great time to save your Scene. Go to File -> Save Scene As... We're going to call our Scene 'Main', and we can save it right in our Assets folder. Once you save, you should see a file called `Main.unity` from your Project pane.

![Save Scene file as Main.unity](/img/tutorials/unity-pong/saved_scene_main.png)

## Step Two: The Paddles

The next obvious step is to make our paddles. Find the image called 'PongPaddle' in the Assests folder, within your Project pane.

Now drag the paddle onto the scene in scene view. A 'Player' object should appear in your Hierarchy menu. Rename that to Player01. Cool. Click on that. Over in the Inspector, click the Tag dropdown and select 'Player'. Set Position to (X, Y, Z) = `(-4, 0, 0)` and Scale to `(0.5, 1.5, 1)`. It should look like this in the Inspector:

![Player setup](/img/tutorials/unity-pong/inspector_4.png)

All we're doing here is making sure that the paddle is positioned where we want it. You could position it wherever, but I like this location the best. The Sorting Layer and other settings don't matter this time because we want this object to be drawn on top, which is what Unity defaults to.

Next we're going to add two components to the Player object. Click on the 'Add Component' button, and then on 'Physics 2D.' Once you've done that add both a 'Box Collider 2D' and a 'Rigidbody 2D.' The Box Collider 2D is to make sure the ball will bounce off your paddle, and the Rigidbody 2D is there so we can move the paddle around.

Note: It's important to use Physics, BoxCollider, and RigidBody **2D** here because 3D versions of those exist - that's not what we want in our 2D game though. 

Now, we're going to make a few changes to the 'Rigidbody 2D' component. Unity has a great, realistic physics system built in that calculates the effects of gravity, friction, and other forces on any object that has a Rigidbody 2D component. That system is often very handy, but we don't want to use it for our paddles. Our paddles aren't exactly realistic -- they're just kind of floating in space and they only move when we tell them to. Thankfully Unity gives us a way to tell our Rigidbody 2D to only move when we tell it to. We just have to click the 'Body Type' dropdown menu and select 'Kinematic'.

Your Inspector should now look like this:

![Physics - Box Collider 2D and Rigidbody 2D](/img/tutorials/unity-pong/player01_physics.png)

Now we want to do the hard part: add a Script for movement for our paddles. I'm going to share the code below. I recommend typing each line. It might seem quicker to copy and paste code, but typing it yourself really does help you understand it.

To add a script, make sure that Player01 is still selected in your Hierarchy pane, then go to 'Add Component', and then 'New Script.' Call this one 'PlayerControls' and make sure the language is C# (or CSharp, as it is spelled out in Unity). Hit 'Create and Add'. Now <kbd>double click</kbd> the icon that appears below in the Project pane to open it up in [MonoDevelop](http://en.wikipedia.org/wiki/MonoDevelop), Unity's Integrated Development Environment (or IDE) - essentially, it's the program we use to write Unity scripts. On Windows, you may be using Microsoft's Visual Studio IDE, but this process will be similar.

![Create New Script](/img/tutorials/unity-pong/image_6.png)

### Code Breakdown

In short, the first three lines are packages of pre-written code we want to tell our program it can use.

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
```

The next line is the class name, the name of our file. It's the same thing that we named our Component.

```csharp
public class PlayerControls : MonoBehaviour {
```

The next few lines are variables we need to write ourselves. The first two lines we add will denote the keys that we'll press to move the paddles (W goes up, S goes down), and the next one is the speed of the paddle. The 'boundY' variable is the highest position that we want our paddle to go. This keeps it from moving off the edges of the screen. The last variable is a reference to our Rigidbody that we'll use later.

```csharp
public KeyCode moveUp = KeyCode.W;
public KeyCode moveDown = KeyCode.S;
public float speed = 10.0f;
public float boundY = 2.25f;
private Rigidbody2D rb2d;
```

By making these variables 'public,' we can adjust them through our Unity interface as well. If we have variables we don't want other developers to see in the Unity interface, we should call them 'private'.

`Start()` is a function that is called when we first launch our game. We'll use it to do some initial setup, such as setting up our Rigidbody2D:

```csharp
void Start () {
    rb2d = GetComponent<Rigidbody2D>();
}
```

`Update()` is a function that is called once per frame. We'll use it to tell us what button is being pressed, and then move the paddle accordingly, or, if no button is pressed, keep the paddle still. Lastly we'll bound the paddle vertically between +boundY and -boundY, which will keep it inside the game screen at all times.

```csharp
void Update () {
    var vel = rb2d.velocity;
    if (Input.GetKey(moveUp)) {
        vel.y = speed;
    }
    else if (Input.GetKey(moveDown)) {
        vel.y = -speed;
    }
    else if (!Input.anyKey) {
        vel.y = 0;
    }
    rb2d.velocity = vel;

    var pos = transform.position;
    if (pos.y > boundY) {
        pos.y = boundY;
    }
    else if (pos.y < -boundY) {
        pos.y = -boundY;
    }
    transform.position = pos;
}
```

*NOTE: Make sure that you name your functions the same thing that I do. For instance, Unity knows to run the 'Update()' once every frame, because it is called 'Update()'. If you called it anything else (for instance, if you typed 'update' with a lowercase 'u' or misspelled it like 'Updaate'), it will not work. When you're writing code, you have to be exact or the computer might misunderstand what you want it to do.*

Cool. Save that, and exit out. Now, when we go back to Unity, we should have a paddle that moves. To test our game, <kbd>click</kbd> the play button (&#9658;) at the top of the screen. Use the W and S keys to move. Does it work? Awesome!

Now, it's important to note something here. You have just entered 'Play Mode'. This is a really neat feature in Unity that lets you not only test your game but also make changes to it while you're playing! You'll know you're in Play Mode when the editor gets darker and your play button looks like this:

![Play mode symbol](/img/tutorials/unity-pong/playmode.png)

Try clicking the Player01 object in the Hierarchy and changing the position of the paddle in the Inspector while the game is running in Play Mode. This is super handy for trying out new ideas in real time. But I have to warn you:

*The changes you make during Play Mode do not stay when you close Play Mode.*

In other words, always make sure you have stopped Play Mode (by pressing the play button (&#9658;) again) before you make significant changes to the game. Otherwise all your recent work will be erased whenever you stop testing the game. Trust me, that's no fun.

Go back to the scene view, and click on our 'Player01' object. Under the Inspector pane, you should see a place to change the key bindings for up and down, the speed that it moves at, and the bounding value for the paddle's position. Mess around with those as you please.

The next step is to make a second paddle. All we need to do is <kbd>right click</kbd> 'Player01' in the Hierarchy menu, and choose Duplicate from the menu that appears when we right click. Rename it to be 'Player02'. Next, change its key bindings (I recommend using 'Up Arrow' for up and 'Down Arrow' for down), and move it to be the opposite location on the board - change the X value in its Transform Position to be positive. Player01 should be on the left, Player02 on the right. Now go to 'Game' and test this one too. That work? AWESOME! You should have something that looks like this now:

![image alt text](/img/tutorials/unity-pong/image_12.png)

If so, move on - if not, go back and make sure you've done everything right. Your completed C# script should look like this: [PlayerControls.cs](https://github.com/ainc/unity-pong/blob/unity5/Assets/PlayerControls.cs)

## Step Three: The Ball

You've made it to the Ball. Congrats! Things get more complicated from here on, but remember, harder is funner (Let's just pretend that's a word)!

The first step is going to be finding the Ball image in the Project pane. Drag the Ball into the Scene, same as our Paddles. There should now be an object in the Hierarchy pane named 'Ball'. Click on it, then head over to the Inspector pane to get the ball rolling. First, add a custom Tag called "Ball", then go back and assign this new tag to our Ball. (This process is just like when you added a Sorting Layer to your background, except you click 'Add Tag' instead of 'Add Sorting Layer'.)

![Tagging the Ball](/img/tutorials/unity-pong/assign_ball_tag.png)

Next, change the scale of our ball to `(0.5, 0.5, 1)`. A smaller ball helps make the game not quite so awkward.

Next we need to add similar components that we did to the paddle. Click the Add Component button, then in Physics 2D, let's add 'Circle Collider 2D' and of course 'Rigidbody 2D.' In the Circle Collider, change the Radius to `0.23`. That way the collider wraps around the ball much tighter.

We also want to apply a Physics 2D Material. You can use the 'BallBounce' Material that was in the dowloadable assets pack for this. If you select it and look at the Inspector, you'll see the friction value is `0`, and the bounce factor is `1`. That way our ball doesn't experience friction from anything, including our paddles and wall. The bounciness factor means that it also doesn't lose any speed. It bounces back with the exact same speed it hit the object with.

To apply the material, select 'Ball' in the inspector. Drag 'BallBounce' to the 'Circle Collider 2D' box. We also need to adjust several settings in 'Rigidbody 2D' so we can get our desired pong ball behavior. It should look like this at the end:

![Physics settings for Ball](/img/tutorials/unity-pong/ball_inspector_physics.png)

But of course, to actually get the Ball to move, we need a script. With 'Ball' still selected in your Hierarchy, click Add Component in the Inspector pane. Create a New Script, this time called 'BallControl', with the C Sharp language selected. <kbd>Double click</kbd> on the new BallControl script to open it in MonoDevelop. I'll do the same thing as before, and post the code with an explanation of what's happening:

### Code Breakdown

First, as always, we import our packages and confirm that our class name matches our filename.

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class BallControl : MonoBehaviour {
```

We need to declare a couple variables that we'll use later.

```csharp
private Rigidbody2D rb2d;
private Vector2 vel;
```

We also need a `GoBall()` function, that will choose a random direction (left or right), then make the ball start to move.

```csharp
void GoBall(){
    float rand = Random.Range(0, 2);
    if(rand < 1){
        rb2d.AddForce(new Vector2(20, -15));
    } else {
        rb2d.AddForce(new Vector2(-20, -15));
    }
}
```

In our `Start()` function, we'll initialize our `rb2d` variable. Then we'll call the `GoBall()` function, using `Invoke()`, which [allows us to wait](https://docs.unity3d.com/ScriptReference/MonoBehaviour.Invoke.html) before execution. This will wait two seconds to give players time to get ready before the ball starts moving.

```csharp
void Start () {
    rb2d = GetComponent<Rigidbody2D>();
    Invoke("GoBall", 2);
}
```

`ResetBall()` and `ResartGame()` are two functions used by other scripts which we will write later. `ResetBall()` is used when a win condition is met. It stops the ball, and resets its position to the center of the board.

```csharp
void ResetBall(){
    vel = Vector2.zero;
    rb2d.velocity = vel;
    transform.position = Vector2.zero;
}
```

`RestartGame()` is used when our restart button is pushed. We'll add that button later. This function uses `ResetBall()` to center the ball on the board. Then it uses `Invoke()` to wait 1 second, then start the ball moving again.

```csharp
void RestartGame(){
    ResetBall();
    Invoke("GoBall", 1);
}
```

`OnCollisionEnter2D()` waits until we collide with a paddle, then adjusts the velocity appropriately using both the speed of the ball and of the paddle.

```csharp
void OnCollisionEnter2D (Collision2D coll) {
    if(coll.collider.CompareTag("Player")){
        vel.x = rb2d.velocity.x;
        vel.y = (rb2d.velocity.y / 2.0f) + (coll.collider.attachedRigidbody.velocity.y / 3.0f);
        rb2d.velocity = vel;
    }
}
```

Nifty swifty, neato spedito, our game is looking swell. Let's recap. We have two paddles that work, and now a ball that bounces around realistically. The completed script we just added should look like this: [BallControl.cs](https://github.com/ainc/unity-pong/blob/unity5/Assets/BallControl.cs). The Game view should have two paddles and a ball, ready for us to play!

So we're done right? Not quite!

## Step Four: The Walls

So you may have also noticed by now that your ball can fly off the screen. Really, you play a point, and then the game is over. Not the most fun thing in the world. So we need to make some walls. Let's first make a game object to contain our walls. Make sure nothing is selected in the Hierarchy pane, then <kbd>right click</kbd> some empty space in the Hierarchy and choose 'Create Empty'. It will create a plain ol' empty game object, which we will name 'Walls'. We don't need to change anything except to make sure that it has a position of `(0,0,0)`.

Now to make an actual wall. In the Hierarchy pane, <kbd>right click</kbd> on the 'Walls' object we just made and choose 'Create Empty'. This will make a new game object that is a 'child' of the Walls object. For our purposes, having our walls as children of 'Walls' will help us keep the hierarchy nice and clear, since we can click the little arrow next to 'Walls' whenever we don't want to see each individual wall object. The idea of 'child' game objects is actually very powerful and does more than just organize things, but we won't go into it much in this tutorial.

Anyway, call this new child object 'RightWall.' Also make sure you go to 'AddComponent' and add a Box Collider 2D. That way the ball will bounce off of the wall object, just like we want!

![Right wall](/img/tutorials/unity-pong/rightwall.png)

Now duplicate our new wall object 3 times. Call those duplicates 'LeftWall', 'TopWall', and 'BottomWall'. Now we need to move and size those walls so they are in the right spot in our game view - right now, they're just points sitting in the middle of our board. We need to make them look like walls.

For the RightWall, set its Position to `(5.8, 0, 0)` and its Scale to `(1, 6, 1)`. This will make it nice and tall. The LeftWall is the same, except its X Position is `-5.8`.

TopWall should be positioned at `(0, 3.5, 0)` with a scale of `(10.7, 1, 1)`, and BottomWall has the same scale but with a Y Position of `-3.5`. If you click on the parent 'Walls' object, you should now see this in the Scene view:

![Walls in the scene view](/img/tutorials/unity-pong/walls_sceneview.png)

One cool thing: you can now hit play (&#9658;), and the ball will bounce off the walls! Next comes an important, but slightly harder part. We need to make this an actual *game*, not just a ball bouncing around. We need a score system, a way to display the score, some win condition, and a reset button. That's right. It's time for….

## Step Five: The Scoring User Interface

First things first, we need a game object for our HUD (or Heads-Up Display). <kbd>Right click</kbd> an empty space in the Hierarchy pane (not on the Walls game object!) and choose 'Create Empty'. Call the new object 'HUD', center its position at `(0, 0, 0)`, and add a new script to the HUD object called 'GameManager'.

This is going to be a fairly long script, but it's really important for our game!

### Code Breakdown

First, as always, we import our packages and declare our class.

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class GameManager : MonoBehaviour {
```

Next we make four variables. The first two variables are just integers to keep track of the scores for the two players. The next is a GUI object. 'GUI' stands for graphical user interface. This object is going to be responsible for displaying all our different buttons and graphics. We'll make this skin object in Unity after we finish here. The last variable is a reference to our ball object.

```csharp
public static int PlayerScore1 = 0;
public static int PlayerScore2 = 0;

public GUISkin layout;

GameObject theBall;
```

Then comes the `Start()` function, which we use when the game first starts.

```csharp
void Start () {
    theBall = GameObject.FindGameObjectWithTag("Ball");
}
```

Next is the `Score()` function. It will get called by another script we write in just a minute, that detects when the ball hits the side walls.

```csharp
public static void Score (string wallID) {
    if (wallID == "RightWall")
    {
        PlayerScore1++;
    } else
    {
        PlayerScore2++;
    }
}
```

The `OnGUI()` function takes care of displaying the score and the reset button functionality. Then, it checks every time something happens if someone has won yet, and triggers the function `ResetBall()` if someone has.

```csharp
void OnGUI () {
    GUI.skin = layout;
    GUI.Label(new Rect(Screen.width / 2 - 150 - 12, 20, 100, 100), "" + PlayerScore1);
    GUI.Label(new Rect(Screen.width / 2 + 150 + 12, 20, 100, 100), "" + PlayerScore2);

    if (GUI.Button(new Rect(Screen.width / 2 - 60, 35, 120, 53), "RESTART"))
    {
        PlayerScore1 = 0;
        PlayerScore2 = 0;
        theBall.SendMessage("RestartGame", 0.5f, SendMessageOptions.RequireReceiver);
    }

    if (PlayerScore1 == 10)
    {
        GUI.Label(new Rect(Screen.width / 2 - 150, 200, 2000, 1000), "PLAYER ONE WINS");
        theBall.SendMessage("ResetBall", null, SendMessageOptions.RequireReceiver);
    } else if (PlayerScore2 == 10)
    {
        GUI.Label(new Rect(Screen.width / 2 - 150, 200, 2000, 1000), "PLAYER TWO WINS");
        theBall.SendMessage("ResetBall", null, SendMessageOptions.RequireReceiver);
    }
}
```

The 'SendMessage' call is something we've been using a lot in this chunk of code - it will trigger any function that matches the name that we send it in a class we specify. So when we say `theBall.SendMessage("ResetBall")`, we tell the program to access the 'BallControl' class and trigger the `ResetBall()` method. Here's the completed script: [GameManager.cs](https://github.com/ainc/unity-pong/blob/unity5/Assets/GameManager.cs)

Ok cool. Looking at the HUD, we now see there's one new variable under this script that needs filling. It's asking for a 'Skin.' We need to make that in Unity. If you look in your Assets folder, you should see a file that we downloaded that is a special font called '6809 Chargen'.

*NOTE : This font pack is also available for free in the 'Computer Font Pack' off the Unity Asset Store. To get it, simply make a free Unity3D account, and go to the Asset Store. Search for the Computer Font Pack, and download it. You can also go into Unity and open up the Asset Store from inside Unity. There, you can download the fonts you want directly into Unity for your use. This font isn't essential to use, but I'll explain how to use it, because it gives the text the right 'Pong' look.*

In your Project pane, <kbd>right click</kbd> and create a GUI Skin called 'ScoreSkin'. Click on that Skin, and you should see a variable field called 'Font' at the top of the Inspector pane. <kbd>Click + drag</kbd> our font to that variable slot.

![Score Skin with font applied](/img/tutorials/unity-pong/score_skin.png)

If you scroll down and look under the dropdown menus for 'Label' and 'Button' you can also change the size of your text, etc. Play around with size until it looks good. I set Font Sizes for my Label and Button to 42 and 20, respectively. In the end, my HUD's Game Manager (Script) looks like this:

![Game Manager with skin applied to layout](/img/tutorials/unity-pong/game_manager_layout.png)

Awesome! Now you should, when you play, see something like this:

![GUI font and sizes enabled](/img/tutorials/unity-pong/gui_font_enabled.png)

Cool. Now let's make sure that the game knows when we do score. To do that, we need to add a script to the 'LeftWall' and 'RightWall' objects under the HUD dropdown. It's the same script so it should be pretty easy to do. First, we'll go to 'Add Component' on the LeftWall, and name this new script 'SideWalls.cs'.

### Code Breakdown

After we import our packages, we just need to write one function. This function detects when something is colliding with our left or right walls. If it's the ball, we call the score method in GameManager, and reset the ball to the middle. That's about it really. Huh. That was easy.

```csharp
using UnityEngine;
using System.Collections;

public class SideWalls : MonoBehaviour {

    void OnTriggerEnter2D (Collider2D hitInfo) {
        if (hitInfo.name == "Ball")
        {
            string wallName = transform.name;
            GameManager.Score(wallName);
            hitInfo.gameObject.SendMessage("RestartGame", 1.0f, SendMessageOptions.RequireReceiver);
        }
    }
}
```

You already added the script to 'LeftWall', and now, since it's written, go to 'Add Component' on 'RightWall' and choose just 'Script' instead of 'New Script.' Choose the Script we just wrote. Here's the completed script: [SideWalls.cs](https://github.com/ainc/unity-pong/blob/unity5/Assets/SideWalls.cs)

Now, in order for Unity to call our `OnTriggerEnter2D` method, we have to make sure both the LeftWall and RightWall have the "Is Trigger" checkbox selected on their Box Colliders in the Inspector pane. This means that Unity won't treat these walls as physical walls, but rather they "trigger" something in the game (in this case, they give a player a point).

![Is Trigger checkbox](/img/tutorials/unity-pong/istrigger.png)

Test your game to make sure both players can score a point. If so, know what that means? We're DONE! Almost.

## Last Step: Make The Game

Now, we only have to make our game playable outside of Unity. To do this, go to File at the top of Unity. Go to 'Build Settings' and then choose 'Mac, PC, Linux Standalone.' This will make an executable (playable) file appear on our desktop. You may need to 'Add Open Scenes', to ensure that Main is included in the build.

![Build Settings](/img/tutorials/unity-pong/build_settings.png)

Now, click on 'Player Settings.' This is where you should put your name on the Project, choose an icon (I chose the Ball sprite), and uncheck 'Default Is Full Screen'. It asks for a default screen width and height. I chose 960x540 but it really doesn't matter too much. The important thing is in the 'Supported Aspect Ratios' list. Click the little arrow and uncheck everything except `16:10` and `16:9`. If we choose a different aspect ratio, it's possible we might not see our paddles. Your settings in the end should look like this:

![Player Settings](/img/tutorials/unity-pong/player_settings.png)

Now, hit Build and choose where you want to save the file (I chose the desktop), and name it something (I named it Pong v1.0). Look at your desktop. Is it there? Sweet. If you want to see the completed version, here's the [sample code on GitHub](https://github.com/ainc/unity-pong).

Congratulations, and enjoy your game. Thanks for going through this tutorial, and if you want to learn more about Unity or coding in general, make sure to check out the rest of [Awesome Inc U](http://www.awesomeincu.com/) online.

## THANKS, AND GAME ON

 ![Finished Unity Pong game](/img/tutorials/unity-pong/finished_pong_game.png)


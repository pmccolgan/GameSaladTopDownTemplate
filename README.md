# GameSaladTopDownTemplate
A top down game template for GameSalad; builds up game elements scene by scene with the steps required in each explained here.

This was started as a resource for Coderdojo but not completed as GameSalad expired and we moved on to other tools but it demonstrates some gameplay elements (player control, enemies, obstacles...).  

Its not complete but the project has the elements demoed in individual scenes so they can be deconstructed, my notes are here and may help but become less detailed as steps as they go on.  Does require familiarity with GameSalad (Scenes, Actors, Tags, Instances etc.).

##Scene 1 - Player control

Create a new project, set the platform to whatever you want but in this project it was 720p HD

Create an actor for the player, call this 'Player - player control'

Set the following attributes on the actor:
  * Physics density set to 1 for now (weight will be relative to other objects)
  * Physics friction set to 3 for now (how much the actor slows down on contact)
  * Physics bounciness set to 0 for now (between zero and two, we don't want a bounce)
  * Physics fixed rotation should be true (don't turn)
  * Physics movable should be true
  * Physics collision shape should be circle (to move around things)

Import the image circle.png, this is a simple circle that will match the physics collision shape and indicate which direction the actor is facing (blue arrow)

Drag image circle.png onto actor

Add real attribute PositionThreshold, used in following rule to help 'jitter' getting to position

Create rule Follow Mouse which will move and rotate actor to mouse press position

*This rule could be duplicated to react to touches*

Create a scene with just this actor

Test the scene; player should follow and look at mouse press position

Create a new actor 'Level switch'

Set the following attributes:
  * Color yellow
  * Physics movable false
  * Physics shape rectangle

In actors create new tag for player (as we'll have multiple players) and add the player actor to it

Add rule to level switch on contact with player tag go to next level

Create new scene

Add a player and a level switch actor to the scene

Test scene; scenes should switch when player hits the switch

##Scene 2 - Walls

Create a new tag collidable

Create a new actor for 'Wall'

Add actor Wall to collidable tag

Set the following attributes on Wall:
  * Color grey
  * Physics density 0
  * Physics bounciness 0
  * Physics movable false

Copy the actor 'Player - player control', rename the copy 'Player - wall collide'

Add to new 'Player - wall collide' the behaviour collide (with actor of tag collidable)

Create a new scene

Add a 'Player - wall collide' and a Level switch

Add a wall actor

Test scene; player should hit against wall, slide along it

##Scene 3 - Room

Copy the scene 'Walls', rename the copy 'Room'

In the scene attributes change the background color to light grey

Add a 'Player - wall collide' and a Level switch

Add wall actors, position, rotate and resize to create a room or series of rooms

Test scene; player can navigate scenery

##Scene 4 - Crates

Create a new actor 'crate'

Add 'Crate' to tag collidable

Change Crate's attributes:
  * Color brown
  * Physics density 5

Add collide behaviour, collides with actor with tag collidable

In the wall actor add collide behaviour, collides with actor with tag collidable

Create a new scene

Add 'Player - wall collide', 'Level switch', Wall and Crate actors

*Can't get the crate behaviour right, too slidey, adjusting physics attributes (drag?) is the key*
*Possibly add floor to entire scene (not collidable, always in contact with objects) so friction is constantly applied*

*Crates includes bonus actor 'explodey crate' which has a rule that on contact with the player makes the crate explode* 😊

##Scene 5 - Reset

Moving on from 'explodey create' make some dangerous static objects

Create a new actor 'reset block'

Add a rule to 'reset block' that on contact with tag Player reset the scene

##Scene 6 - Switch

Another interactive object

Create a new actor 'switch'

Similar to 'reset block' but on contact with player change colour from red to green

Create a test scene with the usual elements and a switch

Add a scene attribute 'door', a boolean initially set to false

Edit the switch instance in the level, on contact as well as changing colour set the scene attribute door to true

Add a wall actor to the scene that blocks part of the scene

Edit the instance of the blocking wall, add a rule on the scene door attribute being true that the actor is destroyed

##Scene 7 - Chaser Enemy

An actor similar to Player but always knows where the player is and turns/moves towards them

Requires new tag (Enemy) and a new Player actor (Player Wall Enemy Collide)

##Scene 8 - Vision Chaser Enemy

A variation on the chaser enemy, always knows where the player is but only reacts when they're within a certain range

##Scene 9 - Camera control

This was trickier than expected, move the camera with the player so scenes larger than one screen can be navigated

Requires a new Player actor (Player - wall enemy collide camera)

This actor has a camera control behaviour, it also has a group 'Broadcast Camera Data From Scene'

Putting this actor in a scene will have pink debug text on the player, this displays the mouse y position and is just to remind you to configure the actor for the scene

Configure the actor by constraining the attributes in 'Broadcast Camera Data From Scene' to the scene camera size/origin values

You can also disable the Display Text behaviour

These camera values are used to translate the mouse position in screen space to a position in the scene (see the actor's group 'Calculate Mouse Position')

The follow behaviour has been updated to use this translated mouse position instead of the actual mouse position

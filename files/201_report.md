# BOĞAZİÇİ UNIVERSITY

![boun_logo_yeni](https://github.com/BUIE201-Spring2023/spring2023-projectpart2-group-12/assets/63300500/9d3c86ac-00a7-40ef-a84b-4b5d116c8f6a)


## **"The Spies of the Cold War"**

**IE 201 Project: Part 3**

**Instructor: Ali Tamer Ünal**

**30.05.2023**

**Spring 2023**

**Group 12:** 

Mete Dibi 2017402135

Buse Naz Kocali 2020402180

## Table of Contents

**1)Description of the Game**

**2)Class Diagram**

**3)Use Case Diagram**

**4)Collaboration Diagrams**


# **1) Description of the Game**
This is a single player spy game. The player tries to infiltrate the enemy militia as a spy and indoctrinate the opposing militants with her own ideology. The player wins the game if no militants with the opposing ideology (right wing) are left. The player loses the game if she gets compromised by one of the right wing militants, or if she gets assassinated by one of the enemy assassins.

The “militants” are slowly growing masses of right wing ideology. They move around randomly and recruit new citizens to their ranks as they bump them. Each militant has a single mentor militant, and a mentor militant can have multiple mentees. The mentor-mentee relationship symbolizes the sharing of ideologies inbetween militants.

The ever-growing mass of militants are constructed as a “preferential attachment” social network: The most influential mentors are the ones with the most mentees, and these leaders have more probability to be assigned the mentor of a newcomer as they join the pack. So the more links a militant has, the more likely she is to receive new links (power law). Here is a demonstration of the preferential attachment network structure:

<img width="611" alt="Preferential Attachment" src="https://github.com/BUIE201-Spring2023/spring2023-projectpart2-group-12/assets/63300500/27a6bc79-ee5f-4d64-83e6-62910be2e7a6">

At certain periods of the game, a directed diffusion of ideologies takes place from each mentor to its mentees. Note that a single militant can both be a mentee, and also a mentor to others. The size of the nodes above represent the total number of mentees a militant has, symbolizing its influence and rank. The bigger the node, the higher its influence.

The player's aim is to control the spy character to infiltrate the enemy militia and indoctrinate the militants. When the player wants to indoctrinate an opposing militant, she shoots a “doctrine bullet”, changing the ideology of the shot militant by a certain amount. Remember that a diffusion of ideologies takes place in the network at certain periods. So as militants change their ideologies, this effect will be diffused to their mentees, who also will diffuse their views to their mentees, creating a domino effect. The more influential/central a militant is, the more effective the indoctrination - so the message reaches more members. 

The opinions of the militants are scaled on a continuous measure. As the player shoots more doctrine bullets, the ideology of the damaged militants move faster along the spectrum towards the player’s ideology. Once a current branch of the network passes the neutral threshold and embraces the player’s ideology (blue color), the player can move freely among the members of this branch - bumping into one of them would therefore not result in instant loss of the game. However, remember that the diffusion of ideologies happens at certain periods, so a branch of militants will convert back to their original ideology (red color) since they are regularly influenced by their mentors.

While trying to propagate her own ideas among the enemy militants, the player must also avoid the enemy assassins, who constantly chase the player. If the player collides with an assassin, the player dies and the game is lost.

Another threatining aspect for the player is the computer controlled player of the opposing team. This cpu player regularly tracks down the most influential militant (the biggest node) and tries to revert her back to the original ideology by shooting doctrine bullets.

The player must regularly replenish her bullets. To do so, she travels to the warehouse and replenishes ammunition by waiting a certain amount of time in the warehouse. The longer a player waits in a warehouse, the more ammunition she can stock; however, she must bear in mind that time is against her in this game (due to chasing assassins, downward diffusion of right wing mentors and the cpu player) so she must plan her replenishment strategy accordingly.

# **2) Class Diagram**

![class drawio-6](https://github.com/BUIE201-Spring2023/spring2023-projectpart3-group-12/assets/63300500/9f1321c3-ea93-41c9-883d-53b69acd1c35)


### 2.1) Description of the Class Diagram

***Disclaimer**: The word “Sprite” has been used interchangeably to denote the specializations of the Sprite class, which are the main classes in the game: “Warehouse”, “Player”, "CPU_Player",“Link”, “Militant”, “Bullet”, “Citizen”. 

**Game:** We have a main class called Game, which is responsible for the creation and the running of the game. The game class holds the information of all the SpriteGroup instances and also some individual Sprite instances (Player, Warehouse and the CPU_Player which only have a single instance throughout the game). The game frequently sends messages to SpriteGroup objects (which in turn message their own sprites), or to the individual objects themselves (if they are of the Player, Warehouse or CPU_Player classes). The game is also responsible for creating SpriteGroup instances and the 3 aforementioned Sprite instances in the initialization phase (so, composition relation since they can't be present without the game class.). All the main operations regarding the gameplay (start screen, pause screen, main running loop etc.) are contained in the methods of the game. 

**Sprite:** This is the base class of our game for all objects. We have created our own Sprite class, this is not the pygame.sprite.Sprite class. All sprites are created by the game during initialization. When they are created, they are automatically added to their affiliated sprite group. During the run of the game, several other instances of classes have the ability to create sprites (which are shown as “create” on the association lines on the diagram). Note that the information of their group is not stored as an attribute in Sprite’s, they just use the information to add themselves to the group.

Also note that all sprite instances hold the information of the pygame screen for drawing operations. 

MovingObject, Warehouse, Citizen and Link all inherit from the Sprite class.

**BaseGroup:** This is a base class for Sprite Groups. It has basic methods like adding or removing a sprite from the group, or drawing them on screen. It has only 1 instance throughout the game, "all_sprites_group".

**SpriteGroup:** This is the class for all groups of Sprite instances in the game. Generally, when we want to command something to sprites (to check collision for instance), we send a message to their affiliated groups and they handle the operation from there. It has a composition relationship with the Game class: All SpriteGroups’s are created and held by the Game, and they have no presence unless the Game object exists. It also has an aggregation relationship with Bullet, Link, Citizen and MovingObject classes since we hold instances of these classes in their affiliated groups. All sprite groups hold the information of the "all_sprites_group" instance, which is a big, main group that collects all the sprites in the game and draws them on screen.

**MovingObject**: A specialization of Sprite class. We add the ability to move and assign a speed & direction to the object, and base all moving objects to this class. They act as an abstract class (like Sprite class). The only direct instance of this class are instances called "assassin", which are held in their related Sprite Groups.

**Militant:** A specialization of the MovingObject class. It has several undirected-directed association relationships:

*Militant-Citizen:* Can collide (recruitment) (undirected)

*Militant-Bullet:* Can collide (indoctrination) (undirected)

*Militant-Spy:* Can collide (comprimise) (undirected)

*Militant-Link:* A militant instance creates her own Link instance when she gets assigned a new mentee. (directed)

All militant instances aggregate the information of their mentee set, which are also militant instances. So this class has an aggregation relation with itself.


**Militia:** We created a specialization of the SpriteGroup to handle the operations related to militants, since they are subject to some other operations. The additional methods and attributes in this class accounts for that and handles these operations more smoothly. All militant instances' information are held as an attribute in instances of the Militia class, together with their rank attribute(aggregation).Like other SpriteGroup instances, Militia instances are also created and stored by the Game.


**Player:** This is our player, a specialization of the MovingObject. Here are its association relations:

*Militant-Player:* Can collide (compromise) (undirected)

*MovingObject-Player:* Can collide (assassination) (undirected) (note that assassins are direct instances of MovingObject, they dont have any additional methods so they are not given a seperate class)

*Player-Bullet:* The player creates Bullet objects when she shoots. (directed)

*Player-Militant:* The player aims at the militants to shoot them (directed)

Also note that it has an association relation with the bullet group (which is the name of the Sprite Group for bullet instances): The player has the information of that group, so that it can use it as a parameter when creating bullets (so that they will be added to their group).


**CPU_Player:**
This is the opponent player controlled by the computer, which inherits from the Player class. In addition to having the same relations its parent class has, it has a directed association relation with the militia class, since the cpu player holds the information of the militia group. 

**Bullet:** A specialization of MovingObject. These objects are only created when shooting takes place (directed association from Player/CPU_Player to Bullet), and are destroyed if it collides a Militant object (undirected association with Militant). 


**Citizen:** A specialization of Sprite. When a militant bumps to a citizen (undirected association with Militant), the citizen creates a new Militant object at exactly its current spot (directed association with Militant). The game then kills the citizen object(directed association from the Game to the Citizen).


**Link:** A specialization of Sprite. It holds the information of the mentor and the mentee (directed association with Militant).


**Warehouse:** A specialization of Sprite. It increases the number of bullets the player has by calculating the total amount of time passed since the entrance time of the player to the warehouse(directed association with Player).

### 2.2) Cardinality Relations

**Compositions**

Game-SpriteGroup: We initialize the game by creating and holding the information of the main Sprite Group instances, there are 4 of them and this doesn't change(groups for bullets, links, citizens and assassins(moving object instance).)

Game-Player: We initialize the game by creating and holding the information of the main Player instance, which is only 1 and doesn't change.

Game-CPU_Player: We initialize the game by creating and holding the information of the main CPU_Player instance, which is only 1 and doesn't change.

Game-Militia:  We initialize the game by creating and holding the information of the main Militia instance, which is only 1 and doesn't change.

**Aggregations**

BaseGroup-Sprite: The BaseGroup instance, which is the big group that aggregates all sprites, is created at the beginning of the game, and there is only 1 of it throughout the game. The game starts with a total of 109 sprite instances (100 citizens, 3 assassins(moving object instance), 2 militants, 1 link, 1 player, 1 cpu_player, 1 warehouse), and the number of sprites can increase throughout the game.

SpriteGroup-Link: There is a single group for all the links. We start with 1 link, and it increases throughout the game as recruitment takes place and a new militant gets assigned a mentor militant.

SpriteGroup-MovingObject: There is a single group for all the assassins (which are basically moving object instances). We start with 3 assassins, and it stays the same throughout the game.

SpriteGroup-Citizen: There is a single group for all the citizens. We start with 3 assassins, and it decreases throughout the game as citizens are turned to militants.

SpriteGroup-Bullet: There is a single group for all the bullets. We start with 0 bullets, and it can increase throughout the game as shots are fired.

Militia-Militant: There is a single group for all the militants. We start with 2 militants, and it increases throughout the game as recruitment takes place and new militants join.Militant-Militant: Each militant aggregates the set of mentees of itself. 1 militant can have 0 to many mentees, and a mentee can have 1 mentor.

**Directed Associations**

Game-Citizen: The game kills a citizen instance during recruitment, which can happen multiple times during the game.

SpriteGroup-BaseGroup: All groups of sprites hold the information of the big group since they have frequent comminication (_all_sprites). There are 4 sprite groups, and 1 base group throughout the game.

CPU_Player-Militia: The cpu player instance holds the information of the militia group since they have frequent comminication. There is only 1 cpu player, and 1 militia throughout the game.

Player-SpriteGroup: The player instance holds the information of the group of bullets, which is a sprite group instance. There is only 1 bullet group, and 1 player throughout the game.

Warehouse-Player: Warehouse communicates with the player each time it is in the warehouse, and can help the player increase its bullets. There is only 1 warehouse and 1 player.

Player-Bullet: The player creates a bullet whenever it shoots. 1 player can shoot many bullets, and a bullet can only be created by 1 player.

Player-Militant: 1 player can aim at 1 militant. This action may or may not happen (showed as 0..1)

Link-Militant: 1 link can hold the information of 2 militants, but a militant need not be stored in a link(if it has no mentor-mentee relation). It may also be part of many links.

Militant-Link: 1 militant can create many links throughout the game. However, if a link is created, it can only be created by 1 militant.

Citizen-Militant: The citizen can create a single militant before it is killed by the game, symbolizing its turn to a militant instance.

**Undirected Associations**

Player-Militant: The player can collide with a single militant. If a collision occurs, the game ends (compromise).

Bullet-Militant: A bullet can collide with a single militant, but a militant can be shot by many bullets (indoctrination).

Player-MovingObject: The player can collide with an assassin (moving object instance), if such a collision occurs, the game ends (assassination).

Citizen-Militant: A citizen can collide with a single militant, but a militant can collide with many citizens(recruitment).




# **3) Use Case Diagram**


![timetick_part2 drawio-3](https://github.com/BUIE201-Spring2023/spring2023-projectpart3-group-12/assets/63300500/4dc70e0e-cdba-4c0b-8276-a7fbe5e955e9)


The controller of the game can take certain actions with her free will: At first, she faces a start screen. She must click on start button to start the game. Then, during gameplay, she can move the player to 4 directions. She can shoot a bullet to the militants by clicking to where she want to aim at. She can also travel to the warehouse and increase her bullet level. She can also pause the game by pushing on spacebar, she can also quit the game by pushing esc button. In pause screen, she can resume the gameplay by clicking on the resume button.



## **3.1) Pseudo Code of the Main Loop**


*In order to better explain the collaboration diagrams of start, pause, resume and exit in the next sections, we have added a pseudo-code of our main loop.*


**def run():**

>self._game_start = False

>self._start_screen(): ## loop to show the start screen to the player


**def start_screen():**

>display_screen_here 

>while not self._game_start

>>for event in pygame.event 

>>>if event == start:

>>>>self._game_start = True 

>>>>self.run_gameplay()


**def run_gameplay()**

>self._running = True 

>self._game_paused = False 

>while self._running

>>self.diffusion() 

>>self.move() 

>>self.show_average() self.diffusion()

>>. . . . ### other functions of the main loop are listed here

>pygame.quit()


**def check_pause()**

>>if self._game_paused == True 
>>pause_screen()


**def pause_screen()**

>display_screen_here 

>while self.game_paused

>>for event in pygame.event
>>>if event == resume:

>>>>self.game_paused = False


# **4) Collaboration Diagrams**
## **4.1) Start**

![start drawio-2](https://github.com/BUIE201-Spring2023/spring2023-projectpart3-group-12/assets/63300500/26f62335-cb7b-42b2-a561-2e3d2e6b36eb)


Once the computer is inside the start_screen() loop, the user faces the start screen as long as _game_start attribute is False. If the player clicks on the start button, a pygame.event.Event instance is created. Once the next event is the start event, this _game_start value is set true and the computer breaks out of the loop, calling the run_gameplay() method to start the gameplay.

## **4.2) Pause**


![pause drawio-2](https://github.com/BUIE201-Spring2023/spring2023-projectpart3-group-12/assets/63300500/ff58be43-35eb-4283-99b2-a847cec03aef)



During the main loop of the gameplay (run_gameplay()), the game repeatedly calls the handle_events() function in each loop to handle user input. In this diagram, it is assumed that the user click takes place during the iterations of the run_gameplay(). The user can pause the game with pushing the spacebar during gameplay. Regardless of when the key was pressed, Pygame holds the information of this event and adds it to the queue. When handle_events() is called in the next iteration, the game checks whether the event was the push of the spacebar. If the event is the spacebar, the game sets the boolean attribute _game_paused to True. After user input is handled, the game calls the check_pause() function, which calls the pause_screen() function if the _game_paused attribute is True.

## **4.3) Resume**


![resume drawio](https://github.com/BUIE201-Spring2023/spring2023-projectpart3-group-12/assets/63300500/183c4507-c1c7-4351-a0dc-e56fccd7070e)


It is assumed that the game is in the pause_screen loop now, iterating as long as _game_paused attribute is True. This subloop constantly checks user inputs. After the user clicks on the resume button, the event is created. Afterwards, the boolean attribute game_paused is set to False. Therefore, the computer breaks the loop and continues the iteration of the main loop of the game. 

## **4.4) Exit**


![exit drawio-5](https://github.com/BUIE201-Spring2023/spring2023-projectpart3-group-12/assets/63300500/da15e241-15f7-4753-b20e-600351c6bbc8)


  During the main loop of the game (run_gameplay()), handle_events() checks the user inputs as before. If the event next in the line is ESC, the _running attribute is set to False. After that, the computer breaks out of the main loop, and messages pygame to call the quit() function.

## **4.5) Move Up**


![move up drawio-3](https://github.com/BUIE201-Spring2023/spring2023-projectpart3-group-12/assets/63300500/29cad0b9-49aa-4a7d-84a5-46c9dcaeddb2)


An Event instance is created to symbolize the clicking on the W button.

The if-else block in handle_events checks whether the next event in the queue is the pushing of W. If it is, the game sends a message to the player to move 1 step up. It also sets the _moving_up attribute True for itself (to use in the move() method during time tick, to continue moving when the user keeps the buttons down.)

## **4.6) Move Down**

![move down drawio-2](https://github.com/BUIE201-Spring2023/spring2023-projectpart3-group-12/assets/63300500/fd35608b-87f6-49da-820e-c8ed5704a6ca)




An Event instance is created to symbolize the clicking on the S button.

The if-else block in handle_events checks whether the next event in the queue is the pushing of S. If it is, the game sends a message to the player to move 1 step down. It also sets the _moving_down attribute True for itself (to use in the move() method during time tick, to continue moving when the user keeps the buttons down.)

## **4.7) Move Right**


![move right drawio](https://github.com/BUIE201-Spring2023/spring2023-projectpart3-group-12/assets/63300500/106a89e9-f102-4a73-a253-b220e8d1bb38)


An Event instance is created to symbolize the clicking on the D button.

The if-else block in handle_events checks whether the next event in the queue is the pushing of D. If it is, the game sends a message to the player to move 1 step right. It also sets the _moving_right attribute True for itself (to use in the move() method during time tick, to continue moving when the user keeps the buttons down.)

## **4.8) Move Left**


![move left drawio-3](https://github.com/BUIE201-Spring2023/spring2023-projectpart3-group-12/assets/63300500/5c800ec3-86d2-4aa3-b5c6-23ee99a0f1b2)


An Event instance is created to symbolize the clicking on the A button.

The if-else block in handle_events checks whether the next event in the queue is the pushing of A. If it is, the game sends a message to the player to move 1 step left. It also sets the _moving_left attribute True for itself (to use in the move() method during time tick, to continue moving when the user keeps the buttons down.)

## **4.9) Shoot**

![timetick2 drawio-2](https://github.com/BUIE201-Spring2023/spring2023-projectpart3-group-12/assets/63300500/eb126208-ea5f-422c-9df8-734eae1367f6)

During the main loop of the game, if the user clicks on the screen, an event of mouse click is created. When the computer executes the handle events function, it checks whether a mouse click occurred by getting the next event from pygame. If that event is a click, the game sends a message to the player to call the shoot method, passing down the coordinates of the click as arguments. The player then creates a bullet which originates from her location and is targeted towards the mouse clicks location. As we described before, the bullets_group information is also provided to the new bullet and it then adds itself to its group, which then adds the bullet to the group of all sprites.


## **4.10) Replenish Ammo**

![warehouse drawio](https://github.com/BUIE201-Spring2023/spring2023-projectpart3-group-12/assets/63300500/d9ac9a47-d0b6-4256-9166-7c8c61e51ae5)

During gameplay, the player can travel to the warehouse if she starts running low on bullets. This interaction is checked in the check_all_collisions function of the main loop, during the handling of the warehouse & player operation. If the checking mechanism (explained in very detail in the next sections) returns false, which means the player is not currently on the warehouse, all stats of the warehouse are reset. These stats are used to calculate how much the player will gain bullets the longer she stays in the warehouse. If the collision bool returns true, the stats keep accumulating via the supply function. If the current wait length of the player on the warehouse is larger than a certain threshold, she gains a new bullet.

## **4.11) TimeTick**

***Please see the attached file in the repository for all diagrams (The resolution decreases when it is displayed on the page).***

## **Time Tick Part 1**

![timetick-6 drawio](https://github.com/BUIE201-Spring2023/spring2023-projectpart3-group-12/assets/63300500/3a29515b-951c-413a-a09c-77267793234a)

### **Diffusion**
At certain periods, a downward diffusion of ideologies from mentors to mentees takes place.

**1.1:** The game sends a message to itself to start the diffusion process.

**1.1.1:** The game sends a message to the militia group to activate the diffusion process for all the militants it contains in the group.

**1.1.1.1:** The Militia group iterates through all its militants and sends them a message to diffuse their ideology down to their mentee's (if they have one).

**1.1.1.1.1:** The mentors call the get_diffused method of their mentees by passing down their own ideology to them, allowing them to set their ideology accordingly.

### **Move**
All moving objects have continuous movement after their direction has been set. This continuity of the movements are handled by the game according to each object's assigned direction.

**1.2:** The game sends a message to itself to start the move process.

**1.2.1:** The game sends a message to the player to keep moving in the direction of its True bool values.

**1.2.1.(1,2,3,4):** The player keeps moving in the direction of the True bool values.


**1.2.2:** The game sends a message to the cpu player to move and doctrinate its militants. 

**1.2.2.1:** The cpu player sends a message to the militia group to search for the militant with the most influence/rank (biggest mentor). 

**1.2.2.1.1:** The militia group sends a message to the biggest mentor to pass down its location information to the cpu player, giving it also the information of who cpu_player is.

**1.2.2.1.1.1:** The biggest mentor sets the direction of the cpu player towards itself, passing down its location information.

**1.2.2.2:** The cpu player checks whether the current distance between itself and the biggest mentor is below the shooting threshold. Sets stopping bool value true if it is.

**1.2.2.3:** The cpu player calls the function to reset his direction and start shooting if the bool value of stop is true.

**1.2.2.3.1:** The cpu player calls the shoot method of itself.

**1.2.2.3.1.1:** A new bullet instance is created, originating from the location of the cpu player and headed towards the location of the biggest mentor. The group of the bullet is also provided to the bullet, so that it can add itself when created.

**1.2.2.3.1.1.1:** The bullet adds itself to its own group.

**1.2.2.3.1.1.1.1:** The bullets_group adds the bullet to the group of all sprites.

**1.2.2.4:** Even if its stopped and shooting, the cpu player asks the information of the biggest mentor to the militia group. So, if the biggest mentor changes when its currently shooting, ie. a mentor with a bigger rank is present, it updates that information.

**1.2.2.4.1:** The militia group sends a message to the biggest mentor to pass down its location information to the cpu player, giving it also the information of who cpu_player is.

**1.2.2.4.1.1:** The biggest mentor sets the direction of the cpu player towards itself, passing down its location information.

**1.2.2.5:** If the stopping bool is False, which means the cpu player is far away from the current biggest mentor, it moves towards the biggest mentor.




**1.2.3:** The game sends a message to the militia group to start the movement process of militants.

**1.2.3.1:** The militia group activates the move method for every militant contained in the group.


**1.2.4:** The game sends a message to the assassins group to start the movement process of the assassins towards the player, passing down the information of the player as an argument.

**1.2.4.1:** The assassins group sends a message to the player to start the chase process toward itself for every assassin in the group.

**1.2.4.1.1:** The player provides the information of its location to each one of the assassins by calling their set_direction methods.

**1.2.4.2:** After all assassins' direction has been set, the assassins group sends a message to all of them to move towards their target location.


**1.2.5:** The game sends a message to the bullets group to move all the bullets.

**1.2.5.1:** The bullets group calls the move method of all the bullets in contains.

### **1.3** Since checking collisions is a vital element of the game, it had many messages and methods. Therefore, it would be hard to present it here in this diagram, so we put 3 additional diagrams in the following section (TimeTick Part 2) that clearly explains our checking collision mechanism.


### **Checking Collisions Between 2 SpriteGroups**

First, lets explain our main collision checking method between 2 groups (Group 1 & 2)

![timetick drawio-3](https://github.com/BUIE201-Spring2023/spring2023-projectpart3-group-12/assets/63300500/edb9e0f9-1f2d-411d-802c-1c079d837ab7)

**1:** The game sends a message to Group 1 to ask Group 2 for collisions. Naturally, it passes down the information of Group 2. This function will always return a dictionary (if it exists) of colliders in the end: Each key is a collided object from  Group 2, and the values of each key are the sprites from Group 1 that the aforementioned sprites collided with.

**1.1.** After getting the information of Group 2, Group 1 sends a message to Group 2 to check collisions between sprites of both groups, providing it with its own sprites also.

**1.1.1** After getting the sprites of Group 1, Group 2 performs a nested loop iteration for all the sprites from both groups. At each step of the iteration, it asks its own sprite (sprite2) to check collision with the sprite from Group 1 (sprite1).

**1.1.1.1** After getting the information of sprite1, sprite2 sends its location and radius information to sprite1 by calling its collision checking method, which returns a boolean.

**1.1.1.1.1** The aforementioned function returns True if the distance between 2 sprites are bigger than the sum of their radii.

**1.1.2** The collided sprites from both groups are added to the dictionary.


### **Checking Collisions Between a SpriteGroup and an Individual Sprite**

Now that we explained the main mechanism we use for checking collisions between different groups, lets proceed with checking collisions between a certain group (Group 1) and a single instance (Sprite 2). The process is very similar thanks to the flexibilitiy of did_your_group_collide_with() function: It returns a dictionary in both cases, whether the input is a SpriteGroup or a Sprite.

![group_ind drawio](https://github.com/BUIE201-Spring2023/spring2023-projectpart3-group-12/assets/63300500/18d5c4fb-4567-4d00-b632-0b1764bc62a7)


**1:** The game sends a message to Group 1 to ask Sprite 2 for collisions. Naturally, it passes down the information of Sprite 2. This function will return a dictionary (if it exists) of colliders in the end: Each key is a collided object from  Group 1, and the values of each key is Sprite 2.

**1.1.** After getting the information of Sprite 2, Group 1 starts to process of checking collisions with the sprites it contains.

**1.1.1** After getting the information of Sprite 2, Group 1 asks each on of its sprites to check collisions with sprite2.

**1.1.1.1** After getting the information of sprite2, sprite1 sends its location and radius information to sprite2 by calling its collision checking method, which returns a boolean.

**1.1.1.1.1** The aforementioned function returns True if the distance between 2 sprites are bigger than the sum of their radii.

**1.1.2,3** The collided sprites are added to the dictionary.


## **TimeTick Part 2**

Finally, we may proceed with the analysis of the time tick with 1.3, check_all_collisions(). This function will seperately evaluate all possible collisions and take actions accordingly.

![timetick2 drawio](https://github.com/BUIE201-Spring2023/spring2023-projectpart3-group-12/assets/63300500/7873c1b5-cf0b-4888-b361-0fe46b5623c2)


**1.3** The game sends a message to itself to start the collusion check process for all groups.

**1.3.1** First, the collision between bullets and militants are checked.

**1.3.1.1** The game starts the main check collision process by sending a message to the bullets group, which returns the dictionary (if it exists) of collisions after operations explained in "Checking Collisions Between 2 SpriteGroups". The keys are the damaged militants, and the values are the bullets that shot them.

**1.3.1.2** The game sends a message to all bullets(values) to damage the militant(their respective keys).

**1.3.1.2.1** The bullets call the function of the damaged militants to change their ideology, passing down their damage values.

**1.3.1.2.2** The bullet must disappear after hitting a militant. Therefore, it calls the remove method of itself.

**1.3.1.2.2.1** The bullet sends a message to its group to remove itself from the sprites set.

**1.3.1.2.2.1.1** The bullets group sends a message to the all sprites group to remove the bullet from its sprites set.

**1.3.2** Next, the collision between player and militants are checked.

**1.3.2.1** The game starts the main check collision process by sending a message to the militia group, which returns the dictionary (if it exists) of collisions after operations explained in "Checking Collisions Between a SpriteGroup and an Individual Sprite". The key is the militant the player collided with(or multiple keys if she collided with a multiple of militants at the same time), and the value is the player.

**1.3.2.2** The game evaluates the dictionary and sends a message to all keys (militants) to check whether they are of the same ideology or not. If they are of opposing ideology, it means that they are currently enemies of our player and the player has been comprimised when they collide with it.

**1.3.2.3** If there is any collision with the opposing ideology, the game sets the running bool to false & game ends by breaking out of the main loop.

**1.3.3** Next, the collision between player and assassins are checked.

**1.3.3.1** The game starts the main check collision process by sending a message to the assassins group, which returns the dictionary (if it exists) of collisions after operations explained in "Checking Collisions Between a SpriteGroup and an Individual Sprite". The key is the assassin the player collided with(or multiple keys if she collided with a multiple of assassins at the same time), and the value is the player.

**1.3.3.2** Naturally, if such a dictionary exists, the running bool is set to false, meaning an assassin has catched the player and the game should end by breakingg out of the main loop.

**1.3.4** Next, collisions between militants and citizens are checked.

**1.3.4.1** The game starts the main check collision process by sending a message to the citizens group, which returns the dictionary (if it exists) of collisions after operations explained in "Checking Collisions Between 2 SpriteGroups". The keys are the militants, and the values are the citizens they collided with.

**1.3.4.2** For every citizen(the values) that collided with a militant(its key), the game calls their function to turn themselves to militants, also passing down the militia group(so that created militants will be added) as an argument.

**1.3.4.2.1** The recruited citizen creates a new militant at its position, and returns it to the game.

**1.3.4.2.1.1** The militant adds itself to the group upon creation.

**1.3.4.2.1.1.1** The militia group adds the newly created militant to the all sprites group.

**1.3.4.3** The game sends the new militant to the militia group and starts the recruitment process. It also sends the information of the links group since a new link will be created when the new militant connects with its mentor.

**1.3.4.3.1** The militia group finds a new mentor for the new militant, according to the preferential attachment algorithm.

**1.3.4.3.2** The militia group sends a message to the new mentor to add a link between itself and the new militant, also passing down the links group information it got from the game.

**1.3.4.3.2.1** The mentor creates a new link between itself and the new mentee.

**1.3.4.3.2.1.1** The link adds itself to its group upon creation.

**1.3.4.3.2.1.1.1.** The links group adds the newly created link to the all sprites group.

**1.3.4.3.2.2** After building a connection with its mentee and increasing its rank, the mentor sends a message to the militia group to update its rank information.

**1.3.4.4** After recruitment ends, the citizen should be removed.

**1.3.4.4.1** The citizen sends a message to its group to remove itself from its sprites set.

**1.3.4.4.1.1** The citizens group sends a message to the all sprites group to remove the citizen from its sprites set.


## **TimeTick Part 3**

We now proceed with the rest of the time tick use case.

![timetick-6 drawio](https://github.com/BUIE201-Spring2023/spring2023-projectpart3-group-12/assets/63300500/f4d09495-bf7a-466a-8a05-0dddc35ab94b)


### **Updating Militants**

At each step, the militants should update their sizes according to their ranks and also change color according to their current ideology levels.

**1.4** The game sends a message to itself to start the size and color update process of the militants.

**1.4.1** The game sends a message to the militia group to update the size and color of its militants.

**1.4.1.1** The militia group sends a message to each one of its militants to update their radius and color.

### **Clearing the Screen**

**1.5** The screen is cleared for upcoming new drawings.

### **Drawing All Objects on Screen**

**1.6** The game sends a message to itself to start the draw process of all the sprites on the screen.

**1.6.1** The game sends a message to the all sprites group to draw all the sprites it contains on the screen, using their image and rect attributes.

### **Displaying the Player's Ammo Level**

**1.7** Next, current ammunition level of the player must be shown.

**1.7.1** To achieve this, the game sends a message to the player to show its ammunition by displaying it on the top right corner of the screen.

### **Check for Pause**

**1.8** Whether the pause button was pushed is checked regularly(explained in player use cases section)

### **Handle User Input**

**1.9** User inputs are checked regularly(explained in player use cases section)

### **Advance Time & Update Display**

**1.10** The clock ticks and display is updated.


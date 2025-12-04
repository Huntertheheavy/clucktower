# One Knight at The Clucktower | Development Commentary

Video Demonstration: https://youtu.be/SJIv2dIWSDU

Itch.io Page: https://hunters-games.itch.io/one-knight-at-the-clucktower

Repository Link: https://github.com/Huntertheheavy/clucktower

Programming Documentation: https://github.com/Huntertheheavy/clucktower/blob/main/Documentation.MD

## Abstract

***One Knight at the Clucktower*** is a first-person point-and-click sit-and-survive horror game developed in Unreal Engine 5.6.1. The core concept is that you play as a knight on duty as a night watchman, who defends themself against the supernatural that comes out during the night. The gameplay plays by creating magical force-fields and using a magical book to track the location of the monster, but they come at the cost of loosing mana. So it becomes a balance of using the book cameras to gather intel on the enemy, and creating force-fields to protect yourself, all while making sure you do not waste power. The game has a low-polygon style, and a low colour density and pixel count filter to be reminiscent of older 3D games. In terms of the technical side of the project, the goals needed to be set for a project such as this are the following: doors to block threats, a camera system, a power/mana system that activates when cameras and doors are used, a player that cannot move but can only look left and right between two angles being the maximum rotation, an enemy AI system that teleports to rooms based on its current room, a timer, and aggression level, and finally, some shaders to sell the low-resolution and low-polygon style, on top of shaders for the magical book, a win condition that then nets the player. The goals were derived from the project requirements, which are the following: moving or changing actors with exposed variables that can be edited in the editor, AI and a behaviour tree with a blackboard and a behaviour tree using nav meshes for pathfinding and navigation, an event dispatcher or an interface, use of arrays or collection to store multiple values, and enhancements such as particles, post-processing or audio.

## Research

### Game Inspiration

Before progress on the game even began, a clear idea of the game was imagined: A horror game that plays like ***Five Nights at Freddy's*** (Cawthon, 2014), typically shorthanded to '*FNAF*' but has a different theming than a run-down restaurant. So what would be pivotal for this project is to study how *Five Nights at Freddy's* works on a technical level, and also on a thematic level on how the game itself hooks the players into playing it. 

Instead of buying Clickteam Fusion and manually decompiling the game, multiple sources were consulted to figure out how the game works functionally and how to improve the scare factor.

- ***Ruining FNaF by Dissecting the Animatronics' AI | Tech Rules by Tech Rules*** talks about the AI system on how the threats,  referred to animatronics in-game, move and attack in the game.

    What key takeaways were taken from the *Ruining FNaF by Dissecting the Animatronics' AI | Tech Rules*  that are relevant to the project?
    
    - Animatronics each have a timer with some variance to desync.
    - Animatronics each have an AI 'Aggression' level between 1-20 which is compared to a randomly generated number between 1-20 to determine if movement is possible or not.
    
    
    ![Alt text](./mdImages/CustomNight.png "FNAF  Custom Night")
    (Cawthon, 2014)  
    [ Figure 1. *Five Nights at Freddy's* Custom Night menu where you can manually alter the AI aggression level of each animatronic.] 

- ***FNAF 1: How does Power Consumption work? By KawiKaze on r/technicalFNaF*** is a forum post on a *FNAF*-themed subreddit talking about how the power-drain system works in the first game. The replies all have varying information on how the power system works, but in the varied responses, many of the points brought up were the same, meaning there is most likely some credence to the forum post, despite its unreliable nature. This forum was moreso a guideline on how to balance the forcefields and book usage in this project rather than something to directly take information from.

    What key takeaways were taken from the *Ruining FNaF by Dissecting the Animatronics' AI | Tech Rules*  that are relevant to the project?
    
    - Power in FNAF is actually an integer that ticks down rather than a float.
    - The power shown on the HUD of FNAF is simply a truncated percentage.
    - The power drained naturally in-game is smaller and variable depending on the night compared to camera and door usage, which is larger, and a constant throughout every night of the game.
    
- ***Fashioning the Immersive Fallacy at Five Nights at Freddy’s: A New Approach to Music, Sound, and Their Relationship to the Immersive Process in Moving Image Media*** is a scholarly article on how audio is utilized throughout the FNAF franchise to produce atmosphere and enhance the fear in the game. However, the audio is not just for making the game scary, but also enhances the gameplay. The franchise uses audio cues frequently (Capstick, 2021). For example; the first game has an animatronic that runs towards the player instead of slowly going room to room. Once the animatronic fails an attack, you can hear banging on the door, thus meaning it is safe to open the door. (Scott Cawthon)

    What key takeaways were taken from the *Ruining FNaF by Dissecting the Animatronics' AI | Tech Rules*  that are relevant to the project?
    
    - Clever use of audio is key to making the game scary and enhancing the atmosphere of the game.
    - Clever use of audio is key to making the game's mechanics more sound.   

### Learning Unreal Engine and Its Blueprints System

Before creating a game in the Unreal Engine, one must know how to use said engine. Throughout the making of the game, several video tutorials on how to do specific things in Unreal that are relevant to the project were used. Due to Unreal's complex Blueprints system, it is quite jarring to get a grasp on how to utilize its functionality effectively when transitioning from code library-based game engines with no UI. 

#### Sources

- ***How To Clamp The Camera Rotation | How To Lock The Camera View - Unreal Engine 4/5 Tutorial by Matt Aspland*** was key to learning how to clamp the camera for the main gameplay sections, to disallow the player from looking up and down, and looking too far left or right.

    What key takeaways were taken from the *How To Clamp The Camera Rotation* tutorial that are relevant to the project?
    
    - How to clamp the camera's roll, yaw and pitch in Blueprints.
    
- ***Unreal Engine 5 Retro Shader Tutorial | Pixelation & Color Posterization Post Processing Material by Pixel Perfect*** was key to give the game a 'retro' filter reminiscent of older 3D consoles. This was chosen to fulfill the requirement of some sort of post-processing within the game.

    What key takeaways were taken from the *Unreal Engine 5 Retro Shader Tutorial* that are relevant to the project?
    
    - How to edit materials.
    - How to create shaders in general.
    - How to create a shader to replicate old 3D graphics like the N64 or Playstation.

- ***How To Create Cel-Shading In Unreal Engine 4/5 (Tutorial) by Matt Aspland*** helped creating the shader for the cameras on the magic book, as since its a book, it diegetically makes sense to give it a hand drawn look. Unfortunately, this tutorial is either outdated or was implemented wrong, but it still produced a passable effect for the camera, so it's logic was kept in the project.

    What key takeaways were taken from the *How To Create Cel-Shading In Unreal Engine 4/5 (Tutorial)* that are relevant to the project?
    
    - Not all tutorials, even for Unreal Engine 5, will work.

- ***Black And White or Old Colour Post Process - Unreal Engine Tutorial by Matt Aspland*** was also looked up to implement a sepia tone to the book cameras. Since the game has a medieval theming, and the camera is a book, having a yellow and brown filter on the cameras is fitting as it would be close in colour to how paper looked back then.  

    What key takeaways were taken from the *Black And White or Old Colour Post Process* tutorial that are relevant to the project?
    
    - How to use the built in post-processing settings for colour grading.
    
- ***Security Camera | UE5 Tutorial by Patsanoob*** was key in the project, as it was used to create the systems utilized for the book cameras, one of the main mechanics of the game. This was key for the game, as other methods for the game would've looked quite awkward and less diegetic to the game itself. The tutorial produces a flip-up effect when opening the cameras, and in the case of this game, makes sense considering the cameras are justified as a physical book.

    What key takeaways were taken from the *Security Camera | UE5 Tutorial*  that are relevant to the project?
    
    - How to apply widget to actors.
    - How to apply separate actors as children to a parent actors.
    - How to use render textures materials.

- ***How to Use a Custom Font in Unreal Engine 5 by Gorka Games*** was used briefly during the implementation of the cameras and main HUD as trying to figure out how to import fonts was complicated with the built in import function, but the tutorial made it clear that importing assets is as easy as dragging them in to the content drawer.

    What key takeaways were taken from the *How to Use a Custom Font in Unreal Engine 5* tutorial that are relevant to the project?
    
    - Importing fonts is possible with Blueprints.
    - Importing assets in general can just be done with dragging instead of manually searching the file explorer.
    
- ***How To Create A Main Menu - Unreal Engine 5 Tutorial by Unreal University*** was used to ensure the game has a layer of polish. Instead of text appearing on screen to signify if the game has been won or lost, the use of entire menus not only better illustrates this information, but also allows for a title screen, and a controls screen to be made.

    What key takeaways were taken from the *How To Create A Main Menu - Unreal Engine 5 Tutorial* that are relevant to the project?
        
    - How to create different levels in a game project.
    - How said levels can be used to facilitate menus.
    - How to add images to widgets.
    - How to load in to levels.


- ***UE 5 TUTORIAL - How to pass DATA (Variables) from LEVEL to LEVEL using GAME INSTANCE. by Bartosz Kaminski*** was used to keep data saved between switching levels. This was to ensure the game has a collectible, as if the player wins the game, they get a star on the main menu. So game instances needed to be used to track the variable to do this.   

    What key takeaways were taken from the *UE 5 TUTORIAL - How to pass DATA (Variables) from LEVEL to LEVEL using GAME INSTANCE* that are relevant to the project?

    - Game instances can be used to effectively pass data between levels. 
    
## Implementation

The beginning of the project started with selecting one of the pre-made unreal templates for a first-person player controller. Considering this game is based off a first-person point-and-click game, a preset made for shooters, which are arguably a form of point-and-click game, is most fitting for this project. Each section is shown chronologically, as each feature can be described as its own 'phase' in the development process.

### Clamping Player Camera

What made the most sense to begin the project is to remove all of the player controls in the first-person template, and clamp the camera to not be able to adjust pitch with the mouse, and to only allow the player to look two angles rather than full 360 degree rotation. Realistically, the game could be designed with 360 degree movement, but to keep things simple, the game is designed much more closer to how *Five Night's at Freddy's* functions.

![Alt text](./mdImages/cameraBounds.png "camera clamping blueprint code")
[ Figure 2. Blueprint code in `BP_FirstPersonCharacter` that sets the maximum and minimum pitch and yaw.] 

With said code, here is the camera clamping in action:

![Alt text](./mdImages/cameraBounds.gif "camera clamping in action")
[ Figure 3. Camera Rotation in action.] 


### Buttons and doors
    
At the point of the game's development, the theming's justification of forcefields being the doors was not considered, so a visible button like *Five Nights at Freddy's* was opted for. The logic used to check, involves checking the angle that the player is looking at is valid for controlling the doors, ergo left and right, if the buttons are within a sphere trace, if said buttons have the tag left or right, and if the player is interacting with a door, ignore the one of the other side. This is then uses an interface which then triggers the `button_A` logic, which acts as the toggle for the doors being open and closed. And the door logic handles the physical movement of `door_A` instances.

![Alt text](./mdImages/checkDoorTag.png "blueprint macro to check which button is being interacted with")
[ Figure 4. Blueprint macro to check which button is being interacted with in `BP_FirstPersonCharacter`.] 

![Alt text](./mdImages/doorControlsInPlayer.png "Blueprint code for door controls")
[ Figure 5. Blueprint code for door controls in `BP_FirstPersonCharacter`.]

![Alt text](./mdImages/doorControlsInButton.png "Blueprint code for door controls for button")
[ Figure 6. Blueprint code for door controls in `button_A`.] 
![Alt text](./mdImages/doorControlsInDoor.png "Blueprint code for door controls for door")
[ Figure 7. Blueprint code for door controls in `door_A`.]

With all of that code working in tandem, it creates the following:

![Alt text](./mdImages/doors.gif "doors in action")
[ Figure 8. Closing and Opening Doors in Action.]

### Asset Implementation

At this point, the theming for the game being a medieval knight fending off against monsters at night was thought of, along with the low-poly art style, so some time was spent sourcing textures, sounds and 3D models and animation.

![Alt text](./mdImages/cabin.png "Cabin")
[ Figure 9. The cabin.]

![Alt text](./mdImages/cabinDoor.png "Cabin Door View")
[ Figure 10. The cabin looking outside.]

![Alt text](./mdImages/forest.png "Forest")
[ Figure 11. The forest throught the book cameras] 

![Alt text](./mdImages/forestUnlit.png "Forest unlit")
[ Figure 12. The forest unlit]

![Alt text](./mdImages/zombie.png "zombie")

[ Figure 13. The zombie]

![Alt text](./mdImages/zombieInGame.png "zombie ingame")

[ Figure 14. The zombie ingame]

### Post-Processing and Shaders

Due to it being a requirement, and it further enhancing the game's visuals, it was decided that the post-processing used for the game will primarily be a retro pixelated-filter which limits colour density.

#### Pixelation 'Retro' Shader

This is the blueprint for `RetroFilter_Mat`, which was made by following a tutorial. What it does is limit colour density and the camera resolution.

![Alt text](./mdImages/retro.png "Retro")

[ Figure 15. Retro Shader]

The following is simply a screenshot of the cabin to illustrate how the pixelated-shader looks

![Alt text](./mdImages/cabin.png "cabin")

[ Figure 16. The cabin with the shader in `PostProcessVolume`]

#### Sepia filter for Cameras

In a post processing volume directly, Unreal has it's own presets for post-processing, which includes colour grading. To replicate the orange-y colour seen on papyrus paper or simply book paper, the tint was turned towards the maximum value, which is grading the colour towards magenta, and the temperature was turned towards the maximum as well, which is grading the colour towards orange. Together, it gives the camera a desaturated beige colour, just like old-paper. 

![Alt text](./mdImages/colorGrading.png "Colour Grading")

[ Figure 17. Colour Grading in `PostProcessVolume2 ]

#### Cel-shading Filter for Cameras

This is the blueprint for `CameraShader_Mat`. It should be noted that the result was not the desired effect, but the unlit colourlessness of the shader was used anyway as it was passable for the project. 

![Alt text](./mdImages/celShading.png "Cel Shading shader")

[ Figure 18. Cel-Shading Shader ]

#### Cel-Shading and Sepia Combined

In practice, the shader and post-process effect produces the following result:

![Alt text](./mdImages/forest.png "Cel Shading shader with colour grading")

[ Figure 19. Cel-Shading Shader and Colour Grading in `PostProcessVolume2`, seen through the cameras]

### The Book Cameras

The book cameras were also made by following a tutorial directly. This is because the desired effect is to have a 'panel' lift up to then view the cameras from. Alternatively, spectator cameras could have been placed around the map and you allow a widget to be called that allows the ability to make the player possess each camera, but having a 'panel', that being a book, flip up feels intuitive as it diegetically makes sense. The following was the result of the tutorial.

![Alt text](./mdImages/bookUMGSetup.png "Book_UMG layout")

[ Figure 20. The designer view of `Book_UMG`]

![Alt text](./mdImages/bookUMGImportCameras.png "Book_UMG ImportCameras")

[ Figure 21. The `ImportCameras` function of `Book_UMG`]

![Alt text](./mdImages/bookUMGImportCameras.png "Book_UMG SwitchCameras")

[ Figure 22. The `SwitchCameras` function of `Book_UMG`]

![Alt text](./mdImages/bookAConstruct.png "Book_A Construction")

[ Figure 23. The `construction script` of `Book_A`]

![Alt text](./mdImages/bookAEventGraph.png "Book_A event graph")

[ Figure 24. The `Event Graph` script of `Book_A`]

![Alt text](./mdImages/bookAActivate.png "Book_A activate default")

[ Figure 25. The `Activate Default` function of `Book_A`]

![Alt text](./mdImages/bookAOn.png "Book_A Cam On")

[ Figure 26. The `CamOn` function of `Book_A`]

![Alt text](./mdImages/bookAOff.png "Book_A Cam Off")

[ Figure 27. The `CamOff` function of `Book_A`]

![Alt text](./mdImages/bookAUpdateCamera.png "Book_A Update Camera")

[ Figure 28. The `UpdateCamera` function of `Book_A`]

![Alt text](./mdImages/playerCamera.png "Player Camera Logic")

[ Figure 29. The camera logic `BP_FirstPersonCharacter`]

![Alt text](./mdImages/roomUMGSetup.png "Room_UMG layout")

[ Figure 30. The designer view of `room_UMG`]

![Alt text](./mdImages/roomUMGEventGraph.png "Room_UMG Event Graph")

[ Figure 31. The `Event Graph` script of `room_UMG`]

![Alt text](./mdImages/roomUMGSelectCamera.png "Room_UMG SelectCamera")

[ Figure 32. The `selectCamera` function of `room_UMG`]

![Alt text](./mdImages/roomUMGDeSelectCamera.png "Room_UMG deSelectCamera")

[ Figure 33. The `deselectCamera` function of `room_UMG`]

![Alt text](./mdImages/renderTexture.png "Render Texture")

[ Figure 34. The blueprints of `Camera_Material`]

![Alt text](./mdImages/bookFlip.gif "book in action")

[ Figure 35. Book Cameras in action]

### Mana, Timer and Main UI

Now with the doors and cameras finished, the mana, or power, system was developed to punish the player using the book to gather intel on the zombie too much and for permanently closing the door. And since it is correlated to UI as well, the timer system was developed as well. Both systems then output a final variable that is updated on the main widget

![Alt text](./mdImages/powerdrain.png "Power Drain Logic")

[ Figure 36. `powerDrainLogic` function in `BP_FirstPersonCharacter`]

![Alt text](./mdImages/powerdraintick.png "Power Drain Logic Trigger per tick")

[ Figure 37. `powerDrainLogic` being called per tick in `BP_FirstPersonCharacter`]

![Alt text](./mdImages/updateTime.png "updateTime")

[ Figure 38. `updateTime` macro in `BP_FirstPersonCharacter`]

![Alt text](./mdImages/gameTimer.png "gameTimer")

[ Figure 39. the timer ticked down each second with `updateTime` being called after in `BP_FirstPersonCharacter`]

![Alt text](./mdImages/mainUI.png "MainUI_UMG")

[ Figure 40. The designer view of `MainUI_UMG`]

![Alt text](./mdImages/mainUIGetText.png "MainUI_UMG Get Text")

[ Figure 41. The `GetText` function of `MainUI_UMG`]

![Alt text](./mdImages/mainUIGetText0.png "MainUI_UMG Get Text_0")

[ Figure 42. The `GetText_0` function of `MainUI_UMG`]

### Enemy AI

#### Concepting

Due to the complexity of AI, it is best to concept the AI algorithms with diagrams and flow charts before putting it together, even if the AI is based of *Five Nights at Freddy's*, as a flow chart and visual diagrams would serve as a good guide on how to actually program the AI instead of just going into the AI controller with just a cursory understand on how to achieve what is necessary. Below is a flowchart and a diagram of the AI 

![Alt text](./mdImages/figma.png "AI Flowchart")

[ Figure 43. Flowchart for AI]

![Alt text](./mdImages/legalMoves.png "AI Legal Moves")

[ Figure 44. Diagram for rooms and legal moves]

#### The Blueprints

From the two images that state the logic and rules the AI algorithm must follow, the following is the programming required to make this AI controller work:

![Alt text](./mdImages/blackboard.png "blackboard")

[ Figure 45. Keys in `BullyAI_Blackboard]

![Alt text](./mdImages/behaviourTree.png "behaviourTree")

[ Figure 46. The behaviour tree of `BullyAI`]

![Alt text](./mdImages/checkroomEventGraph.png "Checkroom Event Graph")

[ Figure 47. The `Event Graph` of `BullyAI_CheckRoom_Task`]

![Alt text](./mdImages/checkroomroomcheck.png "Checkroom roomCheck")

[ Figure 48. The `roomCheck` function of `BullyAI_CheckRoom_Task`]

![Alt text](./mdImages/checkroomroomcheckzoomed.png "Checkroom roomCheckm zoomed in")

[ Figure 49. The `roomCheck` function of `BullyAI_CheckRoom_Task` zoomed in to the logic of one of the rooms as the logic is entirely the same, just the values different for each room] 

![Alt text](./mdImages/AiController.png "BullyAI_Controller")

[ Figure 50. The `Event Graph` of `BullyAI_Controller`] 

![Alt text](./mdImages/BullyAI_MoveAndAttack_Task.png "BullyAI_MoveAndAttack_Task Event Graph")

[ Figure 50. The `Event Graph` of `BullyAI_MoveAndAttack_Task`]  

![Alt text](./mdImages/BullyAI_MoveAndAttack_TaskMacro.png "BullyAI_MoveAndAttack_Task Macro")

[ Figure 51. The `setEnemyRoom` Macro of `BullyAI_MoveAndAttack_Task`]

![Alt text](./mdImages/BullyAI_MoveAndAttack_TaskMacroZoomed.png "BullyAI_MoveAndAttack_Task Macro zoomed in")

[ Figure 52. The `setEnemyRoom` Macro of `BullyAI_MoveAndAttack_Task` zoomed in to the logic of one of the rooms as the logic is entirely the same, just the values different for each room, for the exception of Room 9, which plays an animation for the jumpscare and loads the next level]

With all of this code put together, the following is the zombie with the AI attacked to it

![Alt text](./mdImages/zombieCameras.gif "Zombie Moving on Cameras")

[ Figure 53. The zombie moving in the book camera view and to the doorways]

![Alt text](./mdImages/jumpscare.gif "Jumpscare")

[ Figure 54. The zombie jumpscaring the player]


### Additional Menus and Separate Levels

After developing the entire game loop, it was only logical now to add menus to give the game polish, clear visuals for winning and losing, and adding a 'collectible' to the title screen to indicate that the player managed to beat the game at least once.

#### Title Screen

The title screen simply tracks a variable in `GameInstance_GI` that checks if the game has won. Upon first loading the game, this variable would be false, but upon reaching 6AM in the game, it updates, and places a star on the title screen.

![Alt text](./mdImages/titlescreenUMG.png "Title Screen UMG")

[ Figure 55. The title screen UMG]

![Alt text](./mdImages/titlescreenUMGEventGraph.png "Title Screen UMG Event Graph")

[ Figure 55. The title screen UMG event graph]

![Alt text](./mdImages/titlescreen.png "Title Screen")

[ Figure 57. The title screen]

![Alt text](./mdImages/titlescreenBlueprint.png "Title Screen Blueprint")

[ Figure 58. The title screen blueprint]

#### Controls Menu 

From the title screen, you can access a controls menu which gives instructions on what to do in the game, including the game's inputs. This is necessary as it ensure play testers know how to control the game without guidance, but also it is a good accessibility feature to display the controls instead of letting the player figure them out.

![Alt text](./mdImages/controlsscreenUMG.png "Controls Screen UMG")

[ Figure 59. The controls screen UMG]

![Alt text](./mdImages/controlsscreenUMGEventGraph.png "Controls Screen UMG Event Graph")

[ Figure 60. The controls screen UMG event graph]

![Alt text](./mdImages/controlsscreen.png "Controls Screen")

[ Figure 61. The controls screen]

![Alt text](./mdImages/controlsscreenBlueprint.png "Controls Screen Blueprint")

[ Figure 62. The controls screen blueprint]

#### Win Screen

After the timer reaches 0, the win screen is loaded from the main level. The player actor used in the win screen is special, as it has specialized code compared to all the other menus. Simply put, when loaded it, aside from just setting the cursor to be visible, the player plays a sound cue, which is the win jingle, known as `win`, in the SFX folder.

![Alt text](./mdImages/winscreenUMG.png "Win Screen UMG")

[ Figure 63. The win screen UMG]

![Alt text](./mdImages/winscreenUMGEventGraph.png "Win Screen UMG Event Graph")

[ Figure 64. The win screen UMG event graph]

![Alt text](./mdImages/winscreen.png "Win Screen")

[ Figure 65. The win screen]

![Alt text](./mdImages/winscreenBlueprint.png "Win Screen Blueprint")

[ Figure 66. The win screen blueprint]

![Alt text](./mdImages/winscreenpLayer.png "Win Screen Player Blueprint")

[ Figure 67. The win screen player blueprint]

#### Lose Screen

![Alt text](./mdImages/losescreenUMG.png "Lose Screen UMG")

[ Figure 68. The lose screen UMG]

![Alt text](./mdImages/losescreenUMGEventGraph.png "Lose Screen UMG Event Graph")

[ Figure 69. The lose screen UMG event graph]

![Alt text](./mdImages/losescreen.png "Lose Screen")

[ Figure 70. The lose screen]

![Alt text](./mdImages/losescreenBlueprint.png "Lose Screen Blueprint")

[ Figure 71. The lose screen blueprint]

## Testing

### Methodology

The methodology used for the playtest was blind playtests. Once the play tester was finished with the playtest, they were tasked to fill out a survey rating the gameplay, physics and level design, and additionally, they were given an option to write out their opinions justifying the rating. This is to ensure data is processed without forcing testers to write out a lot, because some may be demotivated to finish the survey if forced to write.

The goal's wanted to be achieved by testing is to point out flaws in game design, to find technical bugs, and to think of new ideas for the project.

### Statistics

The following are the statistics generated from the survey results:

![Alt text](./mdImages/gameplay.png "gameplay chart")

[ Figure 72. Gameplay ratings.] 

![Alt text](./mdImages/Ai.png "AI chart")

[ Figure 73. Enemy AI ratings]

![Alt text](./mdImages/art.png "art chart")

[ Figure 74. Art Direction and Atmosphere ratings]

Due to the small sample size, it is difficult to make strong conclusions on the graphs with a high level of certainty that the data is accurate. However, with the little data in hand, it's best to draw conclusions from it than ignoring it entirely. 

The general gameplay and atmosphere were generally considered to be the game strongest attributes, where as the AI, which still have somewhat positive reception, was not received just as well as the other attributes, however, from comments given from respondents, it is difficult to actually address the issues with the AI, which can be seen in the next section.

### Observations and Conclusions

- Atmosphere due sound design was good
- Cameras are useless, as you can see the zombie in the hallways
- Backtracks too little
- Backtracks too often

### Addressing the Negative Conclusions

#### Cameras being useless

There is a multitude of ways to make cameras useful within the game. Regardless on the approach, it is necessary to extend the map to ensure Room 5 and 6 are visible through the doorways. 

The following are ways to improve the cameras are as follows, multiple of which can be pursued at a time if they mesh well together:

- Increase mana drain severely, but allow for cameras to be a method to gain mana
- Make it so that the zombie isn't visible in the door way when ready to attack by removing the lights, and instead make Room 7 and 8 visible on the cameras
- Pulling up the cameras stalls the zombie briefly when lifted up
- Create another threat that interacts with the cameras more directly, such as a threat like 'Freddy' or 'Foxy' from *Five Nights At Freddy's (Cawthon, 2014)

#### Regarding AI backtracking

Due to the mixed opinions on the AI, further testing is required to gauge the best solution for improving the enemy's AI. When some say it's too difficult or some say its too easy due to backtracking too often, its hard to pinpoint what is needed to improve the AI. It could be indicative of the Enemy's AI being 'just right', but that is not a conclusion that should be made with little testing.

## Reflection

### Positive Analysis

#### Scare Factor and atmosphere

As multiple play testers have pointed out, the game's atmosphere created by visuals and audio successfully create a good build up of fear, for the jumpscare to be a payoff.

#### Overall Functionality 

The features of the game work as intended with no known bugs aside from the title screen occasionally not showing the zombie. Considering the complexity of the AI, the fact that it was flawless achieved in a functional sense is also a major positive of the project, as it the most complex system in the project, especially since no tutorial was used for the implementation in Unreal, and it was simply based off how it works in FNAF.

#### Polish

The overwhelming use of non-generic Unreal assets adds to the project's polish. On top of that, the game's use of menus, especially the controls menu, and a 'reward' for winning as the player would get a star on the menu makes the game feel like a completed project rather than a vertical slice, even though the project is most definitely a vertical slice due to its simplicity. 

### Research Effectiveness

Due to being completely unaware of how Unreal Engine works, and how in-depth FNAF enemy AI, extensive research on what makes Five Night at Freddy's a good game and how to use Unreal Engine effectively had to be done, and the research done, considering the extensive amounts of tutorials, can definitely be considered successful.

### Negative Analysis

#### Time Management and Rushing

To emphasise why the game could be considered polished for what it is, most of the game was created within the span of two weeks. This is due to a previous project taking precedence to poor planning and time management. This means this game is 'rushed' in a sense, despite its functionality, artistic fidelity, and menus

#### Game Design issues

Due to the simplified nature of the project compared to *Five Nights at Freddy's* or similar games, the cameras technically serve no gameplay function if the player is confident enough at the game, as the player can just look left and right endlessly until the zombie gets to the door and then use the doors. On top of this, depending on the player's monitor's brightness settings, the player can see the zombie on Location/Room 5 and 6 just by looking outside of the door. This can be circumvented by just making the world larger so then Room 5 and 6 are further away from the doors. Since the solutions to these issues are already known, and regarding seeing the zombie through the door; is easy to implement, it is especially a negative that they have not been fixed.

#### Lack of Original Assets

Even though in game development, using pre-made assets is par for the course, it is a personal gripe that the game, outside of `tileStar`, `jumpScare`, and `knock`, uses entirely non-original assets. Due to the lack of time from other projects taking too long, the decision to use pre-made assets was simply a time/cost-cutting measure, but if there was more time to do the project, bespoke assets would've been made for the project to practice 3D modelling, 3D texture sourcing, and 3D animation.

#### Asset Organization

Some assets, such as `Book_UMG` are not organized in the correct folders, that being `all/content/blueprints/UMGs`, because it was originally created in the `/blueprints/` folder but due to adding code to the file, it is not able to be moved into the `/UMGs/` folder due to object references requiring to be manually changed, so to not cause conflict and increase workload, it was opted to just keep it in the `/blueprints/` folder. It should also be noted that most things in the project are written in `camelCase` but some files could not be renamed to camel case due to the same issue with moving the files.

#### Unoriginality

Despite theming, the overall gameplay is not too original. With it playing identically to *Five Night's at Freddy's*, but even simpler, is in no way a positive. The game may have an original art style for the genre, but the core gameplay does not shy away from being inspired.

#### Un-optimization

The button being a feature, due to having no visual visible to the player at all, is a point of poor optimization, as it wastes computing power loading and rendering it despite it being behind a wall at all times. This is due to the game's theming of magic came after the implementation of the doors. If the theming was thought of prior, the code used to interact with the button and making the doors close would have been done directly with the door instead of the button.

#### Poor use of Testing

Due to the little time spent on the project, testing was not as much of a priority as simply making the game exist. As a result, testing was simply done out of necessity rather than as a means to improve the game. The game has not changed much since testing began, aside from the aggression level being set to be lower than 20, and the legal moves per rooms being altered to prevent backtracking of the Zombie, the game has been entirely unchanged. On top of this, the sample size of the testing was quite small, so player trends and common complaints will not be as accurate to the game as not enough information can be drawn to make patterns.

### Next Time

- Allot more time for the project to allow for more time to improve the project and to address the next points
- Plan file organization earlier on into the project
- Plan implementation of certain element more thoroughly before following through with it
- Fix clear-to-spot game design issues 
- Create more original assets for to improve personal skills
- Think of more creative twists on the gameplay
- Make use of testing and feedback more


## Bibliography

Black And White or Old Colour Post Process - Unreal Engine Tutorial (2021) Directed by Matt Aspland. At: https://www.youtube.com/watch?v=DPHxG3PyHPQ (Accessed  03/12/2025).

Capstick, H. (2021) 'Fashioning the Immersive Fallacy at Five Nights at Freddy’s: A New Approach to Music, Sound, and Their Relationship to the Immersive Process in Moving Image Media' In: Sonic Scope At: https://www.sonicscope.org/pub/qulejggo/release/1 (Accessed  03/12/2025).

Cawthon, S. (2014) Five Nights at Freddy’s on Steam. At: https://store.steampowered.com/app/319510/Five_Nights_at_Freddys/ (Accessed  03/12/2025).

How To Clamp The Camera Rotation | How To Lock The Camera View - Unreal Engine 4/5 Tutorial (2022) Directed by Matt Aspland. At: https://www.youtube.com/watch?v=hsBzIQfYZw0 (Accessed  03/12/2025).

How To Create A Main Menu - Unreal Engine 5 Tutorial - YouTube (s.d.) At: https://www.youtube.com/watch?v=kumZj_mov58 (Accessed  03/12/2025).

How To Create Cel-Shading In Unreal Engine 4/5 (Tutorial) (2022) Directed by Matt Aspland. At: https://www.youtube.com/watch?v=YwZH4jCO4ZM (Accessed  03/12/2025).

How to Use a Custom Font in Unreal Engine 5 (2024) Directed by Gorka Games. At: https://www.youtube.com/watch?v=xylUKb4dhxI (Accessed  03/12/2025).

KawiKaze (2024) FNAF 1: How does Power Consumption work?. [Reddit Post] At: https://www.reddit.com/r/technicalFNaF/comments/18wz7yn/fnaf_1_how_does_power_consumption_work/ (Accessed  03/12/2025).

Ruining FNaF by Dissecting the Animatronics’ AI | Tech Rules (2020) Directed by Tech Rules. At: https://www.youtube.com/watch?v=ujg0Y5IziiY (Accessed  03/12/2025).

Security Camera | UE5 Tutorial - YouTube (s.d.) At: https://www.youtube.com/watch?v=u3TfEX0dsl8 (Accessed  03/12/2025).

UE 5  TUTORIAL - How to pass DATA (Variables) from LEVEL to LEVEL using GAME INSTANCE. (2023) Directed by Bartosz Kamiński. At: https://www.youtube.com/watch?v=cOm6nDbu5OU (Accessed  03/12/2025).

Unreal Engine 5 Retro Shader Tutorial | Pixelation & Color Posterization Post Processing Material (2024) Directed by Pixel Perfect. At: https://www.youtube.com/watch?v=dUhoCvpDcIw (Accessed  03/12/2025).


## Declared Assets

### Audio Assets

- `doorOpen`, `doorClosed` sourced from ***[Rewind Time](https://freesound.org/people/SilverIllusionist/sounds/831718/)***

- `ambience` sourced from ***[AMBForst-Summer_Nighttime, Gravel Hiking Trail near campsite road, Mammoth Cave National Park, crickets, frogs, distant traffic_QCF_Mammoth Cave KY_Zoom F3 with LCT 440 Pure](https://freesound.org/people/treytatum3/sounds/815311/)***

### Textures and 3D Models

- `brick_small_old`, `grass_dead_01`, `red_gemstones_norm`, `table_door`, `wood_planks_2`, `wood_planks_fancy_03` were sourced from ***[Classic64 Asset Library](https://craigsnedeker.itch.io/classic64-asset-library)***

- `TreeLine` sourced from ***[Green Plants with White Background](https://unsplash.com/photos/green-plants-with-white-background--IZ2sgQKIhM)***

- `fire`, `candleholder`, `body1` were sourced from ***[Low Poly PSX Lantern](https://aycayaman.itch.io/low-poly-psx-lantern)***

- `zombie`, `zombie_Anim_Armature_Attack1`, `zombie_Anim_Armature_Death`, `zombie_Anim_Armature_Idle`, `zombie_Anim_Armature_Walk`, `zombie_Physics Asset`, `zombie_Skeleton`, `corpse1` were sourced from ***[Zombie](https://maximumdamage.itch.io/zombie)***

- `low_wooden_wall`, `wooden_fence_beam` were sourced from ***[PSX Style Walls and Fences](https://valsekamerplant.itch.io/psx-style-walls-fences)***

### Fonts

- `papyrus-font-undertale` sourced from ***[Papyrus Font [UNDERTALE]](https://fontstruct.com/fontstructions/show/1817942/papyrus-text-6-9)*** 

- `Papyrus_Font_Font` sourced from Windows itself

### IDE and Engine

- ***[Unreal Engine](https://www.unrealengine.com/en-US)***

### Project Template

- First Person Game Template from ***[Unreal Engine](https://www.unrealengine.com/en-US)***

### Audio Editing

- ***[Audacity](https://www.audacityteam.org/)*** to edit `jumpScare` and `doorClosed`, and compose `win`
- ***[Fl Studio](https://www.image-line.com/)*** to edit `jumpScare`

### Texture Editing

- ***[Paint.net](https://www.getpaint.net/download.html)*** to edit `table_door`

### Version Control

- Git, hosted on Github

### Flowchart Creation

- ***[Figma](https://www.figma.com/)*** to create Figure. 43

### Footage Used for Figures

- Sourced from play tests

### Building

- Building was done by a friend, ***[HenryNOVA](https://store.steampowered.com/search/?developer=HenryNova)*** due to SDK issues on the computers the game was developed on.
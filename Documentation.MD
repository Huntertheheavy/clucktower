# One Knight at The Clucktower Programming Documentation

Some assets in the game are not mentioned here due to them being from the project template, and are not really relevant to the project.

## Actors

### Body1

Mesh of a lantern. Used in `Lvl_FirstPerson` as the main light source

### book_A 

Actor of the physical book held by the player to look at the external cameras. Made a child of `BP_FirstPersonCharacter`. Used in `Lvl_FirstPerson`

##### UpdateCamera Function

Used to activate the previous camera selected and activate the current camera selected

##### CamOff Function

Sets a parameter `Deactivate` to a value of `0`

##### CamOn Function

Sets a parameter `Deactivate` to a value of `1`

##### Activate Default Function

Activates the default camera and sets view of `SceneCaptureComponent2D` of the cameras as the render target

##### Event Graph

Imports the cameras to the book and calls `Activate Default`

When `CE_SwapCamera` is called, `CamOff`, `UpdateCamera` and `CamOn` are called in sequence

##### Construction Script

Sets the footage of the camera to be the material targeting the render target

### BP_FirstPersonCharacter

Mesh used as the player character in `Lvl_FirstPerson`

##### Event Graph 

`Event BeginPlay` calls `mainUI_UMG`, checks the door tags, and sets camera pitch and yaw rotation bounds

`Event Tick` initiates calls the `powerDrainLogic` function and the `updateTime` macro

`CE_RaiseTablet` raises the book to be seen by the player

`CE_LowerTablet` lowers the book to be unseen by the player

`EnhancedInputAction Show Cameras` calls `CE_RaiseTablet` if `isBookVisible = false` and `CE_LowerTablet` if `isBookVisible = false`

`Left Mouse Button` initiates a sphere trace, and depending what angle the player is looking at, it allows the button to activate the door or disactivate the door, or if the player's angle is not within the bounds, does nothing.

##### determineDirection

Logic to determine whether or not the player is looking at the doors.

#### updateTime

Logic to update the time on the HUD from the actual timer integer

#### checkDoorTag

Gets all actors of the tags 'right' and 'left' from `button_A` and sets `ignoreRight` and `ignoreLeft`

#### powerDrainLogic

Logic to drain power and set the power to a percentage for the mana HUD. By default, `power = 10000`, and per second, `4` is subtracted from `power` . Per every door closed, `10` is subtracted from `power` per second. If `isBookVisible = true`, `10` is subtracted from `power` per second. This means power drains between `4` to `34` per second depending on how many conditions are true. `power` is then divided by `100` and displayed on the HUD

### button_A

Actor used to handle door toggle. Used in `Lvl_FirstPerson`

##### Event Graph 

When `Event InteractFunc_1` is called, check if `isPressed = true`. If so, set `isPressed = false`, call `onButtonPressed` and play a close sound

If not, do the same but set `isPressed = true` and play an open sound

### camera_A

Actor used for cameras in the book. Used in `Lvl_FirstPerson`

##### Event Graph

When `CE_Deactivate` is called, `Capture Every Frame = false`

When `CE_Activate` is called, `Capture Every Frame = true`

##### Construction Script

Clamps where the camera buttons can be placed on the widget

### ceiling_A

Plane used for the ceiling of the cabin. Used in `Lvl_FirstPerson` for aesthetic purposes

### door_A

Actor that behaves as a closable door. Used in `Lvl_FirstPerson`

##### Event Graph

When `onButtonPressed` is called, depending on if `isPressed` is true or false, play or reverse an animation that gradually moves the actor up or down using a Lerp function.

### grass_A

Plane used as the floor outside the cabin. Used in `Lvl_FirstPerson` for aesthetic purposes

### SM_Cube20

Stock unreal actor with `brick_small_old_Mat` applied to it to be the floor of the cabin. Also used to block light from `door_A` from outside the cabin due to `walls_A` being planes. Used in `Lvl_FirstPerson` for aesthetic purposes

### table_A

Cube used to construct the table in the office. Used in `Lvl_FirstPerson` for aesthetic purposes

### TitleCharacter_A

Cube used as the player character in `Lvl_Win`.

##### Event Graph 

`Event BeginPlay` plays `win_Cue` and shows mouse cursor

### TitleScreenCharacter_A

Cube used as the player character in `Lvl_Controls` , `Lvl_Lose` and `Lvl_TitleScreen`

##### Event Graph 

`Event BeginPlay` shows mouse cursor

### treeline_A

Plane used to compose the background seen on the cameras and outside the door frame. Used in `Lvl_FirstPerson` for aesthetic purposes

### walls_A 

Plane used to construct the walls of the cabin. Used in `Lvl_FirstPerson` for aesthetic purposes and to ensure the player does not see the zombie at all times.

### wooden_fence_beam5

Static mesh of a fence. Used in `Lvl_FirstPerson` for aesthetic purposes

### zombiePawn_A

Pawn possessed with the `BullyAI_Controller`.  Used in `Lvl_FirstPerson` as the threat

### zombiePawnNOAI_A

Instance of `zombiePawn_A` without being processed `BullyAI_Controller`  Used in `Lvl_Win` as a background element

## AI

### BullyAI controller

Runs the `BullyAI` behaviour tree to apply the logic to `zombiePawn_A`. Calls `book_A`, `zombiePawn_A`, `button_A` and `BP_FirstPersonCharacter` for variables to be applied to `BullyAI_Blackboard` keys.

### BullyAI 

Main behaviour tree of the AI controller. After waiting between `3-5` seconds, it calls `BullyAI_CheckRoom_Task`, and then `BullyAI_MoveAndAttack_Task` with a sequence.

### BullyAI_Blackboard

To store `currentRoomKey` and object actor references

### BullyAI_CheckRoom_Task

`Event Recieve Execute AI` gets all of the references necessary and then checks if the player's `power = 0`. If so, it sets `currentRoom = 9` and `finish execute` is called. If not, generates a number between `1` to `20` and compares it to `aggressionLevel`. If `rand(1 to 20) <= aggressionLevel`, the function `roomCheck` is called. If not, `Finish Execute` is called. 

When `onRoomChanged_Event` is called, it sets `currentRoomKey` to whatever `currentRoom` is equal to

##### roomCheck

Manually checks with every room if `currentRoom` is equal to that room. If so, using a `Random Integer in Range` function, determines which room is set as the room the AI will move to. It then calls `onRoomChanged`

### BullyAI_MoveAndAttack_Task

`Event Recieve Execute AI` gets all of the references necessary and then calls the macro `setEnemyRoom`

##### setEnemyRoom

Checks `currentRoomMove` and if `currentRoomMove` is between `1-8`, simply moves the controlled pawn to a desired location and `Finish Execute` is called. If `currentRoomMove = 9`, the 'death sequence' is initiated, where `book_A` is destroyed, `jumpScare_Cue` is played, `zombie_Anim_Armature_Attack1` is played, a delay of `1.5` initiated, and then `Lvl_Lose` is opened. Then `Finish Execute` is called. 

## Widgets

### controls_UMG

Displays the controls and rules of the game. Used in `Lvl_Controls`

##### Event Graph

`On Clicked (MainMenu)` opens `Lvl_TitleScreen`

### loseScreen_UMG

Displays 'You Died' text. Used in `Lvl_Controls`

##### Event Graph

`On Clicked (MainMenu)` opens `Lvl_TitleScreen`

### mainUI_UMG

Displays the `powerPercent` and `interpretedTime` from `BP_FirstPersonCharacter` to the screen and has 'Mana' written above `powerPercent` to diegetic reasons. Used in `Lvl_FirstPerson`

##### Event Graph

###### GetText

Displays `powerPercent` from `BP_FirstPersonCharacter` and converts it to text

###### GetText_0

Displays `interpretedTime` from `BP_FirstPersonCharacter` and converts it to text

### room_UMG

Creates the instances for the buttons `book_A` depending on how many `camera_A` instances exist

##### Event Graph

`Event Construct` places the buttons on their locations based on `X` and `Y` values set on each `camera_A` instance and sets the buttons' text to a `code` designated to each camera

`On Clicked (Camera_Button)` calls `selectCamera`

###### selectCamera

Calls `SwitchCamera_Room` and plays animation of the button selected flickering 

###### deselectCamera

Calls stops the animation of the previous button selected flickering

### TitleScreen_UMG

Displays the title, play game, controls and quit buttons. Used in `Lvl_TitleScreen`

##### Graph view

`On Clicked (Play)` opens `Lvl_TitleScreen`, `On Clicked (Quit)` quits the game, `On Clicked (Controls)` opens `Lvl_Controls`

### winScreen_UMG

Displays 'You Win' text. Used in `Lvl_Win``

##### Graph view

`On Clicked (MainMenu)` opens `Lvl_TitleScreen`

## Levels

### Lvl_Controls

Level used to display `controls_UMG`

##### Level Blueprint

`Event BeginPlay` calls `controls_UMG` to the viewport

### Lvl_FirstPerson

Level used to facilitate the main gameplay look

### Lvl_Lose

Level used to display `loseScreen_UMG`

##### Level Blueprint

`Event BeginPlay` calls `loseScreen_UMG` to the viewport

### Lvl_TitleScreen

Level used to display `TitleScreen_UMG`

##### Level Blueprint

`Event BeginPlay` calls `TitleScreen_UMG` to the viewport

### Lvl_Win

Level used to display `winScreen_UMG`

##### Level Blueprint

`Event BeginPlay` calls `winScreen_UMG` to the viewport

## Textures, Materials and Render Targets

### bober & bober_Mat

Image of a cat simply used as a place holder texture. Used on `button_A`, `TitleCharacter_A`, and`TitleScreenCharacter_A` 

### candleholder & body_Mat

Image of a lantern. Used on `Body1`

### brick_small_old & brick_small_old_Mat

Image of a brick wall. Used on `SM_Cube20`

### T_GridChecker_A, Camera_Material, Camera_RT & RenderTarget

Image of a stock unreal asset used as a placeholder until image is replaced by a render target during gameplay. Used on `book_A`

### CameraShader_Inst & CameraShader_Mat

Shader used for the book cameras, intended to produce a 'Cel-Shading' filter, but in practice, it is not working. Still used as it gives a muted colour scheme for the book. Used on `PostProcessVolume2` with colour grading

### corpse1 & zombie1_Mat

Image of an unwrapped zombie 3D model texture. Used on `zombiePawn_A` and `zombiePawnNOAI_A`

### fire & Light_Mat

Image of a gradient from bright red to bright yellow, made emissive and bright. Used on `Body1`

### grass_dead_01 & grass_dead_01_Mat

Image of grass. Used on `grass_A`

### low_wooden_wall, low_wooden_wall_Mat & low_wooden_wall1_Mat 

Image of wood. Used on `wooden_fence_beam5`. `low_wooden_wall_Mat` is unused and is identical to `low_wooden_wall1_Mat`

### red_gemstones_norm & red_gemstones_norm_Mat

Image of a gemstone texture, made emissive. Used on `door_A`

### RetroFilter_Inst & RetroFilter_Mat

Shader used for the office/cabin game. Has two parameters; `Pixelation` and `Colour` that determine colour density in the image and the resolution the camera outputs to the monitor.

### table_door & table_door_Mat 

Image of a door that has been trimmed to remove the handle. Used on `table_A`

### tileStar 

Image of a white star. Used on `TitleScreen_UMG` when `HasWon = True`

### TreeLine & TreeLine_Mat

Image of a forest. Used on `treeline_A`

### wood_planks_2 & wood_planks_2_Mat

Image of wood planks. Used on `walls_A`

### wood_planks_fancy_03 & woody_planks_fancy_03_Mat

Image of  wood planks. Used on `ceiling_A`

## Sound Effects and Cues

### ambience & ambience_Cue

Placed as an `AmbientSound` in `Lvl_FirstPerson`

### doorClose & doorClose_Cue

Called by `button_A` whenever it is interacted with AND `isPressed =true`

### doorOpen & doorOpen_Cue

Called by `button_A` whenever it is interacted with AND `isPressed = false`

### jumpScare & jumpScare_Cue

Called by `BullyAI_MoveAndAttack_Task` during the death sequence when `currentRoomMove = 9`

### knock & knock_Cue

Called by `BullyAI_CheckRoom_Task` in the `roomCheck` function when `isPressed = true`

### win & win_Cue

Called by `TitleCharacter_A` upon `Event BeginPlay`

## Input Mapping Contexts

### default_IMC

Has the `Show Cameras` action mapping, that uses the `Space Bar` to be called

## Game Instances

### GameInstance_GI

Used to keep track of a `Has_Won` variable that is set to `false` by default. When `Has_Won = true`, it is called by `TitleScreen_UMG` to display `tileStar` on the main menu



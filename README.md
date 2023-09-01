# Super Mario 64 but he is very small and fragile #
- [How To Setup](#how-to-setup)
  - [Preparations](#preparations)
  - [Patching the ROM](#patching-the-rom)
- [Inspirations for the Mod / Challenge](#inspirations-for-the-mod--challenge)
  - [Video Inspirations](#video-inspirations)
  - [Why I created the Mod](#why-i-created-the-mod)
  - [Why I documentate everything](#why-i-documentate-everything)
- [Creation Process of the Mod](#creation-process-of-the-mod)
  - [Preperations](#preperations)
  - [Editing of the Code](#editing-of-the-code)

<br>

# How To Setup # 
This is a Guide on how to setup the Challenge on an Emulator.

## Preparations ##
To get this to work you need the following things already:
* A Mario 64 ROM (US Version) --> I can't tell you where to get that
* An Emulator to play the Game setup (Examples: Project64, Mupen64Plus, Parallel Launcher)
  * Project64: https://www.pj64-emu.com/
  * Mupen64Plus: https://mupen64plus.org/
  * Parallel Launcher: https://parallel-launcher.ca/
* The Patch file for the Mod: 
* Floating IPS to patch the ROM: https://www.romhacking.net/utilities/1040/

## Patching the ROM ##

After having the Game with an Emulator setup and the Patch File and Floating IPS downloaded follow these steps:
1. Unzip the .Zip Folder of Floating IPS: <br>
<img src="Images/Unzipping Folder.png" width="230" height="130">

2. Open the file "flips.exe" in the newly unzipped folder and click on "Apply Patch": <br>
<img src="Images/Apply Patch.png" width="220" height="100">

* First select the Patch (.bps) File
* Then select the Mario 64 (US Version) ROM
* And at last define a name of how the newly created (patched) ROM should be called

3. Now you can use the new ROM to play the Mod on the Emulator of your choice :D

<br>

# Inspirations for the Mod / Challenge #
This is how I was inspired to try out this Challenge.

## Video Inspirations ##
I saw this Mod in Super Mario Odyssey in a Video of SmallAnt:

<a href="https://www.youtube.com/watch?v=3t7oWX5d9nI"><img src="Images/SmallAnt Thumbnail.jpg" width="500" height="281.25"></a>
<br> Mod Creator: https://www.youtube.com/@CraftyBoss925

## Why I created the Mod ##

I'm trying to learn how to create Mods for Games that interest me, like the 3D Mario Games, so I thought that I would try to recreate another Mod Idea in Super Mario 64 (because it doesn't exist yet in that game) for the beginning. I'm planning of creating more and more unique Mods as time goes on and learn more and more things.

## Why I documentate everything ##
1. I myself wanna remember, what I exactly did in the future, since I wanna create more Mods
   
2. I wanna inspire people to try out creating Mods themselves and I think if I show off how I did it, it could give people an idea of how they could do the same

<br>

# Creation Process of the Mod #
I will explain how I created the Mod.

## Preperations ##
To edit the Source Code of Super Mario 64, I got the UltraSM64 repository set up. 

The UltraSM64 Repository is a Fork of the SM64 decompilation project that adds several quality-of-life patches: <br>
https://github.com/CrashOveride95/ultrasm64

A fork is a new repository that shares code with the original repository, where you can make changes without affecting the original repository.

--> The Super Mario 64 Decompilation Project is an absolutely impressive project by Fans that reverse engineered and decompiled the entire source Code of Super Mario 64. Saying it more simplified, they analyzed the machine code and recreated the code in a higher-level programming language.

### How to setup the repository ###
To setup the repository I followed this tutorial, but instead of cloning the HackerSM64 repository I cloned the UltraSM64 repository: <br>
https://github.com/HackerN64/HackerSM64/wiki/Installing-HackerSM64

## Editing of the Code ##
After setting up the repository I tried to edit the Mario 64 Code for the first time.

For editing the Code I would recommend everyone to use Visual Studio Code: <br>
https://code.visualstudio.com/

For a lot of questions I had, the "SM64 Rom Hacks" Discord Server helped me out a lot (Thank you so much ❤️): <br>
https://discord.com/invite/BYrpMBG

I wanna say, that this is my first Mod for the Game I ever made, so I just changed stuff to my liking till it felt right. I used a lot Code parts over and over again and I am aware that my changes could be optimized so much further and aren't the most optimal way to create this Mod.

### File Listing ###
These are all the Files, that I changed:
* object_list_processor.c
* mario.c
* mario_actions_moving.c
* mario_actions_airbone.c
* mario_actions_submerged.c
* dialogs.h (us folder)

### What I changed ###
Here are the changes listed, that I made to the files.

____________________________________
#### object_list_processor.c --> Is used to processing objects within the game.

To change Marios size permanently (shrinking him down by 50%) I added the following Code to the Function **"bhv_mario_update"**: <br>
```C
// Apply scaling here
f32 scale = 0.5; // Set the desired scaling factor (e.g., 50%)
    
// Apply scaling to the Mario object's scale vector
gCurrentObject->header.gfx.scale[0] = scale;
gCurrentObject->header.gfx.scale[1] = scale;
gCurrentObject->header.gfx.scale[2] = scale;
```

____________________________________
#### mario.c --> Contains the logic of functionalities regarding Mario.

Mario should die each time he gets damage, so I changed m->health to 0 no matter the damage reason in the function "update_mario_health":
```C
// Set Mario's health to zero when he takes damage
m->health = 0;
```

I changed Mario Lives he gets in the beginning to 100, so you wouldn't constantly get a Game Over (I changed this line):
```C
gMarioState->numLives = 100;
```

____________________________________
#### mario_actions_moving.c --> Handles behaviour of Mario while Moving

I changed a bunch of speed stats. I just searched for speed, looked what the respective speed stat is for (walking speed, shell speed, etc ...) and changed it, until it felt right.

Example for walking speed:
```C
if (m->floor != NULL && m->floor->type == SURFACE_SLOW) {
        maxTargetSpeed = 15.0f;
    } else {
        maxTargetSpeed = 17.0f;
    }
```
--> The first stat is for the walking speed on slow surfaces and the second one for normal surfaces

____________________________________
#### mario_actions_airborne.c --> Handles behaviour of Mario while in the air.

First I had to edit some threshold values.

Example for Long Jump:
```C
dragThreshold = m->action == ACT_LONG_JUMP ? 12.0f : 8.0f;
```

Then I changed a bunch of jump heights and set speed limits (Nomal Jump, Backflip, Long Jump, lava bounce, box bounce, etc ...).

To limit the jump height you can use the following command in the respective function of the jump type (maxJumpHeight needs to be edited to the respective height):
```C
f32 maxJumpHeight = 30.0f;  // Adjust this value to set the maximum jump height

    // Limit the jump height
    if (m->vel[1] > maxJumpHeight) {
        m->vel[1] = maxJumpHeight;
    }
```

To limit the speed you can use the following command in the respective function of the jump type (maxHorizontalSpeed needs to be edited to the respective speed limit):

```C
f32 maxHorizontalSpeed = 30.0f;  // Adjust this value to set the maximum horizontal speed

// Limit the horizontal speed
    if (m->forwardVel > maxHorizontalSpeed) {
        m->forwardVel = maxHorizontalSpeed;
    }
```

To make things like the BLJ Glitch (Backwards Longjumps) impossible I had to limit the backwards velocity of a bunch of Jump types.

Example for Long jump:
```C
s32 act_long_jump(struct MarioState *m) {

    // Limit the backward velocity if it's too high
    if (m->forwardVel < -8.0f) {
        m->forwardVel = -8.0f;
    }
```

I changed marios fallheight and damageheight, so he can't jump from too high before getting damage (He can jump as high as a small double jump). I changed the numbers before the "f"s until I got my expected result:
```C
if (m->actionState == ACT_GROUND_POUND) {
        damageHeight = 40.0f;
    } else {
        damageHeight = 35.0f;
    }

#pragma GCC diagnostic pop

    if (m->action != ACT_TWIRLING && m->floor->type != SURFACE_BURNING) {
        if (m->vel[1] < -35.0f) {
            if (fallHeight > 50.0f) {
```

So that Mario will always get stuck when he jump above snow, I changed the peakHeight to a  negative number:
```C
if (!(flags & 0x01) && m->peakHeight - m->pos[1] > -1.0f && floor->normal.y >= 0.8660254f)
```

____________________________________
#### mario_actions_submerged.c --> Handles behaviour of Mario while submerged underwater.

I changed the swimming speed to be really slow:
```C
f32 maxSpeed = 7.0f;
```

____________________________________
#### dialogs.h (Us Folder) --> Contains written dialog of the game

I changed the written dialog in the beginning cutscene of the game:
```h
DEFINE_DIALOG(DIALOG_020, 1, 6, 95, 150, _("\
Dear Mario:\n\
You are short. Lol."))
```
____________________________________

### Testing ###
If changes need to be tested, the following command can be used in the Ubuntu command line to create a rom (you need to change to the right directory first):
```
make -j$(nproc)
```

After that a folder named "build" will be created with the created ROM inside it.

To test I would recommend the following Emulator (It's the most practical one to test in my opinion): <br>
https://parallel-launcher.ca/

# EXP Template

The EXP Template is a ready-to-use Unreal Engine 5.3 project that contains all of our submodules developed within the EXP Framework, which is designed for multiplayer, embodied XR experiences.
Feel free to use and experiment with it!

This project is ideal for LBX (Large-Base Experiences). It is perfect for a performer and a Unreal/VR technician duo.

All of the assets developped here are in the Plugins folder of this template. For more details on how all of the submodules work, visit our [EXP](https://github.com/CREW-Brussels/EXP) page or our [wiki](https://github.com/CREW-Brussels/EXP/wiki)!

EXP_Template contains the following submodules developed by CREW:
- [CHORUS](https://github.com/CREW-Brussels/CHORUS/tree/main)
- [CIRCA](https://github.com/CREW-Brussels/CIRCA/tree/main)
- [CREWAnimationUtilities](https://github.com/CREW-Brussels/CREWAnimationUtilities/tree/main)
- [CREWNetworkFramework](https://github.com/CREW-Brussels/CREWNetworkFramework/tree/main)
- [CREWXRFramework](https://github.com/CREW-Brussels/CREWXRFramework/tree/main)
- [SPINOSC](https://github.com/CREW-Brussels/SPINOSC/tree/main)

***
Requirements:
- Unreal Engine 5.3
- [Download](https://github.com/CREW-Brussels/EXP_Template)
- Download the two following plugins in the *Plugins* folder:
  - [Vive Open XR](https://developer.vive.com/resources/openxr/unreal/unreal-download/latest/)
  - [VR Expansion Plugin](https://github.com/mordentral/VRExpansionPlugin/tree/5.3-Locked)
- Your headsets are set up with LBE tracking maps.
***

# EXP Template: A full XR performance use case

**This project comes with a multiplayer demo, with 1 performer and multiple immersants**

A typical demo using this template involves the following situation:
- A performer wearing a motion capture suit and a headset with controllers.
- Other VR players (immersants) can participate in the experience and observe the performer.
- The performer, who is invisible at first, can modify and duplicate their avatar with different triggers and buttons on the controller using the [CHORUS animation subsystem](https://github.com/CREW-Brussels/CHORUS/tree/main) (developed later).

***

## Set up the server and Gamemode

The server (main computer with Unreal Editor), should be in Play mode.
>   To make sure that multiplayer works, please make sure in Playmode settings (the three dots on the right of the viewport play buttons), that the **Net Mode** is set to **Play As Listen Server**. This will allow receiving data from the server, and so trackers and other headsets information.

![68747470733a2f2f74393031323137323438372e702e636c69636b75702d6174746163686d656e74732e636f6d2f74393031323137323438372f61366662376465662d653565382d343431662d623733372d6230653730666661366637362f53637265656e73686f74253230323032342d30392d303625323031323132](https://github.com/user-attachments/assets/c2dba806-dcbd-4974-8f3f-b062a17380a9)



> You should also check that the Gamemode, by clicking on the blueprint class logo, make sure that the world override is using a gamemode with your VR Player.

>![68747470733a2f2f74393031323137323438372e702e636c69636b75702d6174746163686d656e74732e636f6d2f74393031323137323438372f65396633656164642d636461302d346335612d613636312d3464313236383061343965352f53637265656e73686f74253230323032342d30392d303625323031323035](https://github.com/user-attachments/assets/e8f4ce30-7cf6-4a09-8049-f6f056e7717a)

> ![68747470733a2f2f74393031323137323438372e702e636c69636b75702d6174746163686d656e74732e636f6d2f74393031323137323438372f30303562643231632d343562382d343731372d393338662d3031643461336237323539652f53637265656e73686f74253230323032342d30392d303625323031323034](https://github.com/user-attachments/assets/fa36a125-df7f-4a91-8bcb-5e683046822e)

***

## Set up a multiplayer experience, with multiple clients (the players, meaning the immersants and performer in that case) and a server
### Method 1 (easiest): 

The project is already set up with the [CREW Network Framework plugin](https://github.com/CREW-Brussels/CREWNetworkFramework/tree/main), but you can get to understand more how it works in the documentation.
It will always work as long as all the application name in the settings and ports are the same, and that they are connected on the same network.


### Method 2: Building the app and making a local shortcut of your .exe app to allow it launching to your computer and device
#### Connecting as a client**

*   Platforms➝Windows➝Package project
*   Once your build is done, click right on the .exe app, and **create a shortcut (right click ➝ more options)**
*   Add "<serverIP>:17777" at the end of the target of the project shortcut

![](https://t9012172487.p.clickup-attachments.com/t9012172487/a2b9dbdf-4228-426f-8aac-55208c7d83a1/Screenshot%202024-09-06%20122158.png)

*   Open Vive business streaming on your headset
*   Play on the Viewport button in unreal
*   Open the shortcut .exe app
  
#### Launch a listen server from a package
to use the unreal editor for something else, while a project is playing

*   Platforms➝Windows➝Package project
*   Once your build is done, click right on the .exe app, and **create a shortcut**
*   Add "<LevelName>?listen?port=<PortNumber>"
*   Open Vive business streaming on your headset
*   Play on the Viewport button in unreal
*   Open the shortcut .exe app


### Method 3: Build an Embbed app for Android 
Package an app with Android ASTC: 
- first install the Android SDK following Unreal's documentation on this topic: https://dev.epicgames.com/documentation/en-us/unreal-engine/how-to-set-up-android-sdk-and-ndk-for-your-unreal-engine-development-environment?application_version=5.3
- Package your app. If there any errors try the following steps: kill all of your Unreal Editors in the Task Manager, delete the *Intermediate* folder in your project, open the project again and try to rebuild it.

In embbed on the headsets, the app can't be too heavy. 

***

### Send your live animation data 

In this template, we can't publish the Motion Capture assets as we don't own them. We used a recorded animation sequence, but you could replace it in the ABP_Rebroadcaster.
![Screenshot 2025-05-21 165734](https://github.com/user-attachments/assets/72b67a74-fd97-412b-8324-67e47578508b)

Here, since it's the ABP sending the animation data, you can replace the node with a MVN Live Link Pose with a MVN Retargeter and T-Pose (in the case you're using an Xsens suit) or an other node depending on what MOCAP technology you use. 
> In this example, the sent data is received in the ABP_Performer.

***

### Select the role of your players 

Once you launch the server, in the server editor mode, you able to chose which one of the player connected, in our case the person wearing a mocap suit, is the performer.

- Select the performer player in the server editor mode
- search *role*
- click on the button *is performer*. There is also a button *is immersant* but every player is immersant by default.

***

### CHORUS in this project

In this example, we are uing an echo manager (echos are the iterations triggered by the performer with the left trigger in that case) and crowd manager (crowd is the population of recorded and replayed avatars created by the performer).
Always make sure the Bluprint of your perforer is linked to the following blueprints from Chorus like on the picture: BP_ChorusEchoManager and BP_ChorusCrowdManager. Otherwise the performer's tools won't work.
![Screenshot 2025-06-02 175002](https://github.com/user-attachments/assets/75d2a12e-dea9-41a0-a863-0d2ae74b935d)

You are able to modify the settings of the crowd and echoes with BP_ChorusEchoManager and BP_ChorusCrowdManager that are in your scene. The pool size is the number of avatars that will be generated either for the echoes or the crowd. 

![Screenshot 2025-06-02 175026](https://github.com/user-attachments/assets/b7737ee0-934c-41f7-bdd3-e8a0199d268f)

![Screenshot 2025-06-02 175042](https://github.com/user-attachments/assets/9778c45b-bd5c-4627-9b54-5279709303c6)

The performer has the following buttons:
  - Right Trigger: Launch and unlaunch the echoes
  - A button: Posses the closest crowd avatar. If the performer is already posssessing one, it will make them unpocess it.
  - B button: Record crowd. If the peformer is already possessing an avatar:
      - 1 clic: Start recording
      - 2nd clic: Stop recording
        or
      - First use the Posses button to unpocess and then play recording 
  - X button: Spawn Crowd until the maximum capacity of the crowd pool
  - Y button: Discard crowd, delete 1 by 1 the crowd avatars of the performer. If the performer was possessing an avatar, it will first make it invisible and then delete the closest crowd avatars.
  - Right thumbstick up: Increase the speed of your last recording
  - Right thumbstick down (or left thumbsitck for Cosmos and Focus3 controllers): Decrease speed of your last recording
  - Right trigger: Change the visibility of the immersants. They will however still see each other at a minimum distance, so they don't bump into each other. The performer will always see them but the mesh will look different when the immersants are unvisible to one another.
 
![Screenshot 2025-06-02 175218](https://github.com/user-attachments/assets/e52b868d-5bd4-447b-8f97-8c7df875cb13)

![Screenshot 2025-06-02 175250](https://github.com/user-attachments/assets/f2099e49-280f-4828-85e7-064592e0f96a)

***

### Archive your experience with CIRCA
For this, you should follow [CIRCA's documentation](https://github.com/CREW-Brussels/CIRCA/tree/main).


## About
<img src="https://github.com/user-attachments/assets/2ffa225b-2966-4f68-8106-3fd403fd6988" alt="CREW-LOGO" width="130"/>  
<img src="https://emil-xr.eu/wp-content/uploads/2022/10/logo_emil-272x300.png)" alt="EMIL-XR-LOGO" width="100"/>

> EXP Template is being developed by [CREW](http://crew.brussels) as part of [EMIL](https://emil-xr.eu/), the European Media and Immersion Lab, an Innovation Action funded by the European Union and co-funded by Innovate UK. 

## Funding
<img src="https://emil-xr.eu/wp-content/uploads/2022/10/EN-Funded-by-the-EU-POS-1024x215.png)" alt="EU" height="100"/>

> This project has received funding from the European Union's Horizon Europe Research and Innovation Programme under Grant Agreement No 101070533 EMIL.

## Overview

PaperZD exists as an answer for Unreal Engine’s lack of support for 2D based games, and seeks to help its users to easily create complex animated characters, by binding the logic in a similar fashion as it’s done in usual 3D development, while adjusting the experience to better serve a 2D game.

The plugin comes with its own “Animation Blueprint” implementation (paired with a custom blueprint compiler) with its own Animation Graph, this allows for easy to follow animation logic for each character or entity.

![1-1](https://drfs.ctcontents.com/file/12489212/17569905074320/2775ea/UE.Documentation/PaperZD/image/1-1.png)

Support for specialized “Animation Notifies” is also present and can be added to each animation sequence that requires it.

![1-2](https://drfs.ctcontents.com/file/12489212/17569905074357/032860/UE.Documentation/PaperZD/image/1-2.png)

Besides this, a plethora of other 2D specific features are available for the user which will help create rich and complex characters without the fuss of having to manage the timings themselves.

PaperZD also allows for “Extensions” to be added to the system, which can allow for other types of animation sources to be used with the tool. These extensions can give access to manage skeletal 2d animation sources like “Spine2D”, “DragonBones” or “Creature2D” (or any custom one defined by the user). Currently only "Spine2D" and "Paper2D" are supported.

The current version of the plugin (called V2.0) heavily re-writes the old architecture and aims for creating a tool that can serve both big and small projects alike, becoming a pivotal piece of software for any user that aims to develop 2D games in Unreal Engine.

## Installation

### How to install

If downloaded via the Marketplace/FAB, installation is already done and the plugin should just be enabled via the “Plugins” menu (An engine restart will be required).

![2-1](https://drfs.ctcontents.com/file/12489212/17569905080481/5a9c05/UE.Documentation/PaperZD/image/2-1.png)

For installing PaperZD as a “Project Plugin”, follow the same steps that on any plugin, simply add the plugin inside a folder called “Plugins” inside your project folder.

Installation of extensions can be performed by adding “Plugin Extensions”, which are intermediary plugins that give PaperZD the ability to manage other types of animation sources besides Paper2D. Due to the nature of these tools being external to the engine, said extensions cannot be provided in the marketplace and must be installed separately (More information in the “Adding support for custom animation sources” TODO: Link section).

### Important Notes

#### Note for "Blueprint Only" Projects

> ℹ️ For any project using C++ plugins, the user needs to be able to compile the projects. This requires having a valid VisualStudio and > toolchain installations even on Blueprint Only projects.

#### Note for C++ users:

> ℹ️ If you require importing and using the PaperZD classes in your game project (like inheriting from PaperZDCharacter or creating custom notifies in c++), then the “PaperZD” module should be added on your project’s “PublicDependencyModules” otherwise a linker error will trigger.

#### Note for v1.0 users:

> ℹ️ Whenever the system loads an AnimationBlueprint that was created with a previous version (v.1.4.3) for the first time, it will update it and create an “Animation Source” for it, while also re-targeting every AnimSequence to now have said object as parent. Due to the nature of Unreal Engine’s Loading protocols, we cannot force the packages to save and can only mark them dirty, it is up to the user to ensure they are properly saved.

# Setting up the animation objects

## Creation of an **AnimationSource**

The first step when creating a new character, is to create what’s called an **Animation Source**. This object works as the source of animations that the **AnimationBlueprints** can use to render the character and control its playback.
By default PaperZD comes with a **“Flipbook Animation Source”** which lets the system know how to manage and render flipbook animated characters, but other different sources can be created which can have other features (like being skeleton based and thus, being able to opt-in for blending support). First, right click on your content browser and go to the **PaperZetaD** section and create a new **Animation Source**:
![3-1](https://drfs.ctcontents.com/file/12489212/17569905132288/8e2cd9/UE.Documentation/PaperZD/image/3-1.png)
>ℹ️ Note: If there aren’t any extra Animation Sources supported besides Paper2D Flipbooks, the system will now create a new asset, otherwise it will ask you to first identify which type of Animation Source you want to create.

With the newly created Animation Source, which we named “MyAnimSource” we will begin the setup of the animations for our character:
![3-2](https://drfs.ctcontents.com/file/12489212/17569905132289/359372/UE.Documentation/PaperZD/image/3-2.png)
## **AnimSequence** creation and setup

The AnimSequence is the object that contains the **Animation** to be rendered, and any metadata pertaining to events inside it (like **AnimNotifies**). **AnimSequences** live inside an **AnimationSource** and thus, we need to specify one when creating a new **AnimSequence**.
One can create a new **AniSequence** either by right clicking on the content browser and selecting from the drop-down, like in the previous step, or inside the **Animation Source Editor** (TODO: Ist this the Animation Editor or the Animation Bliueprint Editor? Either way, add link).
When creating an **AnimSequence** from the drop-down the system will ask the user to specify to which Source this sequence should be attached. The type of the **AnimSequence** to be created depends on the **AnimationSource** you select, selecting an **AnimationSource** for Paper2D Flipbooks (TODO: Check if this is an actual THING in UE) will create a specialized **AnimSequence** for them, the user doesn’t need to worry about it.
![3-3](https://drfs.ctcontents.com/file/12489212/17569905132290/26b9e8/UE.Documentation/PaperZD/image/3-3.png)

![3-4](https://drfs.ctcontents.com/file/12489212/17569905132291/04809b/UE.Documentation/PaperZD/image/3-4.png)

We’ll call this AnimSequence “MyFirstAnimSequence”
![3-5](https://drfs.ctcontents.com/file/12489212/17569905132292/053259/UE.Documentation/PaperZD/image/3-5.png)
After that, double click on either your **AnimationSource** or your **AnimSequence**, to enter the **[Animation Editor](https://github.com/heavybullets/PaperZD/wiki/03.-Setting-up-the-animation-objects#the-animation-editor)** .
## The Animation Editor

When first opening the asset, you’ll find yourself with the following view:
![3-6](https://drfs.ctcontents.com/file/12489212/17569905132293/f6c81a/UE.Documentation/PaperZD/image/3-6.png)
To the left, an asset browser will display all the **AnimSequences** that are currently parented to this **AnimationSource**, a single one should be displayed if you proceeded with the creation of your first **AnimSequence** in the previous step
![3-7](https://drfs.ctcontents.com/file/12489212/17569905132294/336a43/UE.Documentation/PaperZD/image/3-7.png)
Clicking on this **AnimSequence** will open the asset for edition, you should see a new tab appearing on the bottom-center part of the screen.
![3-8](https://drfs.ctcontents.com/file/12489212/17569905132296/c14729/UE.Documentation/PaperZD/image/3-8.png)
To the right side of the screen the **Asset Details tab** should also have been filled with the pertaining information of your **AnimSequence**.
![3-9](https://drfs.ctcontents.com/file/12489212/17569905132297/d8b5cb/UE.Documentation/PaperZD/image/3-9.png)
Now, let’s specify the **Flipbook** that we want to use for rendering this sequence. We’ll use the Paper2D example content project animations, and select the **Idle** animation. After we select the animation, the Viewport and Timeline on the center should reflect the newly created Animation
![3-10](https://drfs.ctcontents.com/file/12489212/17569905132298/c4ded5/UE.Documentation/PaperZD/image/3-10.png)
![3-11](https://drfs.ctcontents.com/file/12489212/17569905132299/ec37bc/UE.Documentation/PaperZD/image/3-11.png)
We now have a simple AnimSequence set up, we can proceed and create another one for the **walking** behavior. Instead of creating it from the content browser, we can create one directly in this editor, by clicking on the **+Add New** button on top of the **AnimSequence** list.
This will create and select a new **AnimSequence** with a default new name
![3-12](https://drfs.ctcontents.com/file/12489212/17569905132274/201173/UE.Documentation/PaperZD/image/3-12.png)
We can then set its Flipbook to be the **Walking** animation
![3-13](https://drfs.ctcontents.com/file/12489212/17569905132275/5485e1/UE.Documentation/PaperZD/image/3-13.png)
>ℹ️ NOTE: By default the **AnimSequences** created via this method are stored in a **folder** called **AnimSequences** which will be created **on the same level as the AnimationSource**. This behavior can be changed in the **PaperZD Editor section in the Projects settings**.

We can also proceed to rename our **AnimSequences** with a more recognizable name, let’s call them **Idle** and **Walk** respectively. Renaming can be done in the [Animation Editor](https://github.com/heavybullets/PaperZD/wiki/03.-Setting-up-the-animation-objects#the-animation-editor) by simply clicking the animation and pressing **F2**.
![3-14](https://drfs.ctcontents.com/file/12489212/17569905132276/7416e0/UE.Documentation/PaperZD/image/3-14.png)
![3-15](https://drfs.ctcontents.com/file/12489212/17569905132278/3d9f54/UE.Documentation/PaperZD/image/3-15.png)
With this we have the backbone for creating our animation logic, while there are other things we can do in this part of the editor, we’ll get back to those in later chapters.
For now, we will create our Animation Logic, by creating a new **AnimationBlueprint** that uses our **AnimationSource** as the base. As always, right click on your content browser, and select the following option (when prompted, choose your **AnimationSource** as the base).
![3-16](https://drfs.ctcontents.com/file/12489212/17569905132279/db8635/UE.Documentation/PaperZD/image/3-16.png)
We’ll call it “MyAnimBP”
![3-17](https://drfs.ctcontents.com/file/12489212/17569905132280/95a846/UE.Documentation/PaperZD/image/3-17.png)
This asset will open the “[Animation Blueprint Editor](https://github.com/heavybullets/PaperZD/wiki/03.-Setting-up-the-animation-objects#the-animation-blueprint-editor)”
## The Animation Blueprint Editor

When opening the **Animation Blueprint Editor** for the first time, you’ll see a familiar Blueprint Editor window, with an Event Graph and an added **Animation Graph**
![3-18](https://drfs.ctcontents.com/file/12489212/17569905132281/48f081/UE.Documentation/PaperZD/image/3-18.png)
Double clicking the **Animation Graph** enables you to modify the logic that the blueprint will follow to render the final animation.
![3-19](https://drfs.ctcontents.com/file/12489212/17569905132282/14e5df/UE.Documentation/PaperZD/image/3-19.png)
As a simple approach, we will use a `Select` node to switch between our two already created animations, depending on the velocity of the Character, displaying the **Walk** animation when moving and the Idle animation when not.
![3-20](https://drfs.ctcontents.com/file/12489212/17569905132283/fc5a6f/UE.Documentation/PaperZD/image/3-20.png)
Other nodes can also be used to specify more complex behavior, like using animation state machines. Those will be explained in later chapters. (TODO: Link)
## The Animation Component and PaperZDCharacter

With the animation logic created, we need to now specify that we want to use it with our character, for this PaperZD comes with a **PaperZDAnimationComponent** that can be added to any Class/Object to drive their animations.
The **AnimationComponent** requires the following data to be set to ensure correct behavior.
- **AnimInstanceClass**: This is the class of the **AnimationBlueprint** you have created and that you wish to use for driving the animations of this component.
- **RenderComponent**: The **PrimitiveComponent** that will be rendering the actual animation, for default behaviors (using Paper2D), this should be a **FlipbookComponent**.

>Note: The **RenderComponent** should be compatible with the **AnimationBlueprint** that you specified.

For default Paper2D characters, PaperZD comes with a **PaperZDCharacter** that already sets up the **RenderComponent** to the main **PaperFlipbook**. If your goal is to create a simple Paper2D character, then inheriting from this class is a good option.
For example purposes, we’ll grab a default PaperZD Character and create a subclass of it. The character will simply route the Left/Right inputs
![3-21](https://drfs.ctcontents.com/file/12489212/17569905132284/137935/UE.Documentation/PaperZD/image/3-21.png)
and we’ll set the **AnimInstanceClass** to point to our created **AnimationBlueprint**.
![3-22](https://drfs.ctcontents.com/file/12489212/17569905132285/e8c305/UE.Documentation/PaperZD/image/3-22.png)
If we now hit play, we should be able to see the animation changing between **Walk**/**Idle** when moving and standing still.
![3-23](https://drfs.ctcontents.com/file/12489212/17569905132286/4c60cd/UE.Documentation/PaperZD/image/3-23.png)






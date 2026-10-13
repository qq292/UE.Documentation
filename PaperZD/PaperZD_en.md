
# Home

Welcome to the PaperZD wiki!




# 01. Overview

PaperZD exists as an answer for Unreal Engine’s lack of support for 2D based games, and seeks to help its users to easily create complex animated characters, by binding the logic in a similar fashion as it’s done in usual 3D development, while adjusting the experience to better serve a 2D game.

The plugin comes with its own “Animation Blueprint” implementation (paired with a custom blueprint compiler) with its own Animation Graph, this allows for easy to follow animation logic for each character or entity.

![1-1](https://drfs.ctcontents.com/file/12489212/17569905074320/2775ea/UE.Documentation/PaperZD/image/1-1.png)

Support for specialized “Animation Notifies” is also present and can be added to each animation sequence that requires it.

![1-2](https://drfs.ctcontents.com/file/12489212/17569905074357/032860/UE.Documentation/PaperZD/image/1-2.png)

Besides this, a plethora of other 2D specific features are available for the user which will help create rich and complex characters without the fuss of having to manage the timings themselves.

PaperZD also allows for “Extensions” to be added to the system, which can allow for other types of animation sources to be used with the tool. These extensions can give access to manage skeletal 2d animation sources like “Spine2D”, “DragonBones” or “Creature2D” (or any custom one defined by the user). Currently only "Spine2D" and "Paper2D" are supported.

The current version of the plugin (called V2.0) heavily re-writes the old architecture and aims for creating a tool that can serve both big and small projects alike, becoming a pivotal piece of software for any user that aims to develop 2D games in Unreal Engine.

# 02. Installation

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

# 03. Setting up the animation objects

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

![3-14](https://drfs.ctcontents.com/file/12489212/17569905132276/7416e0/UE.Documentation/PaperZD/image/3-14.png =450x)

![3-15](https://drfs.ctcontents.com/file/12489212/17569905132278/3d9f54/UE.Documentation/PaperZD/image/3-15.png =450x)

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






# 04. Creating animation events and notifies

For having rich animations, you not only need to be able to correctly display the animation in the correct time and situation (which we accomplished by using the **AnimationBlueprint**), but you also need to be able to play effects, trigger events and play sounds at very precise timings.

For this, PaperZD comes with its own **AnimNotify** system, similar to the one that comes with the Engine.

Opening the **Walk** animation sequence should display the **[Animation Editor](https://github.com/heavybullets/PaperZD/wiki/04.-Creating-animation-events-and-notifies#the-animation-editor)** that we used previously, below the viewport, there’s a **Timeline** that we can use for setting up events.

In this timeline, you can add as many **AnimNotifies** and **NotifyStates** as you want. The plugin comes with some default ones, but you can always create custom Blueprint and C++ subclasses to create your own behavior.

For illustrating the point, we’ll create a “Source AnimNotify”, which is a type of Notify that lives inside the **AnimationSource** and it can be implemented by any **AnimationBlueprint** that inherits from this source.

Right click on the **Timeline**, and click **NewNotify**, filling the name when prompted, let’s call it **Footstep**

![4-1](https://drfs.ctcontents.com/file/12489212/17569905140635/d82286/UE.Documentation/PaperZD/image/4-1.png)

We’ll move it to the correct place (just when the foot hits the floor) and then we’ll create another **Footstep** notify, by selecting from the already created ones.

![4-2](https://drfs.ctcontents.com/file/12489212/17569905140637/eecac6/UE.Documentation/PaperZD/image/4-2.png)

The **Timeline** should look like this:

![4-3](https://drfs.ctcontents.com/file/12489212/17569905140639/543630/UE.Documentation/PaperZD/image/4-3.png)

Normally, just setting the notify and correctly configuring it would be enough, but **SourceNotifies** need to be implemented by the **AnimationBlueprints** that derive from this Source.

If we go back to our **AnimationBlueprint**, by simply clicking the top-right selector hotlink, we will see that the **AnimationBlueprint** has automatically created a new function for us.

![4-4](https://drfs.ctcontents.com/file/12489212/17569905140641/e1b124/UE.Documentation/PaperZD/image/4-4.png =450x)

**SourceNotify** Functions will automatically be created on every child **AnimationBlueprint** and they will all share the same naming convention: Receive Notify: .
We’ll simply double click on this function and put some really simple behavior for now.

![4-5](https://drfs.ctcontents.com/file/12489212/17569905140642/420fd3/UE.Documentation/PaperZD/image/4-5.png)

If we compile and play, we should be able to see the behavior being triggered at the correct times.

![4-6](https://drfs.ctcontents.com/file/12489212/17569905140643/e1530a/UE.Documentation/PaperZD/image/4-6.png)






# 05. Customizing output of the Animation Graph

The **Animation Graph** comes with a set of **Animation Nodes**, that tell the system which animation it should use at a specific time. In previous chapters we’ve used the `Select Animation by Bool` to select between two outputs depending on a boolean value.
An important note is that the **Animation Pin** that goes inside the node (drawn like a papercut character), can be the output of any Animation node, these nodes can be concatenated to create complex behavior and switch between nodes.
An example follows:

![5-1](https://drfs.ctcontents.com/file/12489212/17569905141565/5197c9/UE.Documentation/PaperZD/image/5-1.png)

The **output of an Animation node** can only be connected **to a single animation pin**, this is to ensure that the **Update** sequence runs as intended and doesn’t trigger multiple times. If you want to use the output of the same node multiple times (like using a state machine in different places), you’ll need to use the `CacheAnimation` node as follows.

![5-2](https://drfs.ctcontents.com/file/12489212/17569905141545/a7f5ae/UE.Documentation/PaperZD/image/5-2.png)

![5-3](https://drfs.ctcontents.com/file/12489212/17569905141549/f7408b/UE.Documentation/PaperZD/image/5-3.png)

This example is illustrative, the same output can be used as many times as needed, as we are only obtaining the cached version of it and only updating it once.
Another point of interest is that each Animation node has settings that can be changed to modify how they behave, let’s take the `Play Idle` node as an example.

![5-4](https://drfs.ctcontents.com/file/12489212/17569905141551/35ee85/UE.Documentation/PaperZD/image/5-4.png)

You can see that you can actually change different values like the **PlayRate**, whether to start the animation from the beginning or another playback position and even if we should loop the animation. This variables can actually be made **dynamic** by selecting on the checkbox that is to the right, which will create a new PIN on the node, which you can then bind to dynamic variables and logic into the **Animation Graph**

![5-5](https://drfs.ctcontents.com/file/12489212/17569905141552/a992ee/UE.Documentation/PaperZD/image/5-5.png)

![5-6](https://drfs.ctcontents.com/file/12489212/17569905141560/1bfeb8/UE.Documentation/PaperZD/image/5-6.png)

In this specific case, we could change how quick the **Idle** animation would play, and make it run slower if the character is tired or even pause it entirely.
There are multiple nodes and settings, to ensure the animations work as intended with the least amount of code as possible.



# 06. The state machine

In previous chapters we created a very simple 2 state animation character that simply switched between animations depending on its velocity. While this was easy to follow, it doesn’t scale well and the more the character increases in size, the more complex the animation logic becomes.

For these instances, PaperZD comes with a custom `StateMachine` node that is optimized for 2D usage. Let’s improve our walking character and migrate the logic into a new state machine.

First, we’ll create a new `StateMachine` node on the **Animation Graph**, we’ll call it **Locomotion** for the purpose of this example.

![6-1](https://drfs.ctcontents.com/file/12489212/17569905142762/54c58d/UE.Documentation/PaperZD/image/6-1.png)

![6-2](https://drfs.ctcontents.com/file/12489212/17569905142763/030ee1/UE.Documentation/PaperZD/image/6-2.png)

>ℹ️ Note: As any other Animation Node, the `StateMachine` can be outputted to any other node and concatenated as we did in the previous chapter.

Double clicking the `StateMachine` allows us to modify its underlying graph, inside of it we’ll find a single `Out` node, that serves as the root of the state machine, from here we can start building our logic.

![6-3](https://drfs.ctcontents.com/file/12489212/17569905142765/1ca12d/UE.Documentation/PaperZD/image/6-3.png)

We will recreate the logic of the previous chapters first, for this we’ll drag from the `Out` node and put a **State**, this will be our **Idle**.

![6-4](https://drfs.ctcontents.com/file/12489212/17569905142767/c8dac3/UE.Documentation/PaperZD/image/6-4.png)

![6-5](https://drfs.ctcontents.com/file/12489212/17569905142769/8e311f/UE.Documentation/PaperZD/image/6-5.png)

Double clicking on the state we’ll bring us to the state’s internal animation graph, which works just like the root graph we’ve been using; here we’ll simply put a `Play Idle` node.

![6-6](https://drfs.ctcontents.com/file/12489212/17569905142770/4224b9/UE.Documentation/PaperZD/image/6-6.png)

After that, exiting this **Animation graph** back to the **State Machine**, we’ll drag from the edges of the state and create another State, we’ll call it **Walking**.

![6-7](https://drfs.ctcontents.com/file/12489212/17569905142749/615f35/UE.Documentation/PaperZD/image/6-7.png)

As you can see, an intermediate node has also been created, this is the Transition that will govern the logic that will trigger the state traversal from Idle to Walking. We’ll double click here and put the same logic we had on the previous example.

![6-8](https://drfs.ctcontents.com/file/12489212/17569905142750/f0664b/UE.Documentation/PaperZD/image/6-8.png)

With this the `Idle` will change onto `Walking` whenever this condition is evaluated to `true`. We’ll need another condition for going back to `Idle` again, so we’ll drag from the edges of the `Walking` node onto `Idle` and put the reverse condition, also putting a `Play Walk` node on the state’s internal **Animation graph**

![6-9](https://drfs.ctcontents.com/file/12489212/17569905142752/e73300/UE.Documentation/PaperZD/image/6-9.png)

![6-10](https://drfs.ctcontents.com/file/12489212/17569905142754/a7be88/UE.Documentation/PaperZD/image/6-10.png)

If we now press play, we can see how the state machine works exactly as the example in the previous chapters. We’ll expand on them now, but first need to modify our character and add some extra sequences.
First we’ll create the other animation sequences that we are missing, these are: `Jump`, `Fall`, `Run` and `AirTrans`.

![6-11](https://drfs.ctcontents.com/file/12489212/17569905142755/4d7f7f/UE.Documentation/PaperZD/image/6-11.png)

And we’ll add a simple `boolean` into our character, that changes an `IsRunning` variable. This action should also change the characters movement `max speed`, but we’ll leave that out as we’re only interested in rendering rather than gameplay code. Inside the character we will add a **Run mechanic** and a **Jump mechanic**.

![6-12](https://drfs.ctcontents.com/file/12489212/17569905142757/4914fd/UE.Documentation/PaperZD/image/6-12.png)

We’ll also go to our **Animation Blueprints event graph** and create a variable for our character, so we can query it when needed. We’ll use the event `OnInit` that gets called whenever the **AnimInstance** (class that this blueprint builds into) gets created.

![6-13](https://drfs.ctcontents.com/file/12489212/17569905142758/d08cf9/UE.Documentation/PaperZD/image/6-13.png)

Now, we can go into the state machine’s node Walk and change its internal animation graph to display either Walk or Run depending if the character is running

![6-14](https://drfs.ctcontents.com/file/12489212/17569905142759/1593ea/UE.Documentation/PaperZD/image/6-14.png)

If we press play now, we’ll see that the character changes behavior and alternates not only between being **Idle** or **Moving**, but it can distinguish if it should **run** or **walk**. The logic for this is tightly packed in easy to modify sections and modular, which ensures that it’s as easy to iterate as it was to create.

![6-15](https://drfs.ctcontents.com/file/12489212/17569905142761/8213f6/UE.Documentation/PaperZD/image/6-15.png)




# 07. Creating compact graphs with JumpNodes

We’ve used the state machine to drive the animations of our character in a standard way, the features we’ve seen up until now are shared with the default behavior from the state machines present on the Engine’s **AnimationBlueprints**, but there are some 2D specific features that we can use to make 2D development easier and more manageable, and to overcome hurdles that come with the nature of frame by frame animations like Paper2D flipbooks are.

We will learn about these features while continuing the development of our character, by adding a **Jump** functionality to it, first we’ll want to make sure our Character can jump, for that we’ve already added the needed code in the previous chapter.

Imagine having a very big animation state machine driving your character, and you want to add airborne animations to it. Airborne animations can happen at almost any state, and if the animation graph is big enough, this would mean you need to create a connection from every state to an Airborne state if you wanted to keep the behavior on the same graph. While it is true that there are multiple ways to implement the jumping (like creating a separate state machine for it, and then switching between them on the root animation graph), we’ll approach this by implementing the **Jump **functionality on the same graph, as a matter of example.

PaperZD comes with `JumpNodes`, which are “shortcut” nodes that you can put wherever in your state machine that can be triggered by code to force the state machine to go to said node. These nodes are useful for interrupting the state machine usual flow for things like being damaged or, like in this case, jumping or being airborne, while still keeping the logic inside the same state machine.

We’ll first create the logic of our Airborne states, for this we’ll first create a `JumpNode` called **Airborne**, the name is important as it is the identifier used for later triggering said jump.

Add a `JumpNode` by simply right clicking and adding a node as you would do a state or a conduit, then change the name to **Airborne**.

![7-1](https://drfs.ctcontents.com/file/12489212/17569905143413/9deb91/UE.Documentation/PaperZD/image/7-1.png)

![7-2](https://drfs.ctcontents.com/file/12489212/17569905143416/a26412/UE.Documentation/PaperZD/image/7-2.png)

After that we want to create the jumping logic: When starting a jump (or being airborne) we’ll first run the **Jump** animation (even though we could have just fallen), we’ll explain this after the base logic is completed.

After **Jumping**, one would start falling (after the apex completes), we’ll add the **Falling** state. Passing from **Jumping** to **Falling** happens when the upwards velocity of the fall becomes negative (meaning we are falling and not going up). Finally we reach the **grounded** states, either **Walking** or **Idle**, we’ll connect to **Walking**, as it’s more likely we’ll be moving and not perfectly still, but this could change depending on how the landing logic works in a game.

![7-3](https://drfs.ctcontents.com/file/12489212/17569905143420/5fd7da/UE.Documentation/PaperZD/image/7-3.png)

We’ll also put the **Jumping** and **Falling** animations in their respective states, making sure the `Play Animation` Nodes are set to not loop, as these animations were made with this in mind.

![7-4](https://drfs.ctcontents.com/file/12489212/17569905143423/c4b2c3/UE.Documentation/PaperZD/image/7-4.png)

Inside the link between **Jumping** to **Falling**:

![7-5](https://drfs.ctcontents.com/file/12489212/17569905143428/5fe23e/UE.Documentation/PaperZD/image/7-5.png)

And inside the link between **Falling** to **Walking**:

![7-6](https://drfs.ctcontents.com/file/12489212/17569905143430/102298/UE.Documentation/PaperZD/image/7-6.png)

By default, **AnimationBlueprints** have **Allow Transitional States** setting enabled on their class defaults. This makes the state machine continue evaluating the states until a stable one is found. In the example above, if your character **falls** (without **jumping**), the state machine will first evaluate the **Jump** state, and right away evaluate the transition and move to the **Falling** state, as the velocity is negative; the state machine would then stop there, as no other transitions are available and the **Falling state would render, the Jump state won’t.** The **Jump** state in this example becomes a **transitional state**, which helps create a more compact state machine, removing the need of extra connections by allowing the state machines to flow naturally to their stable state.

Now that the State machine is ready, we simply need to trigger the `JumpNode` by getting the **AnimInstance** that is stored on our **AnimationComponent**.As explained before the **AnimInstance** is the instance of the **AnimationBlueprint** we’re building and that it’s used to drive the animations of the component.

In our character’s blueprint we will add the following nodes, to detect **Jump** and **Falling** behavior, both of them will trigger the same `JumpNode` and the state machine will do the rest.

![7-7](https://drfs.ctcontents.com/file/12489212/17569905143435/673095/UE.Documentation/PaperZD/image/7-7.png)

If you test now, the character will have a Jump/Fall animation setup, but it will have no intermediate animation and will look very stiff.

![7-8](https://drfs.ctcontents.com/file/12489212/17569905143438/f163ed/UE.Documentation/PaperZD/image/7-8.png)





# 08. Transitional Animations

## Creating intermediate states and their drawbacks.

Skeleton animations have a distinct advantage over frame by frame animations and frameworks like Paper2D, these allow for the animations to blend naturally between states, and make sure the flow of the action never feels too cut. Frame by frame animations, because of their very nature, cannot blend naturally and the “blending” animation needs to be added as an extra asset that will play in between 2 animations.

While PaperZD has support for blending of skeletal animation sources (assuming the extensions are set up), it also has some tools to add intermediate animations in an easy manner, creating rich characters that flow extremely well, while keeping the graph side at a manageable size.

In the previous chapter, we added a Jump/Fall behavior which looked stiff because it lacked the intermediate state. We’ll first see how an intermediate animation would be added normally without the use of **Transitional Animations**, to see why adding intermediate states is not sustainable.

For reference, we will add an extra intermediate state between the **Jump** and **Fall** animation, that we will call **JumpApex**.

![8-1](https://drfs.ctcontents.com/file/12489212/17569905144026/cda3d3/UE.Documentation/PaperZD/image/8-1.png)

The link between **Jumping** to **JumpApex** is the same one as we had before, it triggers when the upwards velocity becomes negative. Inside the **JumpApex** we add the **AirTrans** animation sequence **and we make sure it’s set to non-looping**. This last point is important.

![8-2](https://drfs.ctcontents.com/file/12489212/17569905144028/e29843/UE.Documentation/PaperZD/image/8-2.png)

After that, we need to set the transition between **JumpApex** and **falling**. Because this is an intermediate animation, we want to stop playing this animation when its playback is complete, for this we have two ways of accessing that data:

1. Obtain the `TimeRemaining` node related to the Animation that we wish to check

![8-3](https://drfs.ctcontents.com/file/12489212/17569905144029/1cdbc3/UE.Documentation/PaperZD/image/8-3.png)

1. Query the **AnimPlayer** that lives inside the **AnimInstance** that this class compiles into, which will give the current time for the most important animation. This data is 1 frame late though, so be careful about using it.

![8-4](https://drfs.ctcontents.com/file/12489212/17569905144030/a30156/UE.Documentation/PaperZD/image/8-4.png)

![8-5](https://drfs.ctcontents.com/file/12489212/17569905144031/47a8b7/UE.Documentation/PaperZD/image/8-5.png)

>ℹ️ Note: It's usually prefered to use the `TimeRemaining` nodes when possible, but sometimes they are not available as they can only show transition graphs that are relevant to them. On those cases using the **AnimPlayer** is a good way to circumvent those issues.

>ℹ️ Note: It’s important that the animation is set to non-looping, as the playback progress could loop before we trigger this rule. This is especially true in low framerate environments.

>ℹ️ Note2: The **AnimationPlayer** can manage multiple animations at the same time, the Playback Progress will refer to the animation with the biggest weight that is being processed currently. This is not something that non-blendable animation sources (like Paper2D Flipbooks) need to take into consideration, as blending is not available and thus, there’s only one animation running at any time.

With this setup, we now can see how the flow of the character improves dramatically due to the intermediate animation, but this also brings a new issue. What happens if we’re in the middle of the **AirTrans** animation and suddenly hit the floor? The animation would still continue playing until it reaches the end, as per the transition rule. We need to add an extra check for being **grounded**. The animation logic starts to bloat and the more intermediate states we want to add to make a richer character, the more the graph becomes unmanageable.

We can fix this issue by replacing these intermediate states with a custom feature from PaperZD: **Transitional Animations**.
## Improving animation flow with **Transitional Animations**

As we saw in the previous chapter, adding intermediate animations is a cumbersome process that requires adding more states (and thus more logic) to an animation graph which in turn bloats it extremely quickly, making it really hard to debug and manage for even medium-sized projects. PaperZD comes with a feature that attempts to solve this: **Transitional Animations**.

**Transitional Animations** work by enabling the user to add a custom and optional animation graph on a transition node, this transitional graph runs a single loop of an animation and **cannot** have internal state machines, it’s meant to be a simple graph for filling the animation data required in between. When the single loop of the animation completes, the system will continue rendering the **state** node Animation Graph.

Transitional animations are optional in nature, and if the target state they point to stops being the active one, they stop rendering. Only the last transitional graph will run, as an “introduction” to the target state it points to.

Let’s first go back a few steps, from before we added the intermediate state.

![8-6](https://drfs.ctcontents.com/file/12489212/17569905144032/0f6504/UE.Documentation/PaperZD/image/8-6.png)

Adding a transitional graph is easy, simply click on the transition that you want to animate, in this case the `Jumping to Falling transition`, and click **Use transitional animations** on its details panel. You’ll see that the transition itself changes its icon, to reflect the change and showing that it’s being animated.

![8-7](https://drfs.ctcontents.com/file/12489212/17569905144034/57e361/UE.Documentation/PaperZD/image/8-7.png)

Now we can click on the **Open Animation Graph** button that is at the side of the checkbox we enabled. We’ll enter an animation graph. Let’s add the **AirTrans** animation sequence to it.

![8-8](https://drfs.ctcontents.com/file/12489212/17569905144035/5b45c7/UE.Documentation/PaperZD/image/8-8.png)

>ℹ️ Note: You’ll see that inside this graph, you will be unable to add state machines and set the animations to loop. Every animation in this graph is meant to have an end, and thus, looping will be disabled.

With this, the transitional animation has been correctly set up, no more code nor configuration is needed. If you run the example now, you’ll see that the character works just as before, but it now also aborts the transitional animation when touching the ground before it finishes.

Transitional animations are a key feature of PaperZD and a very powerful way to give more life and movement to a character without increasing the complexity of the animation graph.



# 09. Binding playback events

There comes times when adding animations is not enough to implement certain features of a game, and a more granular control of the playback is required. Because of this, multiple ways of managing the playback of the animations are present, to enable users to accomplish their goals as easily as possible.

As shown before, Playback rate can be managed in a “per-node” instance on the Animation Graph.

![9-1](https://drfs.ctcontents.com/file/12489212/17569905144233/0cd36b/UE.Documentation/PaperZD/image/9-1.png)

With this one could implement **climbing ladder** animations depending if the character is moving or not, like old platformers, by pausing the animation when not moving or even reversing it when going down the ladder.

Sometimes though, more global control is needed, and adding a variable on every `Play Node` wouldn’t be scalable enough, in these situations it’s more sensible to directly interface with the **AnimPlayer**.

PaperZD’s playback control is centralized via a central object that works as the **Playback manager** which ticks animation nodes, triggers notifies and calls events pertaining to the state of the animation sequences being run. You can access this object by simply calling the following node:

![9-2](https://drfs.ctcontents.com/file/12489212/17569905144234/39de3f/UE.Documentation/PaperZD/image/9-2.png)

This object comes with a set of interesting callbacks and events for managing the global state of the playback of animations.

![9-3](https://drfs.ctcontents.com/file/12489212/17569905144235/7fc97f/UE.Documentation/PaperZD/image/9-3.png)

Using this object, you can easily pause the playback, change the playback mode (forward or reversed) and query the current state of rendering (knowing the time of the current animation, which animation is playing, etc).

>ℹ️ Note: Certain calls like the current animation or animation time, only take into account the biggest weighted animation on the mix. This is only important on skeleton and blending enabled animation sources, as those can mix multiple animations into a final blend.

The **AnimPlayer** also comes with a set of events that get automatically called during playback:

![9-4](https://drfs.ctcontents.com/file/12489212/17569905144237/8645b0/UE.Documentation/PaperZD/image/9-4.png)

Two of these are already implemented by the **AnimationInstance** and can be overridden directly: **Playback Complete** 

![9-5](https://drfs.ctcontents.com/file/12489212/17569905144239/37eb8f/UE.Documentation/PaperZD/image/9-5.png)

These events (specifically **Playback Complete**) are how the **Transitional Graphs** detect that the transitional animation has finished its playback. You can bind or implement them to be notified when the rendering of an animation changes.



# 10. Adding support for Multi‐Directional Games (Top‐down, Isometric, etc)

Up until this point we’ve shown how to animate a sidescroller game. These kinds of games usually just have 2 views (left/right) and will realistically only use one animation per “state” of the player, the reversed direction usually is mirrored. In practice this means that for every one action, there’s only one “animation” that needs to be rendered.

Obviously this isn’t always the case, and other types of 2D games exist that in fact, require multiple animations for different directions of the same action.

Games like top-down, isometric, boomer-shooters heavily rely on simulating a fake 3d animation by drawing the same animation multiple times in different angles, and then placing them depending on an angle variable. This already presents a big challenge in the art department by increasing the work by 4 or even 8 times, depending on how many directions are being shown. Obviously when implementing the animations, we would rather streamline the process of adding directionality as much as possible.

Usually the answer that engines and tools present to users that need to add directionality to their sprite, is either force them to create “Blend Spaces” and setup every animation on a graph, or forcing them to change the animations by code; both of which require too much time to configure and increases the amount of time the user would stay in this section, thus increasing the cost of these projects.

PaperZD streamlines this step as much as possible and presents a powerful feature that can help in these situations: **Multi-Directional Sequence**s.

Because AnimSequences are an abstraction of an animation to play, and strictly speaking, every rotation of an animation still is the same animation (for the game’s purposes), we added the ability to add “rotations” inside a sequence.

As an example, we will go into our Animation Source and create a new Animation Sequence, we will call it **“MyDirectionalSequence”**.

![10-1](https://drfs.ctcontents.com/file/12489212/17569905144402/c6d357/UE.Documentation/PaperZD/image/10-1.png)

If we open this new Sequence and see its details on the right, we will see a checkbox that says “Multi-Directional Sequence”

![10-2](https://drfs.ctcontents.com/file/12489212/17569905144403/f7579a/UE.Documentation/PaperZD/image/10-2.png)

Clicking on it will show the directionality widget, used for setting up the directional information of the AnimSequence

![10-3](https://drfs.ctcontents.com/file/12489212/17569905144405/21e3e7/UE.Documentation/PaperZD/image/10-3.png)

You’ll see that this new widget contains 4 corners, and the bottom “Animation Data” has been replaced by “Animation Node 0”. The golden section is the currently selected direction, clicking on another square will select that position

![10-4](https://drfs.ctcontents.com/file/12489212/17569905144406/794b59/UE.Documentation/PaperZD/image/10-4.png)

We can also modify the number of directions and even the offset angle of them

![10-5](https://drfs.ctcontents.com/file/12489212/17569905144410/20e16f/UE.Documentation/PaperZD/image/10-5.png)

Because the example project doesn’t contain a top-down animation, we’ll use the flipbooks we already have for demonstration purposes. Set the directionality to N = 4, and fill the directions with different animations.

![10-6](https://drfs.ctcontents.com/file/12489212/17569905144412/b8110d/UE.Documentation/PaperZD/image/10-6.png)

![10-7](https://drfs.ctcontents.com/file/12489212/17569905144397/67288f/UE.Documentation/PaperZD/image/10-7.png)

### Note about directions:

>ℹ️ Directional sequences are meant to represent the same animation, and thus, assumes that all of the sequences have the same length. The length of the sequence will be inferred by the first node (Node 0).

If we focus on the widget and hold the shift key we can move around the preview node that will display which animation would be displayed depending on the angle we’re inputting to the sequence.

![10-8](https://drfs.ctcontents.com/file/12489212/17569905144399/f38948/UE.Documentation/PaperZD/image/10-8.png)

In this case, even if the selected node is the right one (Jump), we’re displaying the top one (Fall).

The resulting AnimSequence can then be used, the notifies will be triggered as normal and the flipbook to use for rendering this animation will change depending on a directional input that we must provide.

Going back to the **Animation Blueprint**, we will now let the **Animation Graph** know which direction we want the system to render the animation (and thus, which flipbook will be displayed). For this we simply concatenate a **“Set Directionality”** node, that will set up the correct metadata for the correct animation data to be evaluated.

We will use the character’s velocity as the directionality input to the node, this is a **FVector2D **that will mirror the **green preview node**. This input can be whatever your project needs, for example:
- The angle difference between the camera and the forward direction of an enemy (for boomer shooters).
- The actor forward vector for top-down games.
- Last non-zero velocity.
- Or even be a value to be used for simply rendering different animations (like sprites having different states that change), it can be non-directional information (although the system is made with directionality in mind to give users an easier time creating those games).

![10-9](https://drfs.ctcontents.com/file/12489212/17569905144400/2acdb9/UE.Documentation/PaperZD/image/10-9.png)

>Note: Changing directions won’t reset the playback time of the animation.

With this we have added directionality to our character. The directionality information is made per-character and not per-animation which ensures that set-up is fast. The system will not fail if a non-directional sprite is presented even if directionality is set.

This way of implementing the characters should be fast to iterate, simply add as many directional animations as needed, then feed the angle in which you are “rendering” the animation, the system will select the correct animation to render for you.




# 11. Using Animation Overrides.

## When not to use JumpNodes

When creating your animation flow, sometimes you wish to override the behavior of the animation graph with your own 'playback' of animations, for example when doing 'attacks' and other single shot states. While**JumpNodes** can certainly help modify the behavior of the StateMachine itself, the changes are not **Fire and Forget** and after firing them, the previous state will be lost. This is desirable when coding things like reacting to damage, or forcing changes in states that you wish for them to stay, but sometimes you wish for an animation to play once and then continue the previous behavior, like for example 'attack animations'. **Jump Nodes** are not the correct tool for this, and instead you should use **Animation Overrides**

## Animation Overrides and Override Slots

An animation override, simply put, is a **fire and forget** animation that can be triggered which, after finished, will go continue the normal animation flow. **Animation Overrides** just like **Jump Nodes** are called through code, but they can receive any compatible AnimationSequence to be played on the AnimInstance. Two methods exist for calling this functionality, only difference are the callbacks that can be received on them.

![11-1](https://drfs.ctcontents.com/file/12489212/17569905144530/c90f11/UE.Documentation/PaperZD/image/11-1.png)

To be able to use overrides, first we need to let the AnimationBlueprint where in the graph should this animation be overriding the AnimGraph logic, we do that by creating an **Override Slot** node.

![11-2](https://drfs.ctcontents.com/file/12489212/17569905144531/79ffc6/UE.Documentation/PaperZD/image/11-2.png)

![11-3](https://drfs.ctcontents.com/file/12489212/17569905144532/c057c9/UE.Documentation/PaperZD/image/11-3.png)

This will create a new **Override Slot** node that can be called directly by code.

>ℹ️ When adding an **Override Slot**, any nodes to the left of this one will not affect the final animation state when the override is active, this means that for **Directional Sequences** its usually best to leave the direction nodes to the right, so even when the override is active, the animation directions are correctly set.


# 12. Creating Custom Animation Notifies

You can create your own notifies by inheriting from **PaperZDAnimNotify** and **PaperZDAnimNotifyState**. These notifies can be shared through different animation sources and are a great place to build common functionality.

![12-1](https://drfs.ctcontents.com/file/12489212/17569905144554/2d8769/UE.Documentation/PaperZD/image/12-1.png)


If we create a notify called “MyNotify”, and then going to the Animation Timeline, we’re able to see that it exists.

![12-2](https://drfs.ctcontents.com/file/12489212/17569905144555/62fe76/UE.Documentation/PaperZD/image/12-2.png)

These blueprint objects will work as any normal blueprint, but will have access to a method **“OnReceiveNotify”**, and Start/End events when it’s a **State Notify**.

![12-3](https://drfs.ctcontents.com/file/12489212/17569905144556/fdcaa6/UE.Documentation/PaperZD/image/12-3.png)

Adding public variables on this blueprint, will enable you to modify their values directly on the timeline.

![12-4](https://drfs.ctcontents.com/file/12489212/17569905144558/96445d/UE.Documentation/PaperZD/image/12-4.png)

![12-5](https://drfs.ctcontents.com/file/12489212/17569905144559/f3585e/UE.Documentation/PaperZD/image/12-5.png)

When implementing the notify events, a member called “OwningInstance” will be given to you, this is the AnimInstance that is calling this notify. This object can be null if called from the editor.

![12-6](https://drfs.ctcontents.com/file/12489212/17569905144560/a791ed/UE.Documentation/PaperZD/image/12-6.png)

If needed, you can obtain the **“Render Component”**, that is used to render the animations, which you can use to add sounds, particles, or even debug boxes for displaying.

![12-7](https://drfs.ctcontents.com/file/12489212/17569905144561/c8c4d1/UE.Documentation/PaperZD/image/12-7.png)

In the following example, we use the Render component location to identify where a sound should be played while playing the game. In Editor logic we draw a debug box, that will only be displayed during the Animation Source Editor window. This can be useful for creating “Hitboxes” that are drawn on the editor window (so we can adjust them as needed), while the game-behavior would actually apply the Hitbox without showing it.

![12-8](https://drfs.ctcontents.com/file/12489212/17569905144562/b0f214/UE.Documentation/PaperZD/image/12-8.png)

![12-9](https://drfs.ctcontents.com/file/12489212/17569905144565/dfa3bd/UE.Documentation/PaperZD/image/12-9.png)






# 13. Applying skins to your characters

With the new v2.2 version, PaperZD now allows you to apply 'skins' to your characters, enabling you to quickly change how they look while maintaining the same animation logic that has been previously coded.

Changing skins is the perfect way to:
- Change attires of your character.
- Modify equipment or body-parts on composite sprites.
- Create a master AnimationBlueprint for enemies that behave the same, and just changing the way they look.

You can create a skin by right-clicking the **Animation Source** that will be affected by the asset and clicking the "Create AnimationSkin Asset"

![13-1](https://drfs.ctcontents.com/file/12489212/17569905144841/145eee/UE.Documentation/PaperZD/image/13-1.png)

This will now create a new **AnimationSkin** file. This file is responsible of mapping the animation played to the custom flipbook you want to render instead, allowing you to change how to display your character in runtime, without having to modify your animation logic.

The file comes pre-filled with all the animations that the source currently has registered, enabling you to quickly map the animation to another flipbook instead.

If at any point you add more animations to your source, you can quickly sync the skin file to it to add any remaining animation that isn't mapped.

![13-2](https://drfs.ctcontents.com/file/12489212/17569905144843/0eb548/UE.Documentation/PaperZD/image/13-2.png)

For multi-directional characters, the array of animations matches the 'Animation Node X' name when selecting the direction on the editor, and starts on the topmost node, moving in a clockwise manner.

![13-3](https://drfs.ctcontents.com/file/12489212/17569905144832/6e482b/UE.Documentation/PaperZD/image/13-3.png)

To apply the skin you simply call the following node, specifying the asset to use.

![13-4](https://drfs.ctcontents.com/file/12489212/17569905144836/a83b0f/UE.Documentation/PaperZD/image/13-4.png)

The "Layer" argument is used when creating composite animations, which will be seen on the next chapter. For now simply use "Layer 0" which points to the main render component.




# 14. Creating composite animations using layers

## Building a composite Animation Source

Since version v2.2 it is now possible to create composite animation sequences to render layered characters.

![14-1](https://drfs.ctcontents.com/file/12489212/17569905144937/cb7bb5/UE.Documentation/PaperZD/image/14-1.png)

To enable this behavior, navigate to the bottom left part of the AnimationSource editor and locate the new **"Layers"** section.

![14-2](https://drfs.ctcontents.com/file/12489212/17569905144939/c44c7b/UE.Documentation/PaperZD/image/14-2.png)

In here, you can configure your **Animation Source** to allow for extra composite layers for rendering additional animations that are synched with your main render component. In this example we have added a total of 6 additional layers to be rendered alongside the character and have given them the correct offset so they can render on top of each other.

## Setting up the animations per layer

Now, we can go into the details of any **AnimSequence** and setup the base body animation (now called "Animation"), and the extra composite layers to be rendered on top

![14-3](https://drfs.ctcontents.com/file/12489212/17569905144940/2e5c41/UE.Documentation/PaperZD/image/14-3.png)

This enables us to create an animation piece by piece and even apply skins to each body part separately to create a fully customizable character which can be used as basis for a 'character customization' like mechanic on games.

>ℹ️ Its important to note that each animation layer has been created to perfectly match in size and duration to the main body part, so effectively they run in tandem overlayed over each other, creating the composite animation.

## Setting up your character to use the layered animations.

By default, you don't need to do much to set up your character to use the layers you created, as the tool will automatically create the components required to render the animations and layer them correctly, this is because we have the following option enabled by default on the **Animation Component**.

![14-4](https://drfs.ctcontents.com/file/12489212/17569905144942/883926/UE.Documentation/PaperZD/image/14-4.png)

Letting the plugin build the layers by itself is the easiest way to get started with your composite character, but this have some caveats:

- The layers are only built when the game starts, which could have some adverse effects on performance if too many characters are loaded at the same time.
- Because the layers are built on **Begin Play**, these layers are not visible while not playing on the editor.
- You don't have easy access to them to modify the render components in any way.

Because of this, its recommended that instead, you manually create the layers on your blueprint and assign them to the composite layers array, to let the tool know how to render them. This also makes it easy to visualize the character in the editor

![14-5](https://drfs.ctcontents.com/file/12489212/17569905144944/432a8b/UE.Documentation/PaperZD/image/14-5.png)

![14-6](https://drfs.ctcontents.com/file/12489212/17569905144945/947de7/UE.Documentation/PaperZD/image/14-6.png)

Now you have a fully functioning layered character that works just as a normal non-layered character. We can even apply skins directly to each layer by modifying the target layer that the skin asset points to.

![14-7](https://drfs.ctcontents.com/file/12489212/17569905144947/9d0754/UE.Documentation/PaperZD/image/14-7.png)

**Note:**
>ℹ️ When adding skins to layers, remember that **Layer 0** is your main render component, and **Layer 1** and forward are your composite components, so in this case we're applying a skin to the "Lower" body part.






















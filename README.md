# This is a compilation of resources and processes that can help you learn or start modding 

### Modding Wiki: https://risk-of-thunder.github.io/R2Wiki/ 

This wiki contains information and other resources to aid with modding Risk of Rain 2. Generally speaking, the modding wiki has most of the resources you need for modding. Information is a bit scattered and some pages are deprecated so here are some more organized links for character modding.

### Henry Template by rob and TimeSweeper:  https://github.com/ArcPh1r3/HenryTutorial

This template has all the instructions you need to start. Simply download the zip file and follow along.

### Where to start with C#: https://youtu.be/GhQdlIFylQ8?si=G6uarRTXL7Tydp8B

Don't know anything about coding or C#? Start with the video above.

### Unity Documentation: https://docs.unity3d.com/2019.4/Documentation/Manual/UnityManual.html

### Unity Scripting: https://docs.unity3d.com/2019.4/Documentation/Manual/ScriptingSection.html

# Ok great, you know basic fundamentals… Uh, what now?? 

It's time to do some reading and experimenting. Download ILspy or dnSpy to look into other mods or go look at public GitHub repos posted on character Thunderstore pages.

## How to use ILSpy: 
1. Install ILSpy https://github.com/icsharpcode/ILSpy
2. Download a mod that you want to view the code of on r2modman. EX: https://thunderstore.io/package/Paladin_Alliance/PaladinMod/
3. Browse your profile folder from r2modman and go to the folder of the mod.
4. Copy the path of the folder and in dnSpy click the file and open and paste the path at the top of the file explorer popup to go to it.
5. Double-click the .dll file in the folder.
6. Done, now you have access to all the decompiled code for the mod. If the mod creator has organized their code, you should be able to figure out what does what.
7. Bonus: Download the code as a csproj for easy access

I highly recommend doing this for ROR2s code as well then downloading the decompiled code as a csproj for easy access.

Now go experiment on the Henry template!

# After messing around

You’ve messed around with Henry and maybe made some basic abilities that you're satisfied with, now you want to take the extra step of making it look nice. It’s time to use some addressables.

These are links to in-game addressable references such as sounds and assets that ror2 uses:

https://xiaoxiao921.github.io/GithubActionCacheTest/assetPathsDump.html

https://heyimmodding.github.io/WwiseRor2EventsWIkiPage/Eventviewer

Great, now wtf do we do with these? Here's a quick example. In your Henry project, there should be a dedicated area where you organize the loading of your assets. Let's say you want to make a slash that looks like mercenaries. What you would do is first, create a static GameObject to store this asset into:

![step 1](/images/1.png)

Then load the asset and give it a name. In this example, we are loading "RoR2/Base/Merc/MercSwordFinisherSlash.prefab" which we found in the addressable references, and the slash we are cloning is named "SeamstressSlash":

![step 2](/images/2.png)

Now all we have to do is instantiate the effect in some way:

![step 3](/images/3.png)

Some effects will need you to create an effect def(CreateAndAddEffectDef() given to you in Henry) (Slash example does not need this):

![step 4](/images/4.png)

To play addressable sounds in the most basic fashion, all you need to do is reference the sound string and the game object you want it to play from:

![step 5](/images/5.png)

# Thunderkit Addressable

Let's say you want to either preview or edit effects to fit your character better. The best way to see how you can mess around with base game assets is with Thunderkit. Here is the most simple guide for setting up a Unity project with it. All you need is the addressable browser for this:

https://youtu.be/PfygcNDlDuI

Once thunderkit is set up, I highly recommend installing SimplyAddress into your thunderkit project as well. This will allow you to clone prefabs and edit them as your own in the project. EDITED PREFABS ARE NOT SOMETHING YOU CAN DIRECTLY USE IN HENRY TEMPLATE. THIS WILL BE DONE IN YOUR C# PROJECT:

https://github.com/PassivePicasso/SimplyAddress

So you have the addressable browser. Now you can directly access the prefabs you were referencing earlier with the addressable website:

![step 2.1](/images/2.1.png)

Let's go back to the slashEffect example from before. Find where the prefab is and inspect it:

Sick! We can look at the children of MercSwordFinisherSlash now. It's even cool to see Hopoo games made SwingDistortion? since they had no clue wtf they were doing! Anyways, let's clone this thing so we can change the values. All you need to do is create an empty game object on the left and add an addressable prefab component. Then, paste the address of your prefab and check replace self. If it doesn't load immediately just hit random buttons or delete any previous game objects parented to it:

Now all the component values can be edited to experiment with any changes. Since this is a fully editable prefab now, you can also clone other prefabs and Frankenstein them together to experiment with any desired outcome:

Now, how do we get our edited prefab in the game? We need to make sure to follow the steps we took to edit our prefab in code. In this example, all I want to do is change the size of the SwingTrail. Back in C#, all we do is load the slash effect as normal and then change the values after we clone it:

Bonus tips for changing certain colors or materials. 
To change a material, make sure to reference the Renderer and not the direct component:

When changing the color of a material, avoid changing it directly as this changes the material for every asset using it. Instead, edit the main material of the renderer directly 
In this example, I instantiate a material and change the mainColor ("_Color") and the emissionColor ("_EmColor")  (tintColor is "_TintColor"):

For some renderers, they require an array so you will need to create a new one to set them:

To edit materials in a System directly is a bit more annoying. First, create a var. Then set it to the Systems.main. You can now edit values in it directly:

# Modeling

Let's say you want to make your model. There aren't any specific guides for it but here are some resources and tips:

I recommend following this video to get a basic idea of controls and ways to block out your model:

Try your best to make as few vertices as possible. Basically, it's ok if your character has the roughness of a ps2 character, you can smooth it out by using shade smooth and hopoos shaders will carry. I didn't know this and my first model now has god-awful topology.

## Here's a good place to start: 

https://www.youtube.com/watch?v=IhIGVO4fqLg

## Now, look up a basic guide on what rigging is and how to do it. Here's a random ass video I found:

https://www.youtube.com/watch?v=9dZjcFW3BRY&t=513s

## Henry Tutorial Rigging Guide:
https://github.com/ArcPh1r3/HenryTutorial/wiki/Survivor-Model-Tips#rigging

For animating make sure any walking or sprinting animations have the same leg touch the floor first. I was told this is important so I assume you should follow it too. 
If you've made it this far, I don't think you need anymore guides. Use google and that noggin of yours to figure anything else out.

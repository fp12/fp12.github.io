---
layout: post
title: "Unreal Fest Online | Part 1: Next-Gen & Abilities"
tags: ue4 ue5 gas
date: 2020-07-20
image: /assets/images/UnrealFestOnlineBanner.jpg
---
![Unreal Fest Online](/assets/images/UnrealFestOnlineBanner.jpg)  

_This is the first part of a blog series in which I review sessions from the **Unreal Fest Online 2020**.  
This article focuses on **Unreal Engine for Next-Gen Games** & **Benefits and Pitfalls of Using Gameplay Abilities System**._

<!--more-->
---
Let's start this blog series with two very interesting sessions.  
First, we'll have a look at what Epic has in store for the Next-Gen, which is always exciting, then we'll dive into one of their system, the Gameplay Ability System.


## Unreal Engine for Next-Gen Games

> In this presentation, Nick Penwarden, VP of Engineering, and Marcus Wassmer, Engineering Director, cover the features in Unreal Engine that will be crucial to the success of developing the next generation of games and reveal innovative features being developed that will revolutionize game development.
> Then, Jerome Platteaux, Art Director, provides an in-depth look at how Epic created the "Lumen in the Land of Nanite" UE5 demo.

If you missed the Unreal Engine 5 reveal and the demo running on Playstation 5, you can watch it here, it's pretty impressive:

<iframe width="560" height="315" src="https://www.youtube.com/embed/qC5KtatMcUw" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

This Unreal Fest Online session starts with the tools and processes they aim to leverage for this next generation of consoles. For instance, they expect game developers to build content once and deploy to all platforms. For this, productions can already (and will continue to) rely on existing scalability systems such as the custom [device settings](https://docs.unrealengine.com/en-US/Engine/Performance/Scalability/ScalabilityReference/index.html), [dynamic resolution](https://docs.unrealengine.com/en-US/Engine/Rendering/DynamicResolution/index.html) for rendering, or the [Significance Manager](https://docs.unrealengine.com/en-US/Engine/Performance/SignificanceManager/index.html) for gameplay needs.

The Next-Gen hardware will drive optimizations in specific areas (I/O mainly), as well as the development of current in-house features such as [Landscape](https://docs.unrealengine.com/en-US/Engine/Landscape/Creation/index.html), [Niagara](https://docs.unrealengine.com/en-US/Engine/Niagara/index.html), [Chaos](https://docs.unrealengine.com/en-US/Engine/Chaos/ChaosDestruction/index.html), and [Insights](https://docs.unrealengine.com/en-US/Engine/Performance/UnrealInsights/index.html). While 4.25 already shipped with Next-Gen support, a 4.25+ version is already anticipated with console-specific updates and bugfixes. During Fall, 4.26 should go out with new features, like any major UE4 version. Is it worth noting that Fortnite will ship on Next-Gen consoles as soon as they go out. While it isn't that surprising, it is still comforting to know that the engine you use can ship such a game so quickly.

As you may have seen in the previous video, two new systems are pushing Epic's vision for Unreal Engine 5: **Nanite** and **Lumen**.  
Nanite allows artists to directly import from high-quality assets (ZBrush...) and looks impressive. It runs mostly on the GPU and works for rigid geometry (which can move). Later they want to support translucent and masked materials, as well as non-rigid deformation and skeletal animations. Note that this system should not be used for grass, leaves, or hair, but you can combine Nanite and non-Nanite objects in a scene.  
Lumen is a full dynamic global illumination system with no baking nor lightmaps UV. Note that for now it may run at 30 fps in the demo but the target is to reach 60 fps.

To me, Nanite seems like a real game-changer for production workflows and I can't wait to get my hands on it in early 2021!

Regarding the other features:
*   Quixel should have more *things* coming soon...
*   Niagara's update in 4.25 brings a cleaner UI, more stability and performance
*   Hair simulation will be improved
*   For animations, ControlRig will bring a per-bone procedural control, and a full-body IK-solver should ship in 4.26, while locomotion warping should join the host of new features in UE5
*   UE5 will deprecate PhysX in favor of Epic's new physics engine, Chaos! They aim to have networked physics in UE5 as well.
*   Audio Mixer, Epic's new sound engine, boasts a consistent multi-platform audio rendering. Note that it is now the default audio solution starting from UE4.24 and it shipped in Fortnite already. They are working on *Convolution Reverb* and on *Soundfield Ambisonic Rendering*. I'll have to look those up because I'm not sure I understood them correctly :)
*   World editing will be improved with water, roads and clouds tools in 4.26
*   In UE5, they will add:
    *   automatic grid-based editing and streaming management
    *   automatic HLOD and imposter generation
    *   better cooperation on large teams with a single file per actor. This is great news as it will improve the designers and gameplay programmers' workflows!
    *   Foundations, a new feature to reuse sub-levels

One of the greatest challenges of the next generation of productions will be the management of huge amounts of raw data that will be used as in-game assets thanks to Nanite. To help alleviate this burden, Epic plans to improve cook times, but most importantly, to provide **asset virtualization** for large projects: the project sync will only fetch assets metadata, while the real data is stored on efficient shared storage. Then this *real* data would only be loaded on-demand by the editor. I think this is pretty much mandatory to ship with Nanite otherwise it won't be easily accepted by productions.

Unreal Engine 5 will not be a hard break from UE4. The same tools and plugins we know and use will still be there, and the engine philosophy stays globally the same. Once again, to prove their engine is ready, Epic will ship Fortnite on UE5 before its final release!

<iframe width="560" height="315" src="https://www.youtube.com/embed/roMYi7BU1YY" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


## Benefits and Pitfalls of Using Gameplay Abilities Framework

> The Gameplay Ability System in Unreal Engine helps to deliver gameplay functionalities and quickly iterate over prototypes. It is a robust framework, but many developers rely on trial and error to use it effectively within gameplay architecture.
> In this talk, Mateusz Buda will share his experience of using the Gameplay Ability System in ongoing projects, and how Flying Wild Hog uses almost all elements of the framework across all of its gameplay systems. He'll also show examples of challenges that Flying Wild Hog was able to overcome in very little time using Gameplay Tags and Abilities and talk about the studio’s learnings along the way.

The Gameplay Ability System (or GAS) is one of my favorite systems in Unreal. It is a clean and integrated framework that will help you deliver complex gameplay features while saving you a lot of production time. However, it is huge and I'm always looking forward to watching people talk about it because I'm sure I will learn something new about it! And this talk is no exception! :)

Let's start with the benefits, enumerated by the speaker:
*   it can efficiently replace a state machine (thanks to Gameplay Tags mostly)
*   it eases AI development by allowing to define a single BT task to execute an ability, which can be reused in multiple Behaviour Trees
*   debug is easy with quite exhaustive tools
*   Gameplay Abilities/Effects/Events can be adapted to most gameplay concepts (actions, buffs...)
*   lots of callbacks from the system can help drive your UI, etc.

Now for the pitfalls...
*   since it must be used mostly from c++, GAS suffers from a weak Blueprint exposure
*   the default recommended method for binding inputs is somewhat limited: they must be defined in c++, and you can only bind 1 input by ability... To help with that, they managed input handling in the controller, then triggered abilities explicitly (by tag) from the Blueprint. It allowed for more complex inputs, as well as using a delay to queue abilities. I think it's an interesting idea, and I'll probably try to prototype something similar...
*   they enjoyed the Gameplay Cues so much (and I have to admit it's a great system) that they started using it for non-ability related VFX. It turned out it was not that convenient for multiple reasons: few parameters are exposed and it is not easily extensible. Also, they used the GameplayCueTranslators which seemed to be a good idea but ended up being a hard to maintain list... Note that it's the first time I hear about Translators, so be prepared for a blog post about those!
*   Gameplay Abilities can only manage one animation montage at a time even with different slots, so it can be a limitation in some cases
*   they had a "minimalist" setup consisting of two instant Gameplay Effect to add Abilities and initialize Attributes. While it was practical, it became inconvenient in the long run. I would like to investigate more on this topic because there are so many ways to add Abilities and init Attributes, you have to find what fits best with your project and anticipate what will scale during production.

In a later section, the speaker detailed how they used both an Attribute for the character Health and the `TakeDamage` mechanism, an important piece of Unreal Engine's actors. They used the DamageType to carry the Gameplay Effect definition (that will modify the Health Attribute with the help of a SetByCaller) and Gameplay Event tags to trigger reactions. On a personal project, I didn't care about the `TakeDamage` but I agree it's convenient so I'll probably try a similar solution soon...  

One system that worked out well for them was to queue inputs while an ability is active, to be able to try again later automatically. I think it's a nice system that can be useful for combos as an input buffer, but it depends on your gameplay.  

They also managed to tie in automatic ammunition reload right at the end of abilities, by having a component listening on the Ammo Attribute change and queuing the reload Ability.

They consider that the GAS allowed for a faster development without compromising on quality (fewer bugs). They mentioned that they could build simple functional tests, but they didn't expand on the topic, and I'm really curious to know more about this now!  

However, the mistakes they made were mainly due to the low understanding of the whole system, which led them to create (useless?) workarounds. One could look at the source of the plugin, but it's a daunting task. Unfortunately, this is not the first time I hear about this issue... I wish Epic could provide better and more complete documentation of the system. Right now, the best resource is a community effort from Tranek on [GitHub](https://github.com/tranek/GASDocumentation).  

They also over-relied on Blueprints for Abilities while they could have simplified them with curated Gameplay Tasks. I cannot blame them as I fell in the same trap once... :)

All in all, I enjoyed watching this session. Now I have even more topics to dig and present in further blog posts! :)

_The video is not yet available. I will update this post once Epic publishes the video on YouTube_

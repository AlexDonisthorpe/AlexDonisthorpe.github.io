---
layout: post
title: The Escape
subtitle: Solo Project
cover-img: /assets/img/EscapeBanner.png
thumbnail-img: /assets/img/EscapeLogo.png
share-img: /assets/img/EscapeLogo.png
tags: [puzzle, ue4, 3d, unreal, C++]
---

"The Escape" was my first Unreal project that I set out to complete a minimal viable product for. I wanted a small project I could sink my teeth into, in order to learn the Unreal engine, having primarily focused most of my development time in the Unity engine. "The Escape" is a single player first-person puzzle game where the player needs to escape the ancient egyptian style tomb by solving the puzzles and revealing the way out.

"The Escape" is available to play, [here](https://alixxir.itch.io/the-escape)!

| Genre | Team Size | Project Duration | Engine |
| :---- |:--------- | :----------- | :----- |
| Puzzle | Solo | Two Weeks | Unreal/C++ |

<hr class="medium">
<iframe width="560" height="315" src="https://www.youtube.com/embed/hENlxnZeH9A" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen class="mx-auto d-block"></iframe>

### Objectives

For this project I primarily wanted to just play around with unreal and figure out it's systems, but I also had a few other goals in mind:

* Try my hand at the Unreal Blueprint system
* Develop a basic understanding of the difference between Unreal C++ and the C++ 11 Standard
* Figure out how source control works with Unreal projects
* Investigate the Unreal engine and learn the differences compared to Unity.

<img class="mx-auto d-block" src="/assets/img/Escape1.png" style="border-radius: 50%;" />

### Personal Highlights

![Highlight](/assets/img/EscapeHighlight.gif){: .mx-auto.d-block :}

#### Object Highlight

I wanted to try an object highlight effect often used in games with interactable objects, this would also make it clear which objects were able to be picked up during gameplay. 

For the most part this is just a simple line trace (or raycast), then calling a method on the object I hit to change the material render depth (from a material I found [here](http://www.michalorzelek.com/blog/tutorial-creating-outline-effect-around-objects/)).
```cpp
FHitResult UGrabber::GetPhysicsBodyInRange() const
{
	FHitResult Hit;
	const FCollisionQueryParams TraceParams(FName(TEXT("")), false, GetOwner());

	GetWorld()->LineTraceSingleByObjectType(OUT Hit, GetPlayerLocation(), GetRayEndpoint(), FCollisionObjectQueryParams(ECollisionChannel::ECC_PhysicsBody), TraceParams);

	return Hit;
}

void UGrabber::CheckForObjectHighlight()
{
	FHitResult Hit = GetPhysicsBodyInRange();

	// If the Line Trace hits a physics object
	if(Hit.GetActor())
	{
		// If it's the same object we've already got highlighted, then return
		if(Hit.GetActor() == HighlightedObject){ return; }
		
		HighlightedObject = Hit.GetActor();

		// Set Render Depth for the highlight material to activate
		auto ActorArray = HighlightedObject->FindComponentByClass<UPrimitiveComponent>();
		ActorArray[0].SetRenderCustomDepth(true);
	} else
	{
		if(HighlightedObject)
		{
			// Turn off the highlight on the object if we're no longer hovering over it
			auto ActorArray = HighlightedObject->FindComponentByClass<UPrimitiveComponent>();
			ActorArray[0].SetRenderCustomDepth(false);

			HighlightedObject = nullptr;
		}
	}
}
```


<hr class="medium">
<img class="mx-auto d-block" src="/assets/img/Escape_Blueprints.png" style="border-radius: 50%;" />

#### Blueprint UI System

Every game needs a UI. For this game I decided to do the entirety of the UI inside the Unreal Blueprint system. At first it was an unfamiliar approach compared to just coding components I've done previously, however it seemed like the standard OOP practises apply with blueprints too. Attempting to apply SOLID principles to the blueprints was difficult though, and an area I'll pay much closer attention to in future projects.

### Project Retrospective

<img class="mx-auto d-block" src="/assets/img/Escape3.png" style="border-radius: 50%;" />

#### Unreal

I had a great time digging into the unreal engine, and it is something I am keen to explore further in my future projects, but it did take some getting used to! Unreal has such a wide library of features, each with their own collection of functions, variables and types that I found confusing at first. Now I have spent some dedicated time in the engine I am confident I will be able to make good use of the tools available in my future projects!

#### Blueprints & Unreal C++ Libraries

Blueprints were an interesting feature to me, they provided all the functionality that code might have, but in a visual form. After using it for this project though, I am confident I can read, understand and use them in future projects to great effect. There was a fair number of features around blueprints I did not use though, that I would like to explore in my future projects.

With that said, I also had a great time working with the Unreal libraries. Although it was vastly different from the Unity approach and features that I am used to, there were enough similarities that coding the gameplay features of this game was not too strenuous.


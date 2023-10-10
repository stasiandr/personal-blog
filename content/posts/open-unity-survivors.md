---
title: "Open Unity Survivors"
description: "Description"
date: 2020-09-15T11:30:03+00:00
tags: [""]
draft: true

editPost:
    URL: "https://github.com/stasiandr/personal-blog/tree/main/content"
    Text: "Suggest Changes" 
    appendFilePath: true
---

Ah, the classic conundrum for developers — how do you showcase your skills when all your coolest code is hidden behind NDA? The challenge is especially true when you're preparing for job interviews in the game development world. Business deadlines often clash with the developer's desire to produce quality code, making it difficult to maintain a portfolio that truly reflects your abilities.

So, to help both myself and the community, I'm thrilled to introduce "Open Unity Survivors." This is an open-source project that focuses on implementing core mechanics similar to those found in "Vampire Survivors"©. Let's be clear: this is not intended to be a clone or a rip-off of the original game. Instead, consider it a practical educational resource for those looking to get their hands dirty with Unity game development.

If you find this project useful, or if you end up incorporating any of its elements into your own game, I'd be more than happy to hear about it! The project is available under the CC0 license, meaning you're free to use it as you see fit, no credits required (though always appreciated).

I want to give a special shoutout to [Quaternius](https://quaternius.com) for their fantastic low-poly models, which I've used in this project. If you're in need of some quality assets, do check out their website.



Next I would like to share some usefull thought and ideas that you might incorporate in your games.

## Hierarcy pattern

I haven't heard from anyone about this pattern until I was talking to my friend about struggling with following problem. I have two types of abilities in open unity survivors -- active and passive. While passive is always about messing up with player stats, active is about grenade go boom or poison slowly killing enemies. And the problem is that at some point I'd like to instanciate object representing both of them. Active ability needs to be MonoBehaviour, because it need some presentation in game world. Passive ability instead don't need to access any Unity stuff and just want to mess with PlayerModel class. The answer to this problem is hierarcy pattern.


So as I've said earlier I will skip details about WeaponFactory class. Now is the time to discuss it in details.

## Factory string -> GameObject

There are so many ways to spawn GameObject from code. But there aren't so many widely known options when to spawn GameObject from pure C# class. I hate using strings in my code. I stumbled so many times with the situation when I lost hour debugging misspeled variable in JS or serialized string. So I will use Enums, simple scriptable objects, anything except strings. But that friend also gave me second idea -- sometimes it isn't about you and your coders colleges. If you want to make your game balance you are anyway bound to excel. And if so, values there anyways will be string like it or not. So, I present to you my way on how to protect yourself from strings as my as I could.

// Don't forget to menthion about WebGL reflection.

## VContainer

Ah, the magic of Dependency Injection — one of those topics that can be a game-changer in Unity game development. MonoBehaviour serves as both the entry point for our C# code and as a presenter in Unity. While this dual role has its advantages, it can get problematic when trying to separate domain logic from the presentation layer. That's where DI comes in with its principle of Inversion of Control. Using DI containers like VContainer, you can make pure C# classes your entry points instead of MonoBehaviour. This approach allows you to separate domain logic from presentation.


And that's it for today! I'll be dedicating an entire post to VContainer and how it seamlessly integrates with Unity to make our developer lives a bit easier. Stay tuned!

Feel free to share your thoughts, and let's continue to build better games, one line of code at a time!
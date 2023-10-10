---
title: "Flask viewer"
description: "Description"
date: 2020-09-15T11:30:03+00:00
tags: [""]
draft: true

editPost:
    URL: "https://github.com/stasiandr/personal-blog/tree/main/content"
    Text: "Suggest Changes" 
    appendFilePath: true
---

# From square water to raymarching

Once upon a time I was developing chemistry lab in VR. From the first day of development was clear that key to the best visuals in game about mixing liquids is liquid rendering. It was a great journey I would like to share with you today.

## The problem

We need to properly draw liquids in vessels. Half of the problem -- render it on screen, but other half -- to properly render consistent volume of liqud. Imagine you have 30 ml of liquid in 250 ml Conic flask, where would be plane dividing liquid and air? Also concider that flask could be rotated.

![Flask liquid plane]()

Second problen was that even if you know where would be that plane you need to render it somehow. But you don't have any triangles there to render it with classic fragment shader.

Our approach to this complex task was divided in iterations, so in this post I will structure as iterations too.

## Where is the plane?

### Kinda approximation

When we were building prototype we needed to implement liquid plane as fast as possible with possible loss of quality. So we decided to try and solve the problem on 2D plane. Let's slice flask in half so it's axis of rotation would be on that plane. As you see task convertes to basic planar math. So we took beaker wich looks like cylinder and got function (angle, volume) -> distance from bottom. All next flasks properties like radius or height are concidered constant. 

![Sliced flask with 2D math]()

This is pretty good approximation, but it suffers and breaks on extreme angles and volumes, because actually object we trying to calculate isn't body of revolution. Beaker is body of revolution, but sliced liquid isn't (see picture). But as it always is we needed prototype done yesturday and so we tweaked our function to work better at extreames and continued with our journey.

### Intergrals

Second thought was to calculate proper integral of sliced body. I don't have now papers, but it was like 5 A4 pages dedicated only to cylinder. Equasion concidered 4 different cases and was rather hard to implement in code. We implemented.
---
layout: post
title:  "Flow based user interface for complex structures (part 1)"
date:   2016-08-11 17:07:03 +0200
categories: ui dev
comments: true
author: "Dion"
---

For quite some time now I have this idea slumbering in the back of my mind. I think it started some years ago when a colleague  of mine, not a developer, said he was certain that coding would become redundant. There would be user interfaces that can create logics like I do with my code at the same level of user friendliness Adobe Photoshop has to offer.
 
Listening to that comment from a developers perspective can be somewhat provocative. I mean, coding for input output type of projects, yes, I can see how our work will become redundant. But not by means of a user interface. More in an automated ai or machine learning  type of solution. We became used or even attached to our carefully crafted code. Once you "know" how to code the ease of use argument loses any ground to stand on. In my opinion the entire statement also reveals a fundamental misunderstanding of what coding is. At the very least there is a fundamental misunderstanding about what it solves, and how. And, while I'm already burning down this comment from my well meaning colleague: Frankly, Photoshop isn't that easy to use!

To be fair, at the time I was mostly building websites. Some large some small, but nothing exceedingly complicated. A lot of those websites were powered by a content management system. We were using ExpressionEngine most of the time. The entire idea of coding through a UI is nothing new, and in my humble opinion it sucks for anything serious most of the time. There definitely are some interesting attempts that I shouldn't leave unmentioned: [NoFlow](http://noflojs.org "NoFlow"). It even has a Jekyll example [Jekyll](https://github.com/the-grid/noflo-jekyll) (this site is running on Jekyll). It looks promising but, if I understand correctly what they are doing it's an entirely different beast. I have tried such flow diagrams on my drawing board before, but in the context of web development. More specifically in the context of a content managed website. I failed, I kept struggling to keep the resemblance of an ironical pile of spaghetti down to a minimum. I mean, even in the case of a simple input/output type of website there is quite a lot going on. We have a user, this user might have access to some parts of the website and some not. There are cases that should trigger a 404, we might need ways to restrict and validate user input and the list goes on.

I still think it's a bad idea to go about coding through flow diagrams. In my opinion it usually adds complexity, while this is exactly what it's trying to solve. And that for the sake of not having to write code. Really, just learn how to write code instead and you will be better off. But! For the act of configuration, I can definitely see a lot of added value by using a flow diagram type of system. Still, the challenge remains. How not to get a big pile of spaghetti. (spaghetti makes more sense than mud here) So, let me take you through my thought process while trying to tackle this problem.

First, let me show you some of the examples I have seen in the past. They are coming from 3d and animation software. Some of them contain this type of configuration / logics system for many years already. 3d animation packages have a lot going on. Think of modeling, mapping, animation, material editing, lighting, rendering and whatnot. For animation and materials the node or flow based structures are a really powerful tool to make more complicated structures without the need for coding. 

![Blender](https://s3-eu-west-1.amazonaws.com/dionsnoeijen/nodes/blender.png "Blender")

<span class="sub">Blender, an open source 3d animation tool has a really nice implementation.</span>

![Blender grouped](https://s3-eu-west-1.amazonaws.com/dionsnoeijen/nodes/blender-grouped.png "Blender grouped")

<span class="sub">It also contains this awesome idea, grouping. You can make input and output available through connections made to the left and right edges.</span>

![Modo](https://s3-eu-west-1.amazonaws.com/dionsnoeijen/nodes/modo.png "Modo")

<span class="sub">Modo, also a 3d animation tool containing a very good implementation.</span>

![Modo modifiers](https://s3-eu-west-1.amazonaws.com/dionsnoeijen/nodes/modo-modifiers.png "Modo modifiers")

<span class="sub">I like the way you can integrate simple logics.</span>

![Modo grouped](https://s3-eu-west-1.amazonaws.com/dionsnoeijen/nodes/modo-grouped.png "Modo grouped")

<span class="sub">Also, with grouping in place.</span>

There's a lot of inspiration I can draw from those examples. I see a lot of use for the grouping method, it should really help reducing the messy effect that too many connecting lines have. I would like to add another idea to the table, breadboards. Lately I have been playing around with my Arduino, I bought some components to make an ultrasonic radar. Mostly just for fun. I have the starters kit and it contains a small breadboard.

![Arduino Breadboard](https://s3-eu-west-1.amazonaws.com/dionsnoeijen/nodes/arduino-breadboard.jpg "Arduino Breadboard")

![Arduino Breadboard](https://s3-eu-west-1.amazonaws.com/dionsnoeijen/nodes/arduino-breadboard-2.jpg "Arduino Breadboard")

<span class="sub">In short, you can connect stuff that's on the board with resistors and wires</span>

It gave me an idea. We need to get simple logics embedded in the node itself by making use of the input and output connections on the node itself. Like this. First, you would drag out a desired size for your breadboard on a grid.

![Node Breadboard](https://s3-eu-west-1.amazonaws.com/dionsnoeijen/nodes/node-breadboard.png "Node Breadboard")

Then, you would drag elements on top of them, snapping to the predefined grid on the board. Let's drag one out, a numeric value.

![Node Breadboard](https://s3-eu-west-1.amazonaws.com/dionsnoeijen/nodes/node-breadboard-num.png "Node Breadboard")

Maybe we want to multiply this value before we give output on the node level.

![Node Breadboard](https://s3-eu-west-1.amazonaws.com/dionsnoeijen/nodes/node-breadboard-mod.png "Node Breadboard")

The input circle would be able to take a value for the numeric field. That field has output available on the board when the size of the element doesn't reach the edge. Otherwise it would create an output circle for the node. The node output is created by the multiplication modifier. This is the most basic exemplary use case I can think of. In a real world application this would require a lot more sophistication. But I will get into that in the next article since here's a lot more to think about from here on. How the interaction should work and how to make use of the breadboards in a meaningful way. For now I will leave it here. In the next part I will explore a fictional project that makes use of the ideas explored here.

Feel free to leave a comment!

[Part 2](https://dionsnoeijen.github.io/ui/dev/2016/09/14/flow-based-user-interface-for-complex-structures-part-2.html "Go to Part 2")

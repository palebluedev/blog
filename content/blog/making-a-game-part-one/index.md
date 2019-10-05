---
title: Making my first game.
date: "2019-10-05T14:00:00.001Z"
description: "The journey in how I made my first game"
---

## Introduction

For a long time I've wanted to write a game, but never really committed to writing one becuase I know
the amount of time that I would have to put into getting it done. However recently I've been very good
at cutting the scope of projects to get them develivered and with this new skill I'm wondering if I can
finally complete a game.

**The goal:** is to make a game in a week.

What I hope to share here is my process of coming up with an idea and delivering on that idea. I'm a sofrware engineer but I know nothing about making good games, so lets see how this goes!!

Timer is starting: **23:00 Satuday the 5th of Oct**

## The idea

Sadly the idea doens't matter, I wont dwell on this to much but lets say I had the best idea. I don't think I could deliver that in one week. However lets make the game at least somewhat intersting.

#### Contraints
```
 - I have to create it in a week
    - meaning I can't spend to much time on one thing 
    - meaning that I can't spend to much time stuck, I need room to change direction
 - I can't work on it every day
    - meaning I have probably tomorrow and maybe next saturday to get something completed
 - I can't create good art
    - meaning we're going to have to do something simple maybe some ascci art
```

One idea, is a game in which you have two go through a map and find keys for all the doors. It doens't sound that interesting however technically it has a couple of interesting parts.

```
 - simple state
    - player, key and door position. 
    - key and door connection 
    - door open/close state
 - simple state mutation
    - player position
    - player invesntory
    - key picked up
    - door opened
 - simple systems
    - player movement
    - open a door
 - simple state to know whether the game has been completed
    - all doors have been opened
 - simple map to create
    - for the concept this can just be a single room

I want the room to look somewhat like this...

        ############################
        #··························#
        #··························#
        #··················$·······#
        #··························#
        #··························#
        #······k···················#
        #··························#
        #··························#
        #··························#
        ######################D#####

```

Okay so lets get started!!!

## Day one: Defining the tools

Picking the tool, the most important quality is something that I can get up to speed very quickly. The 
last time I played around with some game stuff it was with Rust, so I think I'll do the same here just
because I'm warm to it.

Another reason I'm doing it in rust is to biggy back on the awesome work in this video https://www.youtube.com/watch?v=1oSnLVE3YbA, I recommoned you watch it!

 - [Rust](https://www.rust-lang.org/)
 - [Specs ECS](https://github.com/slide-rs/specs)
 - [tcod](https://crates.io/crates/tcod)

These tools should allow me to get something onto the screen very quickly along with controlling state in a nice way, which makes sense for me.

## Day two: Getting something on the screen
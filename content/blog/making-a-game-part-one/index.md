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

Getting something on the scren was very easy only took around 40 minutes but this is what was invovled, watching the video above. The video is amazing
at was good at refreshing my mind on how to construct my code.

I ended up with this, if you've never seen rust before. Don't worry I will highligh key parts... but really don't worry about it its not important.
```rust
/**
 * import some stuff from tcod and specs.
 */
use specs::{Join, world::Builder, World, Component, VecStorage, System, ReadStorage};
use tcod::colors;
use tcod::console::*;

/**
 * define some consts
 */ 
const SCREEN_WIDTH: i32 = 80;
const SCREEN_HEIGHT: i32 = 50;
const LIMIT_FPS: i32 = 20;

/**
 * At the top here we have a couple of components
 * components are things we apply to entities which we act uppon in systems.
 */
struct CharacterGlyph {
    glyph: char,
}
impl Component for CharacterGlyph {
    type Storage = VecStorage<Self>;
}
#[derive(Debug, Default)]
struct PrintMeTag;
impl Component for PrintMeTag {
    type Storage = specs::NullStorage<Self>;
}
#[derive(Debug, PartialEq)]
struct Position {
    x: i32,
    y: i32
}
impl Component for Position {
    type Storage = VecStorage<Self>;
}

/**
 * This is a printing system
 * USEFUL FOR WHEN WE CREATE SOME BUGS
 */
struct PrintSystem;
impl <'a> System<'a> for PrintSystem {
    type SystemData = (ReadStorage<'a, Position>, ReadStorage<'a, PrintMeTag>);

    fn run(&mut self, (position, print_me): Self::SystemData) {
        for (pos, _) in (&position, &print_me).join() {
            println!("{:?}", pos);
        }
    }
}

/**
 * This is the rendering system
 */
struct Render {
    window: Root,
}
impl<'a> System<'a> for Render {
    type SystemData = (
        ReadStorage<'a, CharacterGlyph>,
        ReadStorage<'a, Position>
    );

    fn run(&mut self, (sprites, positions): Self::SystemData) {
        let root = &mut self.window;
        
        root.clear();

        for (sprite, pos) in (&sprites, &positions).join() {
            root.put_char(pos.x, pos.y, sprite.glyph, BackgroundFlag::None);
        }

        root.flush();
        root.wait_for_keypress(false);
    }
}

/**
 * !!! NOTE: This is the entry point of the application
 */
fn main() {
   
   /**
    * some initial setup of the world
    */
   let mut root = Root::initializer()
      .font("font.png", FontLayout::AsciiInRow)
      .font_type(FontType::Greyscale)
      .size(SCREEN_WIDTH, SCREEN_HEIGHT)
      .title("Rouge like using specs")
      .init();

   root.set_default_background(colors::BLACK);

   tcod::system::set_fps(LIMIT_FPS);

   let mut world = World::new();

   /**
    * a dispatcher is how we connect our world to the systems
    */
   let mut dispatcher = specs::DispatcherBuilder::new()
      .with(PrintSystem, "print_system", &[])
      .with_thread_local(Render {window: root})
      .build();

   dispatcher.setup(&mut world.res);

    /**
     * !!! NOTE: This is where we add stuff to the world.
     */
    world
        .create_entity()
        .with(Position{ x: 10, y: 10 })
        .with(CharacterGlyph { glyph: '@' })
        .with(PrintMeTag {})
        .build();
    world
        .create_entity()
        .with(Position{ x: 2, y: 2 })
        .with(CharacterGlyph { glyph: '.' })
        .with(PrintMeTag {})
        .build();
   //

    dispatcher.dispatch(&mut world.res);
}
```

With this code we see something like the following


```

   .          
             
          
          
          @

```


## Day three: Getting things moving

Here I will continue to watch the video for the last little bits to get the player gif moving.


# Randomify - Fast & Easy Weighted Randomizer
﻿
![RandomifyCover](https://drive.google.com/file/d/1yXTa0ocXyyj-9y4b1aYxWK_wFLYwrseQ/view?usp=sharing)
 
**Randomify** is a *blazingly fast* and *easy-to-use* weighted randomizer for C#. It has been developed to be used on Unity projects, but it can be used in any C# project.

## Index
- [What does Randomify do?](#what-does-randomify-do)
- [Quick Start](#quick-start)
- [Use Cases](#use-cases)
- [FAQ](#faq)
- [Credits](#credits)

## What does Randomify do?
### The Magic Box
Let's start with a little metaphor. This plugin works like a *magic box*. Imagine you have three balls: one is red, another is green, and the last one is blue. Then, you add each ball to the box in this way: you take the red one and say: *"Its probability is 20%!"*, and throw the ball inside the box (please, slowly, try not to break the magic box!). Then, you take the green one and say: *"Its probability is 30%!"*. And repeating with the blue one: *"Its probability is 50%!"*. 

Some days later, you want to retrieve one of the balls. You say to the box: *"Please, give me back one of the balls"*. Then, the box throws at you one of them. But, which one of them? Well, it's a random decision. But a weighted random decision! As you decided when adding the balls to the box, there is a 50% probability that the box returns the blue one, a 30% for the green one and a 20% for the red one.

### The Real World
So, do you have a better idea of how it works now? Good. I want to clarify some things.

In the plugin code, we call that *magic box* a `Generator`. You can create generic `Generators` that store any kind of data, from built-in `value types` like `int` or `float`, to built-in `reference types` like `string` or `object`.  You can store Unity GameObjects, enums, your own objects... whatever you want! 

Then, when you ask a `Generator` to return you something, it returns that "something", but the returned thing is **not** removed from the `Generator`. So, if you want to specifically remove something from the `Generator`, you can easily do it with the method `RemoveElement(T element)`.

Another important aspect is that Randomify probabilities are based on floating point numbers from **`0.0`** to **`1.0`**. 
I recommend you to add elements whose probabilities add up to `1.0`, and are in the range from `0.0` to `1.0`, **but** it's not mandatory. Refer to the **FAQ** for more information!

## Quick Start

### The Basic
Let me show you a simple usage example (from a real project!) on how to use Randomify. Let's go!

Imagine you are programming a really cool procedural world-generation algorithm for your new awesome game. At some point during the course of development, you've got lots of different tiles which you want to spawn based on a weighted probability distribution. **Here arrives Randomify**.

Let's say each tile has a name and an associated probability (as well as some other data). To get started you just have to **create** a new `Generator<string>` like this:

~~~
Generator<string> tileGenerator = new Generator<string>();
~~~

Then, you **add** each of the tiles to the generator this way:

~~~
tileGenerator.AddElement(tileGround.Name, tileGround.Probability);
tileGenerator.AddElement(tileWall.Name, tileWall.Probability);
...
~~~

Now, if you **ask** the generator for an element, you will get a random tile name based in its probability. You could do it like this:

~~~
string randomTileName = generator.GetRandomElement();
~~~

Finally, you could use that `string` to search and instantiate your tile. Or rather, to do whatever you want with it!

### More Functions

##### Removing Elements
Your procedural algorithm is working well, but you want to **improve** it. After careful consideration, you decide some tiles should be limited, meaning that you only want to instantiate a maximum of `x` tiles of that type.

So you may adjust your code by doing the following:
```
if (special_tiles_limit_reached)
{
    tileGenerator.RemoveElement(specialTile.Name);
}
```

In this way, you won't get more `specialTiles` from the `Generator`. The weights of the other tiles will be properly rebalanced, so you don't have to worry about anything!

P.S: *Be extra careful when using not built-in objects as a `Generator` type. You may want to override the `Equals()` and `GetHashCode()` methods on your object, which are handy for objects comparison. More info on the **FAQ***.

##### Counting Elements
As a further improvement, you think it's a good idea to add random tiles to the `Generator` each time you run the algorithm (*Randominception?*). So, you decide to add 5 different random tiles each time. 

You could use the `CountElements()` function of the `Generator` to manage that, as shown next:

```
while (tileGenerator.CountElements() < 5)
{
    Tile randomTile = GetRandomTile();
    tileGenerator.AddElement(randomTile.Name, randomTile.Probability);
}
```

Et voilà! You have added a number of random elements to the `Generator`. Maybe the next step is to add random `Generators` to the `Generator`...?



## Use Cases
You can use **Randomify** for any project where you need weighted randomization. This plugin has been used in real games and projects, so it's been well-tested and proven in the field. Here I'm going to list some use cases:

- Randomizing enemies spawn points, making some points more **difficult** than others. 
- Randomizing **rewards & upgrades** on replayable/arcade games.
- Implementing **AI** algorithms which require randomized weights
- Implementing all kinds of **procedural** algorithms: from world generation to rewards, enemies, structures, really anything you can think of.
- Random **stats assignment** for character/enemy creation.
- And much, much **more**. It's a very flexible general-purpose tool!


## FAQ

- #### **What happens if the elements probabilities add up more than 1.0?**
	- If you add elements whose probabilities add more than **`1.0`**, the probabilities will be **properly distributed**. There is no real problem, but I recommend adding up to **`1.0`** and keeping in the range because it's easier to imagine and think about, and could have a very slightly better performance. However, this is just personal preference.

- #### **What happens if I add an existing element with the same probability? And one with a different probability?**
	- No problem! If you add an existing element with the same probability, nothing happens. If you add an existing element with a new probability, the element's probability will be updated on the `Generator`. 

- #### **My objects are not being properly removed from the `Generator`!**
	- Ouch! You probably need to override the `Equals()` method on the objects you are using as elements. Also, be sure you are passing the **correct** object to the `Generator`. More info [here](https://docs.microsoft.com/es-es/dotnet/api/system.object.equals?view=netcore-3.1). 

- #### I want to improve the performance of Randomify!
	- Well, the best way to achieve this is overriding the [method](https://docs.microsoft.com/es-es/dotnet/api/system.object.equals?view=netcore-3.1) `Equal()` on your objects. Also, if you are using value types (like `structs`) as elements, you can implement the [IEquatable<>](https://docs.microsoft.com/es-es/dotnet/api/system.iequatable-1?view=netcore-3.1) interface. That implementation can avoid unnecessary allocs. Gotta go fast!
	
- #### **What happens if I find a bug/error or I don't know how to use some plugin functionality?**
	- In the first place, my apologies! If that's the case, contact me on the places indicated below. I'll solve your problem as soon as possible, and also update the plugin with fixed bugs or update the documentation with clarified information.

## Credits

- Plugin made by [Delunado](https://www.twitter.com/Devlunado)
- Promotional images made by [Agustin Macsemchuk](https://www.behance.net/agmac/)
- Thanks to [Alexander](https://windfishstudio.com/landing) for correcting the documentation!

#### If you have any questions, problems or suggestions, contact me on on [Telegram](t.me/Delunado), on [Twitter](https://www.twitter.com/Devlunado), or via e-mail to devlunado@gmail.com. Cheers! 

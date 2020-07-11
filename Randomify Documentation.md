# Randomify - Fast & Easy Weighted Randomizer
**Randomify** is a *blazingly fast* and *easy-to-use* weighted randomizer for C#. It has been developed to be used on Unity projects, but it can be used in any C# project.

## What is really Randomify?
### The Magic Box
This plugin works like a *magic box*. Imagine you have three balls: one is red, other is green, and the last one is blue. Then, you add each ball to the box in that way: you take the red one and say: *"It's probability is 0.2!"*, and throw the ball to the box (please, slow, try not to break the magic box!). Then, you take the green and say: *"It's probability is 0.3!"*. And repeating with the blue: *"It's probability is 0.5!"*. 

Some days later, you want to retrieve one of the balls. You say to the box: *"Please, give me back one of the balls"*. Then, the box throws at you one of them. But, which of them? Well, it's a random decision. But a weighted random decision! As you decided when added the balls to the box, there is a 50% probability that the box returns the blue one, a 30% for the green and a 20% for the red.

### The Real World
So, do you have a better idea of how it works now? Good. I want to clarify some things.

In the plugin code, we call that *magic box* a `Generator`. You can create generic `Generators` that store any kind of data, from built-in `value types` like `int` or `float`, to built-in `reference types` like `string` or `object`.  You can store Unity GameObjects, enums, your own objects... whatever you want! 

Then, when you ask a `Generator` to return you something, it returns that "something", but the returned thing is **not** removed from the `Generator`. So, if you want to specifically remove something from the `Generator`, you can easily do it with the method `RemoveElement(T element)`.

Another important aspect is that Randomify probabilities are based on floating point numbers from **`0.0`** to **`1.0`**. 
I strongly recommend you to add elements whose probabilities add up to `1.0`, and are in the range from `0.0` to `1.0`, **but** it's not mandatory. Look at the **FAQ** for more information!

## Quick Start

### The Basic
Here I'm going to show you an easy example (based on a real project!) on how to use Randomify. Let's go!

Imagine you are programming a really cool world procedural generation algorithm for your new flagrant game. At some point of the development, you have lots of different tiles which you want to spawn with different probabilities. **Here arrives Randomify**. 

Each tile could have a name and an associated probability (besides other things). So, in first place, you just **create** a new `Generator<string>`.

~~~
Generator<string> tileGenerator = new Generator<string>();
~~~

Then, you **add** each of the tiles to the generator in that way:

~~~
tileGenerator.AddElement(tileGround.Name, tileGround.Probability);
tileGenerator.AddElement(tileWall.Name, tileWall.Probability);
...
~~~

Now, if you **ask** the generator for an element, you will get a random tile name based in its probability. You could do it like that:

~~~
string randomTileName = generator.GetRandomElement();
~~~

Finally, you could use that `string` to search and instantiate your tile. Or, well, for whatever you want!

### More Functions

##### Removing Elements
Your procedural algorithm is working well, but you want to **improve** it. After some thinking, you decide there will be some tiles that are limited, so when you instantiate more than X tiles of that type, you won't instantiate more of them.

So, you take your code and do the next:
```
if (special_tiles_limit_reached)
{
    tileGenerator.RemoveElement(specialTile.Name);
}
```
That way, you won't get more `specialTiles` from the `Generator`. The weigths of the other tiles will be properly rebalanced, so you don't have to worry about anything!


P.S: *Be extra careful when using not built-in objects as a `Generator` type. You may want to implement the [System.IEquatable<T>](https://docs.microsoft.com/en-us/dotnet/api/system.iequatable-1?view=netcore-3.1) interface or override the `Equals()` and `GetHashCode()` methods on your object. That's neccesary for objects comparison.*

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
You can use **Randomify** for any project where you need weighted randomization. This plugin has been used in real games and projects, so it has been well tested and proved. Here I'm going to list some use cases:

- Randomizing enemies spawn points, making some points more **difficult** than others. 
- Randomizing **rewards & upgrades** on replayable/arcade games.
- Implementing **AI** algorithms which need randomized weights.
- Implementing all kinds of **procedural** algorithms, from world generation to rewards, enemies, structures, large etc.
- Random **stats assignment** on character/enemy creations.
- And much **more**. It's just a very flexible tool!



## FAQ

- #### **What happens if the probabilities add up more than 1.0?**
	- If you add elements whose probabilities add more than **`1.0`**, the probabilities will be properly distributed, so zero problems! In example, you could add "A" with probability 50, "B" with probability 20 and "C" with probability 10, and the `Generator` will work perfectly.

- #### **What happens if I add an existing element with the same/a different probability?**
	- No trouble! If you add an existing element with the same probability, nothing happens. If you add an existing element with a new probability, the element's probability will be updated on the `Generator`.
	
- #### **What happens if I find a bug/error or I don't know how to use some plugin functionality?**
	- In first place, I'm sorry! If that's the case, contact me on the places indicated below. I'll solve your problem soon as possible, and also update the plugin with fixed bugs or update the documentation with clarified information.

## Credits
creditos
##### If you have any questions, problems or suggestions, contact me on [Twitter](https://www.twitter.com/Delunad0), on [Telegram](t.me/Delunado), or via e-mail to devlunado@gmail.com. Cheers! 

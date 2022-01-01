---
title: Hacking Roblox: Creatures of Sonaria
subtitle: Or, how I stopped fighting arrays and learned to love the vector.
---

## IMPORTANT NOTE:

**I use the word "hack", and that comes with some negative connotations. To specify, I have not created a program that intrudes in the server-side code of either Roblox or Creatures of Sonaria. The program I wrote is only used on the player's side, and even then, it is entirely stand-alone, it does not access the Roblox program running on the player's PC, nor alters any values within it, or on the player's computer, as I will explain below.**

# Part One: The Problem

So, recently some friends of mine got into playing this game on the games platform Roblox called Creatures of Sonaria. The premise is really simple: you are a mystical creature of some sort, you have needs that need to be satiated, such as hunger and thirst.

That's about it, really.

There are many kinds of creatures, each sustaining themselves in various ways, whether they be a carnivore, herbivore, or otherwise. As of writing there are about 135 creatures of various mysticality, from small, mundane creatures all the way up to giant, three-headed dragons that shoot beams from their mouths.

Recently, the game added a winter event where you can go around to hunt for presents strewn about the map. When you touch the presents, one of four things can occur:

1. The present will drop bells, the event's currency, onto the ground for you to pick up.
2. The present will spawn a snowman, which you can defeat to earn an amount of bells.
3. The present will prompt you with a creature's name, and ask you which ability it has from four choices.
4. The present will propmt you with scrambled text, and ask you which creature's name the scrambled text can be arranged to make.

Of these four outcomes, the last two are the most likely. The third outcome can be easily solved by creating a list of creatures and pairing the ability they have to their name, since each creature only has one ability. The last outcome, however, is nigh impossible unless you're really quick at unscrambling text. Even then, however, the names of the creatures are just that, names; there's no rhyme or reason to them. Case in point: this excerpt from the game's wiki:

![some example creature names](https://i.imgur.com/UKxkxsh.png)

So, I went about making a program to solve that last outcome.

# Part Two: Breaking Down the Problem

Really, the solution to the problem is incredibly simple. All I'd need to do is make a program that solves anagrams and uses a custom dictionary. Right away I noticed something that would make solving the problem a cinch: there's only ever one solution for any scrambled text the game provides, which means that each name of each creature in the game is unique in both letter counts and length.

Basically, if I were given the text "SOSRAMG", I would know that it could not be any name other than "Granmoss", because I know that the event's minigame would not work if there were two solutions to "SOSRAMG".

In addition, the program needed to pull the names from a text file so that, in case of an update that adds new creatures, the program could still be usable without needing to re-code the entire thing

Also, I wanted to write it in C++, both to learn the language, and to have a program I could package into a zip file and send to people.

# Part Three: How the Program Works

The program works by taking the list of names in the `names.txt` file and generating letter counts for every name. 26 letters plus spaces and apostrophes. It takes that data and compiles it into a lookup table where each creature name is linked with a map of each letter and the number of them in the name.

Once that's all set up, the user is prompted to enter some scrambled text, say for example: "ocouzu". That text, "ocouzu" is then processed letter by letter to generate counts for each letter in the word, which is then processed into a map of its own with each letter in the word and the amount of times that letter shows up in the scrambled text, dropping any letters that don't show up at all:

* `'o': 2`
* `'c': 1`
* `'u': 2`
* `'z': 1`

Once this is processed, the program then goes through each name in order to see if it's the right one. Yeah, it's a sequential search, but the point of the program was to create something quickly, so worring about computation time was low on my priorities list.

That said, I did add some shortcuts. For example, one of the first things that's checked is to make sure that the scrambled text is as long as the name it's checking. If the name is a different length than the scrambled text, it moves on immediately. Once it finds a name of the same length, however, it goes down the word counts in the scrambled text, making sure that they line up as well. If at any point the word counts don't match, it goes to the next name in the list.

Once it finds a name of the same length as the scrambled text, which also passes all the word count checks, the program can safely say that it's found the name, which it prints out to the user, and asks it for some more scrambled text. From there, you tab back into the game, and type in the name the program gave you, netting you some bells during the game's event.

![an example of the program in action](https://i.imgur.com/64UhfM7.png)
###### An example of the program in action.

# The Results

So, I don't really have any concrete data on bell-generation from completing quizzes given by collecting presents, but at one point I asked a friend who had been using the program for about 30 minutes how many bells they'd gotten, and they said they had 200 bells before using it, and 1000 by the time I asked.

So, about 1600 bells an hour, if you really focus on collecting presents.

# Conclusion

It worked, I learned some C++, and my friends got some enjoyment out of speeding through the winter event in a game they like to play. Overall, I'd say I'm pretty happy with the final results, even if they're not 100% optimized or by the book. I think I could do a few things differently, maybe have some sorting algorithm to set up the names in a tree so that they're checked faster, as well as clean up some code to avoid repeat actions.

Source code and download link is [here](https://github.com/zacharyluck/creatures-of-sonaria-unscrambler)

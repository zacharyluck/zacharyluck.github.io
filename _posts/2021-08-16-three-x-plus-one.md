---
title: 3x+1, A Quick Foray
subtitle: A simple idea with infinite complexity.
---

# Part One: Exploring the Idea

I recently discovered the mathematical problem of 3x+1, it's actually a simple premise, really. Take a number, specifically any whole number, and iterate on it using the following rules:

1. If it's an odd number, multiply it by three and add one.
2. If it's an even number, divide it by two.
3. Continue this process until you've either found an infinite loop of numbers, or work your way down to the number 1

The thing that's absolutely remarkable about this process is, that despite being so simple, we still haven't found a solution where there is a set of whole numbers that, upon being iterated using this ruleset, creates an infinite loop of numbers.

Basically, what this means is: as far as we know, this ruleset will always create a sequence of numbers wherein each number is unique and wherein the sequence will eventually devolve to 1.

I was introduced to this problem by a friend of mine, who is an artist, not a programmer, their first attempt at programming something to solve the problem looked something like this:

![their attempt](https://i.imgur.com/qTvgcWt.jpeg)

Obviously, the code is not complete, and wouldn't even run properly, but it gave me a good idea of what I needed to do in order for it to work, and so I gave my first shot at it:

![my first attempt](https://i.imgur.com/s21aKjD.png)

![the result](https://i.imgur.com/bp3JWWI.png)

Ah, right. Python really doesn't like recursion if you go too large, but you can see the code, and how the problem works, in action all the way up to 996, and how they all terminate in 1. At this point, I started to notice a pattern I'd guessed would happen when I was discussing the problem with my artist friend and solving these problems by hand, something that's especially visible in the results of 1 through 20:

![a pattern emerges](https://i.imgur.com/0CgiS78.png)

As you solve for certain numbers, you notice that those numbers come across ones you've already solved before, or can even predict numbers in the future that will also fail. For example, 3's pattern is: 10, 5, 16, 8, 4, 2, 1. We've already solved for two, as it, obviously, is halved to become one, and then fails, but it also proves that the numbers 4, 5, 8, 10, and 16 will also fail inevitably.

7 is another good example, as it proves that numbers as high as 52 and 40 will inevitably fail. We're already proving that numbers halfway through double digits will already fail using single-digit numbers, so I think it's time we add in some code to make it run faster.

# Part Two: Simplifying The Process

Thinking about the pattern I noticed from before, my first idea was to have a set of numbers that I would store which I knew were numbers that would fail, so I could skip them when I encountered them. **This was a terrible idea and I didn't do it**, and the reason why is explained by looking at the solution for 27:

![solution for 27](https://i.imgur.com/6DiRtk6.png)

Even without a mathematical way to calculate the Big-O for space complexity of how many numbers you'd have to store, if I had to take a guess, I'd say it's probably an exponential amount of space required based on the number you're currently trying to solve. We don't want that, we want something simple and easy, and space efficient, which we can accomplish with two easy filters:

1. Automatically stopping any time we go below the starting number
2. Skipping every even starting number

The first one is pretty self-explanatory, we've already checked all of those numbers and know for a fact that they fail, so if at any point our sequence of numbers goes below the starting number, we can safely say that the sequence will inevitably fail. In a similar fashion, we're skipping every even starting number because the first thing you do with an even number is divide it by two, and since all numbers below the starting number have already failed, we can safely skip every even number. Let's implement this into the code:

![code example](https://i.imgur.com/Gi0KHOv.png)

I also added a new method to check when we've looped; since we're already saving numbers to display them, we can just check against that list to see if we've hit any numbers we've already encountered and found a loop.

So, now that I've added a few simple early catches, how fast does it run, and how much space does it use?

![the code in action](https://i.imgur.com/6YCTYVQ.gif)
![how bad it's getting](https://i.imgur.com/B2uudDA.png)

You can see the numbers going up by the hundreds of thousands really quickly while it doesn't use too much memory, and, while I do have a fast computer, you can also notice the pattern in the numbers being stopped early, where most numbers terminate after three iterations, even when they're really big:

![termination showcase](https://i.imgur.com/dEBQ1jH.png)

This means that, despite the numbers we're testing increasing as time goes on, the number of iterations will remain relatively low, increasing the speed of the code overall.

Obviously, we're not gonna find the solution anytime soon. From what I've heard, the solution to the 3x+1 problem would have to be 1.8 **BILLION** numbers long (if using a positive starting number) and, according to the rules we've discovered already, be comprised entirely of unique numbers that never fall below the initial starting number. That said, we can keep improving the code we've got to make it run even faster.

# Part Three: I Overload My Computer

Okay not really, but I'd like to still add in one last thing to see how fast we can get this code running.

Remember the idea I had before, of saving every number we know for a fact fails? We're gonna implement that, but in a smart way so we're not saving every number we know fails. Here's the ruleset I devised while thinking through the "fail list" as I'll be calling it:

1. The numbers stored in the fail list cannot be even
2. The numbers stored in the fail list must be above the lowest failing number
3. The numbers stored in the fail list must be removed when the loop gets to them

This will ensure that, at the very least, I'm reducing the number of failing numbers I'm storing to a quarter of their original value.

![part 3 code](https://i.imgur.com/in5q6YF.png)

I also added some code during the loop to help with the rules above

![part 3 loop code](https://i.imgur.com/fKA4Guh.png)

So how fast does it run?

![it runs very fast](https://i.imgur.com/Jdcy5Jg.gif)

And how much space does it take up?

![oh](https://i.imgur.com/4aj6KOh.png)

![oh no](https://i.imgur.com/gGV0hf3.png)

![oh dear god no](https://i.imgur.com/nyRVJvR.png)

It was about this point when I stopped the code. Needless to say I probably couldn't run this forever, and there's definitely something I'd need to do to limit the speed at which the code eats up memory.

# Conclusion

Obviously, the 3x+1 problem won't be solved via brute force, the way I'm doing it, and I'm sure that some mathematician way smarter than me will eventually figure out some formula or equation to solve the problem, but I still wanted to take a shot at it using the tools and knowledge I have, and maybe I could figure out some rules about the problem to see if there's anything I can figure out about the solution, if one is ever found during my lifetime. And there's two things I can say for certain:

One, the starting number will be odd

Two, the sequence of numbers will all be numbers larger than the starting number.

# Addendum: 8-21-2021

I've put the code I showed off in this page [here](https://github.com/zacharyluck/three-x-plus-one) if you want to go check it out and see how I've gone about things, maybe alter my code if you want to try tackling 3x+1 yourself!

Some ideas I had after the fact:

**Solve the problem backwards;** we're looking for loops, right? So the first and last number will be the same, instead of doing 3x+1 and x/2, try using 2x and (x-1)/3, and stop whenever the number isn't divisible by 3. Thinking about the problem like this also creates even more rules each number needs to have in order to be part of the loop.

**Solve the problem formulaically;** This one is a bit harder since you'd need to be some genius math wizard to figure this out, but surely there must be some form of f(g(f(g(f(x)))))-ish formula where it equals x when f(x) = 3x+1 and g(x) = x/2. But again, that's some super high level math stuff. I may be good at math but I'm not *that* good at math.

+++
title = 'An Interesting Clock Puzzle'
date = 2025-02-10T14:53:56-05:00
displayPublishedDate = true
draft = false
+++


This is the first time I'm writing a post for this website so I'll keep it short and sweet.

I was attending a physics class about classical mechanics where I got introduced to this puzzle. I enjoy the statement and the solution
of this problem for a couple of a reasons. The question and are easy enough to present for your average middle schooler to understand.
To solve this problem you do not need to solve for solutions but only count them. Finally, there is a natural appearance of a torus topology in the solution.


The problem is as follows. Suppose you have a standard 12 hour analogue clock with only an hour and a minute hand.
The twist is that the hour and the minute hand are indistinguishable.
How many times in a 12 hour period are you unable to determine the exact time?


The first important insight is that for most hours of the day, you are able to determine the exact time because you can tell which hand is the hour hand
and which hand is the minute hand. For example, if one hand is pointing directly up and one hand is pointing directly to the right.
The hour hand never points directly up at 12 when the minute hand is pointing directly at 3. So therefore the exact time must be 3:00.

Now I will explain one solution to this problem which involves carefully choosing a configuration space for the clock hands.

Let $x$ and $y$ represent the proportion of clock face which they have rotated for the minute and hour hand respectively.

The position of the hour hand as it rotates around as a function of the minute hand can be written as $y = \frac{1}{12}x$. However,
since we have 12 possible positions of the hour hand, we actually have 12 equations of the form $y_k = \frac{1}{12}x + \frac{k}{12}$ where $k = 0, 1, \dots, 11$

Similarly, we can write $x = 12y$ to represent the minute hand as a function of the hour hand. We also get 12 separate equations depending on the exact 
hour we are at. These equations are $x = 12(y-\frac{k}{12})$ where $k = 0,1,\dots, 11$.

As a result, if we graph these equations on the unit square, we get 12 parallel diagonal lines from the first set of equations and 12 parallel diagonal 
lines from the second set of equations.

Each line in the total of 12 crossings resulting in a total of 144 crossings.

Now we need to account for the crossings that happen on the line $y=x$ because for these times, even if the hour and minute hand are switched, we get the same time.
There are 12 such crossings (11 if you consider the fact (0,0) and (1,1) are the same point) so we do 144-12 = 132 (or 143 - 11 = 132) total times were we cannot tell the time.

Finally, we make a nice remake that by gluing the edges of the unit square the way we did we topologically get a torus. We can also come to this conclusion if we consider the face
that the configuration space of two rotating objects $\mathbb{S}^1 \times \mathbb{S}^1$ is homeomorphic to the torus.

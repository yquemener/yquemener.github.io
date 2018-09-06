

---
layout: post
title: Lessons I learned on deep-learning this summer
---

# Where GOFAI meets DL

This summer I took my first holidays in about 3 years: one month of bliss next to a swimming pool. I also took the occasion to dive a bit deeper in some subjects I am interested in AI, namely, problems solving. I love automated planners and wanted to see if I could apply some of my newly acquired deep-learning knowledge to teach a neural network to calculate an heursitic score for a problem state.

Quick recap if you don't see what I mean: in automated planner, you are given a problem consisting of an inital situation, a desired situation and a series of actions that will change the situation. Most actions have prerequisites. Your goal is to find a sequence of actions that transform the initial situation into the desired situation. 

A typical way to solve these problems is to create a heuristic function: a function that scores intermediate situations to give an indication of whether you are gettinc closer or further from the solution.

A very simple example of a planning problem is maze solving: given a maze, a start point and an end point, find the sequence of moves that brings an agent to the end point. The heuristic for this would be the shortest path from the current position to the end point. It is easy to program manually but I wondered if I could make a model that would learn it?

I learned interesting lessons both on the technical side on the academic side of things.

# A formating problem

Searching for maze solving attempts using deep learning, I saw several people solving mazes by using 2d images of a maze and using convolution layers to find paths. This is a nice approach, but I was interested in a more generic problem than that. My mazes are supposed to be an introduction to more complex problems. I did not want to limit myself to things that can be represented in 2D, I wanted to input topological mazes, in the form of a graph.

Most models either assume a fixed input size or convolutional data (i.e. data points that are related to each other in space or time and where proximity is meaningful). In my case I want the order of the edges to be irrelevant and to receive the same result even if the edges are shuffled. 

I decided to use a recurrent network, namely a LSTM. These networks are typically fed long sequences of inputs and are good at spoting long term dependencies. They are often used on temporal data where proximity is meaningful but this is totally optional to the LSTM, that can learn to focus on the nature of the input instead of its position.

I described edges as a triple of values, and encoded them as one-hot vectors. The outputs would be another one-hot vector, of size 20, therefore outputing a number from 1 to 20.

# A simple test

Before trying to digest a maze, I wanted to check if a simple model would be able to learn to count, which is somehow a prerequisite to any shortest-path algorithm. This is supposed to be a easy task for an LSTM. So I fed it with series of input consisting of edges in the form "box contains object143" as well as random edges that may have "box" in first position or "contains" in second, and trained it to count the number of items contained in the box.

A simple LSTM succeeded easily at this task.

# A surprisingly direct success

Now, a shortest path is a classic program but not an especially trivial one. It requires to keep track of already visited nodes, find non-visited nodes adjacent to the best scored visited one, and iterate. The most direct way to do it is to have a stack of pointers but this is not something that common DL models easily learn.

I wondered if a big enough model may be able to find a bruteforce solution to this problem. A LSTM is a nice way to encode inputs but I suspected I may have to add a few densely connected layers somewhere in it.

Anyway, I wrote a maze generator (using different mazes at each epochs prevented any kind of overfitting, that's one of those nice cases where you don't have to worry about setting up test cases vs training cases) and fed it into a basic LSTM with 1024 neurons. And to my surprise, it trained into a model that managed to find the correct path length in more than 50% of the cases. Fiddling with parameters, I quickly got it to train enough to arrive at 99+ % accuracy.

# From surprise to awe

That looked a bit too easy, but this is the way I felt the first time I fed a computer vision problem to a VGG network. 

I googled a bit more, I did not find much people trying to extract global characteristics from a graph. Most DL problems using graphs as input are interested in classifying the vertices. I contacted a friend who used to do research and he suggested to do some more bibliography and try to submit a paper for reviews.

I set my experiments a bit more rigorously and tried to find optimal parameters, vary model and training parameters as well as the generated mazes' length.

Now I was a bit more paranoid. I knew that in this case, what could have happened is information leakage: you involuntarily put in your input a way to cheat and find the solution more easily. I was especially suspicious of that as my generator starts by generating the correct path and only then adds distraction paths. So I shuffled my inputs, the edges can be fed in any order. I shuffled the one-hot encoding so that rooms that are part of the solution do not end up being at the same spots.

The model continued to learn to evaluate shortest paths efficiently. One oddity though: learning was systematically quicker when fed *bigger* mazes. I had the opposite of a curriculum learning.

I stopped believing I had found a super efficient architecture when I started removing neurons from my LSTM. With 1024 neurons on the hidden layer, I could have accepted it found some way to count and keep track of paths of length 15, even if I could not figure out how. But when it managed to do the same with just 50 neurons, I knew something was fishy. I can believe in unexpected cleverness, but even in the middle of the wonders of deep-learning, I still do not believe in magic.

# Finding the flaw

As I suspected, the key was in my generator. I wrote another generator in python with a different algorithm and this time generated a million mazes, as a test test. The accuracy fell down to 20%.

So what was happening was that my generator was creating mazes with a distinctive feature: the path I asked them to find was usually the longest possible path in the maze. This was much more likely to be true in the biggest mazes, explaining why my model was helped by bigger sizes of inputs. It probably still learned to find paths of length 2 or 3 because in that case my generator was likely to propose alternate paths.

# Your innovation is old news

Well, that was a bit diappointing, but this was just confirmation of my first intuition, right? a basic LSTM is probably not complex enough for this task. Let's try to make a model able to emulate some kind of primitive stack, good enough for counting its steps as it iterates through the maze.

I thought I may be able to gather enough interesting things for a paper, given that I could not find many people studying input graphs like I did, especially if that shortest-path finder could work.

Ah, the hubris of beginners! I do not know how my google-fu failed at the beginning but reading a bit more on neural turing machines (which at first had appeared to me as overcomplicated) I stumbled upon a Nature article, dating of December 2016, describing a new architecture: the Differentiable Neural Computer (DNC) in which are explained how to train algorithms to find shortest paths or to plan simple tasks.

# So what were the real lessons?

I still learned a lot doing this, so, anyway, if you have an idea, code it, test it. But bibliography is not a waste of time, especially in a world moving as fast as deep learning is. There are several subfields, several problems and some different fields than you expect may be trying to solve what you are working on.

Oh, and it is hard for a hobbyist to write research papers in such a field...

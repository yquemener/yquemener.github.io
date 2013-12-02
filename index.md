---
title: Automated Planning and PDDL
---

# Automated planning
## Introduction

This page is more of a personal brainstorming notepad than a helpful resource for now, I doubt it will be useful to anyone in its current form.

Hopefully I will document my increasing insights on the PDDL language, on automated planners, and on my AI-related efforts as they unfold. Not sure if this will be in the form of a blog or of a static and updated documentation though.

## Resources

* IPC 2008 -> Mirrored the code of the planners, that's good!
* IPC 2011 -> Only links. One can only find FastDownward and LAMA (which are now merged).

* FastDownward
* FastForward

## Problems I want to use planners for

### Automated code generation

I would like to be able to describe a programming problem, in the form of a task to achievem either in PDDL or in a higher lever language, and have the planner solve it by producing a program achieving the described task. This is a quest toward one of the programmers' Graals: the ability to solve a problem once, independently of a programming language. The generator would be able to generate programs in all the programming language it knows. Adding programming language and adding problems to solve would be orthogonal tasks.

### Micro-economics simulations

Economic simulations lack smart agents, or at least agents to mimmick somt classical behaviors of human "bounded rationality". The goal here would be to create an agent who tries to maximize its profit in an environment with changing conditions and imperfect knowledge. 

## Considerations

### On the importance of a community and IPC's relevance

One of the problems of academic research is that it tends to focus on producing publications rather than software that is easy to reuse by other teams. In that respectm the International Planning Competition provided a very interesting forum. They proposed a set of challenge in a normalized language (PDDL) which has now become the de-facto standard for a number of planning problems.

More than that, it created a focus for different team who then worked less in isolation and could not only compare their code with others but also have a neutral benchmark of their respective capacities. It is unfortunate that the IPC 2011 did not do what the IPC 2008 did: mirror the code of the participating planners. This has made the IPC 2008 a treasure trove.

Now, one project has attracted a community of developpers: FastDownward, which is the result of the merge of three different projects, including LAMA: the winner of the 2008 competition and a top performer in the 2011 one. 


### On the differences and complementarity between optimization problems, deterministic and probabilist planning, and Markov and Bayesian inferences

## Tips and tricks
## Kind of state of the art
## Please correct me!


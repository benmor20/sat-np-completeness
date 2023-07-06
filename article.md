# SAT: The Original NP-Complete Problem

## Prerequisites

- Some familiarity with basic set notation
- Interest in computer science!

## Introduction

Did you know that if someone found a way to efficiently determine whether a level of Super Mario was solvable tomorrow, we could use that as way to find a cure for cancers? As a (studying) computer scientist, one of the things that never ceases to amaze me is the similarity between so many of the problems we encounter. In fact, in some regards, they are the *same problem*. In this article, we will dig into the first problem of this type we found, and how it became the seed for all of them that follows.

## Background

Before we can really delve into the heart of this, there is a significant amount of background knowledge that we need. I intend for this article to require zero background knowledge, so I will be starting from the beginning. If you feel like you have a good handle on Turing Machines and Complexity Classes, feel free to skim or skip this section.

### Turing Machines

When the field of Computer Science first started, there was one main question: What does it mean to "compute" something? Or, can we formalize the notion of computation? Computer Scientists were specifically interested in the idea of decision problems: given some string of characters (say, "01110100100" or "abc" or really whatever), give a yes or no answer to some question. These are called "decision problems". For instance, let's say our problem is "Is this number prime?" and our possible characters are 0, 1, 2, 3, 4, 5, 6, 8, and 9. We would need to find some mechanical way of converting some string of digits into a yes or no answer - yes if the string is a prime number, no if it is not.

There were a few attempts at this, including recursive functions and the lambda calculus, but the most famous (and most useful for our purposes) is the Turing Machine. It was invented by Alan Turing - the same person who would go on to crack the German's Engima code in WWII - as a way to prove that not everything can be computed (if you would like to know more about that, google the Halting Problem).

Turing figured that computation would need four things:

1. Memory
2. A way to read memory
3. A way to change memory
4. Instructions

Picture some really, really, long line of tape extending off to the right (Turing made this infinite but for our purposes it does not matter so long is it is sufficiently big). This tape is broken down into many squares from left to right. On each square is a symbol - classically, this is 0 or 1, but in reality it can be anything you would like. This comprises the memory of the Turing Machine. Above the tape is the machine itself. Picture it as a box looking down at the first square. Inside that box is a set of states with some transitions defined between them. Each transition is of the form "If I am in state $p$ and I see symbol $a$, replace $a$ with $b$, move one square in direction $d$, then transition to state $q$." For example, for the instructions below, if the machine is in state 1 and it sees a 0, it will replace the 0 with a 1, move to the right, and transition to state 2. Eventually, the machine will transition itself into one of two states: an accepting state and a rejecting state. When the machine moves itself into either of these states, it will stop and the answer to the decision problem will be yes if it is in the accepting state, and no if it is in the rejecting state.

Let's formalize this definition a little. We will say a Turing Machine consists of seven things:

1. $Q$: some set of possible states
2. $\Sigma$: A finite set of symbols the input string can consist of
3. $\Gamma$: A finite set of symbols that the Turing Machine can write on the tape. Note that $\Gamma \supseteq \Sigma \> \cup \{␣\}$. That is, $\Gamma$ must contain all the elements of $\Sigma$, plus ␣ (a visible space, which we will use to mean a blank square), plus maybe other symbols
4. $\delta$: a function from $Q \times \Gamma$ to $Q \times \Gamma \times \{L, R\}$. That is, $\delta$ is a function that takes in an element of $Q$, the current state the Turing Machine is in, and an element of $\Gamma$, the current symbol the Turing Machine sees, and returns an element of $Q$, the state the Turing Machine will transition to, an element of $\Gamma$, the new symbol to write on the tape, and an element of the set $\{L, R\}$, the direction the Turing Machine will move after writing the symbol.
5. $s$, an element of $Q$ that gives the state that the Turing Machine starts in
6. $a_{cc}$, an element of $Q$ such that $a_{cc} \not = s$, the accepting state
7. $r_{ej}$, an element of $Q$ such that $r_{ej} \not = s$ and $r_{ej} \not = a_{cc}$, the rejecting state.

These seven items are enough to uniquely define a Turing Machine. That was a lot, so feel free to reread that section if you need to. It will be an important part of the following sections.

As it turns out, this is enough to compute everything that can be computed - including everything the device you're reading this on can do. Given enough time, a Turing Machine could perform the exact same set of things. Take a second to appreciate that: a machine, which can basically only read and write a single symbol at a time and has a fairly basic program, can do the exact same thing as any modern computer.

One of the most compelling pieces of evidence for this is that Turing Machines can simulate *other Turing Machines!* This is a concept called Universality: basically, we can create a Turing Machine that takes as input another Turing Machine, and some input to that other Turing Machine, and runs the exact same program the other Turing Machine does, down to accepting and rejecting the same strings!

#### Non-deterministic Turing Machines

Wait, hold on, surely there are ways the Turing Machine can be more powerful, right? What if we gave it more tapes? What if we let it move further? Or not at all? As it turns out, these variations of Turing Machines are not more powerful, in the sense that they cannot solve more problems - anything that they can solve, a regular Turing Machine can too. The way we know this is also Universality - we can create a regular Turing Machine that takes as input any of these modified Turing Machines and runs the exact same program.

However, some of these are more powerful in a different sense: they can run the same programs faster.

### Complexity Classes

At the center of this study is the concept of NP-Completeness, but before we understand that there's a few more things we need to know.

In the mid to late 1900's, when algorithm development really kicked off, computer scientists started to find that, as problem size grew, some problems were fast to solve, and some were slow. This difference was not so much a gradual increase, as one might expect, but really two separate classes of problems with vastly different times to solve. The first, the "easy" class of problems, was dubbed "Polynomial" problems. These are problems where the amount of time to find a solution for a certain input was some polynomial function depending on the size of the input. For instance, if we had an input of size $n$, then the amount of time would be something like $n^2$. 

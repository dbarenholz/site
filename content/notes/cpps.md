---
title: Competitive Programming and Problem Solving
description: "Primer on what competitive programming is."
date: "2018-06-03"
taxonomies:
    tags: ["honors"]
---

The mainstream idea of what programming is, is to write code for some kind of program.
This program can be many things.
Your web browser, e-mail clients, and applications such as Microsoft Word, Adobe After Effects and Angry Birds are just a few of the millions of possible examples.
Still, for every single one of these programs, there is one universal thing that holds.
Someone, _a programmer_, had to write some code in order for it to work.

However, programming itself doesn’t necessarily have to be necessarily for an application.
There can be other use cases for programming, for example, showing off your skills by generating a supposedly infinite number of prime numbers using only 53 bytes[^1] or by a one-liner that converts any decimal number X to binary[^2].
Another part of programming is that isn’t always necessarily serious.
A tribute to that fact are the programming languages called Whitespace (where one programs using spaces, tabs and enters) and Brainfuck (which works using only 8 characters).
Of course, a programmer doesn’t know all of these things by hard: we have Google and StackOverflow.
Perhaps the search engine can help out here to explain what competitive programming is?

{{ img(path="/img/google_defs.png", op="fit_width", width=1000, height=3000, caption="Searching on google.") }}

When combining both definitions, you may get that competitive programming is writing a bet- ter piece of code than others.
This isn’t too far from its actual meaning, but it raise the question what better code means.
Is the shortest piece of code the best? Or is it the most human- readable piece of code? There’s no real definition for better code, which is where you, as an aspiring programmer, decide to take action yourself and Google competitive programming.
You immediately stumble upon Wikipedia and find that competitive programming is _a mind sport usually held over the internet or a local network, involving participants trying to program according to provided specifications_.
I won’t continue to bore you by a Google query of specifications and then any other unknown words in this definition. I will tell you, however, that this explanation of competitive programming is pretty close to its actual meaning.
In essence, competitive programming is a contest or competition with multiple participants or competitors.
The tasks and setup of a contest may vary, but with all of them there is some kind of problem to solve, and the participants write their code, either in group or alone, and submit their solution to the problem.
The better piece of code is then the code that gives a better solution to the problem, or it is the piece of code that runs fastest (within constraints).

[^1]:`main(m, k){for( m%k-?:(k=m++);kˆ1? printf("%i|",m));}` in the C programming language.
[^2]: `(_$=($,_=[]+[])=>$?_$($»+!![],($&+!![])+_) _)(X)` using Javascript.
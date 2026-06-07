---
title: "everything is open source if you know reverse engineering (hack with me!)"
source: "https://www.youtube.com/watch?v=m0XAPRAOJ8A"
author:
  - "[[Low Level]]"
published: 2025-09-15
created: 2026-04-26
description: "Thanks again Hex Rays for sponsoring todays video! Get 50% off IDA Products at https://go.lowlevel.tv/idapro with code LOWLEVEL50Get 30% off IDA Training at ..."
tags:
  - "clippings"
---
![](https://www.youtube.com/watch?v=m0XAPRAOJ8A)

## Transcript

**0:00** · Today's video is sponsored by Hexrays.

**0:01** · Whether you're a cyber security analyst or somebody who likes to program, learning how to reverse engineer is the best way to get good at computers. And in this video, I'm going to teach you how to do just that. Now, while reverse engineering real software has ethical and legal implications, luckily, the world of hackers have these things called capture the flags where we have challenges that we can download that are meant to be reverse engineered. In this video, we're going to do the flag casino challenge from Hack the Box and use it to extract the flag out of a binary that doesn't want us to know the flag. The flag gets us points, and points make me feel good.

**0:33** · So, let's download the file and see what trouble we can get ourselves into. So, what is reverse engineering, right? What is the art of taking things apart? And why do we do it? Well, we download a program, right?

**0:43** · We can run the program and we'll see pretty quickly that it wants us to place our bets, right? Put in some number. And let's say that my bet is one. Well, unfortunately, my bet is incorrect. Now, it's going to activate the security system and tell me to please vacate. The problem is that I've received for the challenge a file called casino. And I'll move my fat head out of the way and I'll show you that the file casino is just an elf or a binary, which means that if I dump it into a hex editor, it's just a bunch of gobblelygook, right?

**1:10** · There's not a lot of information in here that a human can read because the program started as source code and has become machine code for the computer to understand. Reverse engineering is the art of taking that computer code and figuring out what the author intended and then using that to exploit a vulnerability in the program. So, what I'm assuming here is that the the bets I have to place are going to be the flag, right? If I put in the right bets, right, the right number sequence, that will give me the flag and give me the internet points. So, let's figure out what's going on with this program.

**1:40** · The way that I like to start any CTF challenge is first by doing file on that program to see what I'm dealing with.

**1:47** · It's a 64-bit least significant bit program. Basically, this just means that it was compiled with a modern compiler.

**1:53** · Nothing too crazy going on here. We can also do is run the strings program on this, right? What the strings program does is literally goes through the binary and finds five or more characters that are asky printable in a row. The reason that I do this is by reading the imports, the functions that the program uses. I get a general idea of what the program is going to do. We see the function exit. Obviously, we have to exit the program at some point. We also are going to use srand, which is a function that seeds randomness.

**2:19** · So, what I can already infer is that there's something probably wrong with the way that this program uses estrand that will allow me to predict the random numbers and maybe guess the flag. Again, this is a casino problem, right? It's you're at the casino. Casinos are random, but if they use S randing correctly, we could probably predict the randomness, maybe win some money at the flag casino. And obviously, they have puts, which prints to the screen, print f that puts to the screen, then you have scan f, the function to read data from the user. The strings program will only get us so far, right? Because, you know, it's not going to have a string that represents the code that's going on the program.

**2:50** · So, instead, what we have to do is now take our program and put it into a disassembly framework. The one we're going to use today is IDA Pro. I love IDA. I've used IDA for most of my hacking career. I actually learned to hack learns to re on IDA back in the day. Do new in IDA and we're going to go and find flag casino and then inside flag casino we'll open the problem in IDA. Now what you're looking at here is the disassembly of the program. So what IDA did is it took the problem and it found all of the computer instructions, the assembly instructions.

**3:21** · Now you don't need to know a lot of assembly to see what's going on here. Like obviously you need to know assembly to read this entire thing. But you can see very simple things are happening here. like it calls puts on the string, hello, welcome to Roboc Casino. It calls puts on the banner and calls puts on please place your bets. We know these things have to happen because they do happen when you run the program, right? I call these reality anchors where basically we know that the thing is printed to to the screen. So we can identify in the program where it is printed. Now what I'm ultimately interested in is this reading part, right?

**3:51** · We know based on the strings analysis that the program is going to read in some data via scanf. So what we need to do is find the call to scanf and then see where does it do things with that code. So if we look from the top of the main function, we kind of go down this graph. We can see the call to scanf happening here, right?

**4:09** · This is where the user is asked for their input. And so we want to kind of figure out what it's doing with this data. Let's walk the graph. So we print the, you know, the banner of the casino here. Then we compare some variable to hex 1 C. That's the number what 16 + 12 that's 28. What I'm going to say is this is going to be some counter. So I'm going to go into IDA and rename that to instead of RBP plus some random variable, I'm going to call it I. When I program in C, I think the variable is going to be something kind of like an iterator. So I'm going to call it I.

**4:39** · It's going to compare that to 28. If it's below 28, we're going to go down here and call print F on this little carrot. It's going to give us the prompt to put in data. And then we're going to do a percent C read. It's going to read a character. It's going to write it to this location variable five. Rename this uh user input so that I can easily read this and then it's going to do something with the user input. Now we could go through and trace the assembly here. I think it is a good idea to do that to learn, but for the sake of the challenge, we're actually going to use IDA's disassembler.

**5:06** · What this disassembler here is doing is giving us the best guess at what IDA thinks the C code that created this program looks like. Right? We don't actually have the C code, right? We can't have the CC code. What we can do is use the symbols from the functions like puts and srand and give a best guess on what the code would look like. And this is actually looking really good. We have i like some kind of for loop and then we have the input from the user going into a user input buffer.

**5:32** · So what the code does here is it takes the user input and it uses a user input to call sran and then it checks to see if the next call to random is equal to some check array. And we can see this is an array of characters that we have to hit. SRA rand and rand are a pair of functions that work together.

**5:52** · Let's go through and look at the man page for rand. The rand function returns a pseudo random integer in the range from zero to randmax inclusive. And the srand function sets its argument as the seed for a new sequence of pseudo random integers to be returned by rand. So effectively in this program the user input is given to SRAND to seed the

**6:15** · random number generator and then it gives a random number and we have to check it against that check value and if the number is not correct if it doesn't match that check value it'll say incorrect it'll you know alert the security system and then the the challenge will will exit with ag -2 right so what we have to do and what likely is the flag for this challenge is figure out what input goes into srand such that the return value from rand is equal to this number here. Okay.

**6:44** · And what's lucky for us is there's a very small search space we have to iterate over. Right? So you'll see here there's a percent c percent c and c is a character which means it is a single bite meaning there are only 256 combinations. So, what we can do is write a separate C program and generate all 256 combinations and then use that to programmatically figure out what the flag is for this challenge. Let's go write that code. Right now, we're going to use our trusty editor uh Vim, right?

**7:18** · So, we're going to make a new piece of code and we're going to say from standard io.h import and import also um standard lib because that's what you need to use I think srand. And then we'll do is we'll make our main function. Very simple stuff. And what we want to do is literally just create what is called a lookup table. Right? A lookup table is basically you have a key and an associated value. And the in this code the key is going to be all integers from 0 to 255. Right? Because that's going to be the percent C, right? The character input that we as a user can give it.

**7:47** · And the value is going to be the random number that's generated if we srand off of that C. way we do this in C is going to be int for int i equals z i less than 256 i ++ srand of i. So we'll seat it off of that i value and then we're going to do print f percent d col percent 08x where we'll print i and then rand of i. Right?

**8:14** · So we're going to get the corresponding i value and associated random number value that comes out of srand when we seed it off that value. Okay, we're going to use this to figure out what number do we have to put into the program to get it to say correct for one of our inputs. Let's go ahead and do that. We'll GCC the L. We'll run L.C.

**8:34** · And so now what you see is we have a list of characters on the left that give us the associated rand value on the right. So just to test this again, as hackers, we run little micro experiments to confirm or deny assumptions. If we go into the program, we'll go to the check array and we'll see that this first value, what is this? 244 B28BE.

**8:56** · Okay, let's see if we can find this 244 B28BE is associated to 72. Okay, now this is just the decimal number 72. What this means is that I have to go through and associate the ASI value to that number. How do we do that? In Python, we can just do character of 72 and the number is H. Right? So the character is H. Now we can confirm this by just running the program again. And if I type H as the first character, it should say that my entry is correct. Please continue. H correct. Okay. Now what does H stand for?

**9:31** · The the challenge is from hack the box. So likely this is going to be T, going to be B, right? And so we can go through and programmatically check this. Now what we can do is just extract this data right here, which is going to be the array of checks. And then all we have to do is map the input to the output, right? Map all of the keys that we have to their values. And then we can use that to derive the flag.

**9:53** · Now, what I want to do is extract this check array so that I can use the data here that's going to be somewhere else on my computer. I'm going to write a little Python script to programmatically go through this data and look it up via our lookup table. So using uh Python's IDAPython down here, I can use the get bytes command to get the data in the binary at a certain address.

**10:12** · We're going to do get me the data at hex 4080 which is you know the address right here and the size is going to be the difference between the two labels right so the the size will be hex 40 f4 minus hex 480

**10:30** · right and you'll see I got a bunch of gobbly gooks so I can use that and then dohex on that data to kind of hex encode it and make sure it's the data that I want you see that it ends in this 22 here which is the same as the 22 on the end of this variable it's little Indian so it's flipped Um but yeah, so we can take this and again this is Python so it emits it as a valid Python string that I'm able to now check. So we can put that into our Python script and go do it over there. Okay, so we kind of have two pieces of data we have to associate now and we'll we'll we'll map those all together by going through uh them in in Python.

**11:00** · So the way we do this in Python is let's call the script get flag.py. Uh the first one is we're going to call the check data, right? Is equal to this blob here which came directly out of IDA. And then we're going to have our L which is going to be equal to just the output of this program. Right? This L is going to be literally the association of the ASKI character to the S rand value. Right? So we'll go ahead and we'll do this. We'll just copy this whole thing. We could open this as a file, but I want to just make it simple for myself right now. And we're just going to copy it off the screen. And we'll call this L, which is going to be a uh a big string like this.

**11:34** · Now what I want to do is parse the L here. And I want to make the L into a dictionary where I can just look up this value and get the associated key value, right? Or the associated character value. So what we'll do is first of all say let is equal to L.Split on new line.

**11:50** · What this will do is take all the new lines out of this and make it a list of those. And then we'll do for um element in the L. I want to make a new L dictionary. This is an empty dictionary in Python. on a Python as a data structure that has a key and value association. So we'll have for element inl what we're going to do is do first of all element is equal to element.split on the colon here because again this colon is going to be the delimter between the the value and the key.

**12:18** · Then we're going to say um let d of int of element of one which is going to be this this value here from base 16 because we're taking the string value that is hex. So base 16 and converting that into an integer and making that the index is going to equal right the character of element or character of integer element of zero. Right?

**12:49** · This will get us the asky representation of this value. Now what I can do with this let of d is literally put in the random value from the check array and output the character. Right? And I can use that to get the entire flag in theory. Let's see what it looks like. Now kind of the harder part of this is I have kind of all this data. This this is the asy representation or the bite representation of the these numbers that are in IDA, right? IDA knows their numbers. Python does not know their numbers. So, we have to coersse the data to look like the appropriate um 32-bit values, right?

**13:21** · And the way we do this with a library called strruct in Python.

**13:25** · Strruct basically is a library that allows us to take data that is in some shape and convert it to data in another form. So we can do is take four bytes of characters and convert them to the associated integer value. Right? So what we'll do is we'll iterate over this value or we'll iterate over range. So for i in range zero len of check data four. What this gives us is basically a um a list of every four elements. We can cut it into pieces. Right?

**13:52** · So we'll do um random is equal to check data of i to i + 4 and we're going to do strruct.pack that data because it's packed binary data. We're going to unpack it little Indian as an integer. Right? What this basically does is say hey we know that data is the asky representation of little Indian numbers. So we're going to say that and then get that first element out. Let's print rand here just to see if we got the right number.

**14:22** · The first number should be okay. And there we go.

**14:26** · So 244 2b8e. So now we know we have the numbers as they appear in IDA and they are taken from that checked. What we can do with this is because we have that L set up where it associates that random value to the character. We can literally just print the lookup of the LD d off of rand.

**14:45** · And you'll see oh we're getting something asy printable hack the box rand. Okay. So let's go and instead of printing it, we're going to put it into a string. We'll do um flag equals blank and we'll do flag plus equals rand let d rand uh print flag.

**15:04** · There we go. Hack the box. Rand is very predictable. Guys, this is the art of of reverse engineering, right? It's taking a binary where we don't know what's going on under the hood. And then so we figure out what's going on under the under the hood by using tools like IDA and then using that to infer functionality and find vulnerabilities and make the program do things you want to do. Obviously guys, this is written for the sake of reverse engineering. Go try to solve this challenge on your own.

**15:32** · There are a bunch of good challenges on Hack the Box that I highly recommend that you you play with a little bit. Now guys, again, today's video is sponsored by Hexree, the company behind IDA Pro.

**15:40** · If you want to buy IDA, you can use my code lowlevel50 to get 50% off any IDA product as well as 30% off with lowlevel 30 on any of the Hexray Academy trainings to learn how to reverse engineer. Guys, IDA is the tool that I use personally to learn RE to learn hacking back in the day. And it is still today trusted 20 years later by cyber security experts, game hackers, and more. Guys, go play with IDA. If you want to learn to hack, this is the way to do it. I guarantee it. Anyway, Hexray, thanks again for sponsoring the video. I really appreciate it. All right, guys. Thanks for watching. I appreciate it. will see you in the next one.
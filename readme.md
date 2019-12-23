# Walking Bass Generator
by Max Hilsdorf

## Introduction

<img src="images/jazz_band.jpg" alt="jazz_band" width="300"/>

The **Walking Bass** is one of the key stilistic elements of Jazz.
You will find it, for example, in Frank Sinatras famous interpretation of the jazz standard 
["Fly Me to The Moon"](https://www.youtube.com/watch?v=ZEcqHA7dbwM).

Outside of Jazz, the Walking Bass found great usage in the 1950's Rock'n'Roll. 
Even in pop music, every now and then a Walking Bass finds its way into the top charts. Two examples are:
1. [The Beatles - All My Loving (1963)](https://www.youtube.com/watch?v=TSpiwK5fig0)
2. [Queen - Crazy Little Thing Called Love (1980)](https://www.youtube.com/watch?v=zO6D_BAuYCI)

## Music Theory

<img src="images/sheet_music.jpg" alt="sheet_music" width="300"/>

This article assumes a basic understanding of music theory. You should know what scales are and how to build chords from them. If you don't, you can still **use** the algorithm, in which case I suggest you skip to the next chapter.

In this chapter, I hope to give you a basic understanding of what a Walking Bass does and how it can be composed within a certain set of constraints. Remember that part of what makes music so interesting is that these constraints or rules can be broken in creative ways to produce new things. However, the algorithm won't. It will 100% obey to the rules it is given.

For simplicity purposes, the following ruleset it rather restrictive. Therefore, it can't showcase the Walking Bass in all its variety.

A Walking Bass is played in quarter notes, which - assuming a 4/4 time signature - setting leaves us with four notes to fill a single bar. The notes are chosen in a specific way from the musical material of the underlying chord. In this example, we will compose a Walking Bass over the first bar of Emin7.

<img src="images/notation example 1.PNG" alt="notation example 1" width="400"/>

1. **Beats 1 and 3**

On the strong beats 1 and 3, we will always play the root and fifth of the chord, respectively. As Jazz chord progressions are often highly complex, this helps to stabilize the harmonic movement. While you don't get to chose the note itself, you can still chose it's position. This lets you manipulate not only the single note pitches, but also the general direction of the basslines movement. Playing an E3 and a B3 would initiate an upward motion, while E4 and B3 would drive the bassline downwards. This will be important for choosing your next note.

3. **Beat 2**

Beat 2 will help manifest the basslines general motion created in the previous step. For an upwards motion, the third will be chosen, while a downwards motion will be supported by the seventh.

4. **Beat 4**

The fourth note helps transition into the next chord. Typical for Jazz, any chromatic movement into the next note (the root of the next chord) is allowed. In practice, this means that the last note will be one half step away from the root of the next chord. In our example, the Emin7 is followed by an A7, which allows for either an A# or an Ab (or G# and Bb if you will).

## Algorithm

<img src="images/algorithm.jpg" alt="code" width="300"/>

## Output

<img src="images/daw.PNG" alt="code" height="200"/>

## Discussion

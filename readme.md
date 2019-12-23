# Walking Bass Generator
by Max Hilsdorf

## Summary

This algorithm takes a chord progression as an input, composes a Walking Bass for it and generates a MIDI-File for further use, f.e. in a Digital Audio Workstation. Possible applications are 
1. (partially) automating Jazz style compositions for movies, advertisement or karaoke events.
2. creating learning material for music students.
3. creating backing tracks for drummers to practice any jazz standard.


## I - Introduction

<img src="images/jazz_band.jpg" alt="jazz_band" width="300"/>

The **Walking Bass** is one of the key stilistic elements of Jazz.
You will find it, for example, in Frank Sinatras famous interpretation of the jazz standard 
["Fly Me to The Moon"](https://www.youtube.com/watch?v=ZEcqHA7dbwM).

Outside of Jazz, the Walking Bass found great usage in the 1950's Rock'n'Roll. 
Even in pop music, every now and then a Walking Bass finds its way into the top charts. Two examples are:
1. [The Beatles - All My Loving (1963)](https://www.youtube.com/watch?v=TSpiwK5fig0)
2. [Queen - Crazy Little Thing Called Love (1980)](https://www.youtube.com/watch?v=zO6D_BAuYCI)

## II - Music Theory

<img src="images/sheet_music.jpg" alt="sheet_music" width="300"/>

This article assumes a basic understanding of music theory. You should know what scales are and how to build chords from them. If you don't, you can still **use** the algorithm, in which case I suggest you skip to the next chapter.

In this chapter, I hope to give you a basic understanding of what a Walking Bass does and how it can be composed within a certain set of constraints. Remember that part of what makes music so interesting is that these constraints or rules can be broken in creative ways to produce new things. However, the algorithm won't. It will 100% obey to the rules it is given.

For simplicity purposes, the following ruleset it rather restrictive. Therefore, it can't showcase the Walking Bass in all its variety.

A Walking Bass is played in quarter notes, which - assuming a 4/4 time signature - leaves us with four notes to fill a single bar. The notes are chosen in a specific way from the musical material of the underlying chord. In this example, we will compose a Walking Bass over the first bar of Emin7.

<img src="images/notation example 1.PNG" alt="notation example 1" width="400"/>

1. **Beats 1 and 3**

On the strong beats 1 and 3, we will always play the root and fifth of the chord, respectively. As Jazz chord progressions are often highly complex, this helps to stabilize the harmonic movement. While you don't get to pick the note itself, you can still choose it's position. This lets you manipulate not only the pitch of the individual notes, but also the general direction of the basslines movement. Playing an E3 and a B3 would initiate an upward motion, while E4 and B3 would drive the bassline downwards. This will be important for choosing your next note.

3. **Beat 2**

Beat 2 will help manifest the basslines general motion created in the previous step. We pick the third in case of an upwards motion and the seventh in case of a downwards motion.

4. **Beat 4**

The fourth note has a transitional character. It leaves the tonal material of the underlying chord and anticipates the following chord with a chromatic movement. In practice, this means that the last note will be one half step away from the root of the next chord. In our example, the Emin7 is followed by an A7, which allows for either an A# or an Ab (or G# and Bb).

## III - Algorithm

<img src="images/algorithm.jpg" alt="code" width="300"/>

### a) Notes and Chords

First, I created a'note' class with the attributes 'name' and 'midi_pitches'. The name is going to be used to read the chord progressions inputs. Every key on a keyboard has a 'pitch' value in the MIDI format. In order for the algorithm to choose between different instances of the same note, this attribute will store all available MIDI pitches as a list.

```python
class note:
    def __init__(self, name, midi_pitches):
        self.name = name
        self.midi_pitches = midi_pitches
```
In a dictionary, I stored the name of every note as well as all available MIDI pitches. I entered the lowest playable instance of every note wmanually and filled up the rest with the 'add_octaves' function.

```python
notes_dict = {"a" : [45], "a#" : [46], "b" : [47], "c" : [48], "c#" : [49], "d" : [50],
        "d#" : [51], "e" : [40], "f" : [41], "f#" : [42], "g" : [43], "g#" : [44]}

def add_octaves(note_pitch):
    for pitch in range(note_pitch[0], 64, 12):
        if pitch == note_pitch[0]:
            pass
        else:
            note_pitch.append(pitch)
            
for element in notes_dict.keys():
    add_octaves(notes_dict[element])
```

I created an object within the 'note' class for every one of the twelve notes.

```python
a = note(list(notes_dict.keys())[0], list(notes_dict.values())[0])
a_s = note(list(notes_dict.keys())[1], list(notes_dict.values())[1])
b = note(list(notes_dict.keys())[2], list(notes_dict.values())[2])
c = note(list(notes_dict.keys())[3], list(notes_dict.values())[3])
# [...]
```

Here is where we lay the foundation for programming the actual Walking Bass.
The 'Chord' class with its subclasses 'Major', 'Minor' and 'Dominant' gives the algorithm important tools for harmonic interpretation.
These 3 chord types have a 'chord_structure', which is based on the major-, minor- and dominant seventh chords, respectively. For example, a major seventh chord uses the root, the major third, the perfect fifth and the major seventh note of the scale. The chord structure (0, 4, 7, 11) consists of the indices, with which these notes could be accessed in a list of all 12 notes.

```python
class Chord:
    def __init__(self):
        self.name = "Chord"
        self.mode = self.assign_mode()
        
    def assign_mode(self):
        return Chord.Mode(self)

    class Major:
        def __init__(self, name, root):
            global notes
            self.name = name
            self.root = int(root)
            self.chord_structure = [0, 4, 7, 11]
            self.notes = apply_chord_structure(self.chord_structure, notes, self.root)
            
    class Minor:
        def __init__(self, name, root):
            global notes
            self.name = name
            self.root = int(root)
            self.chord_structure = [0, 3, 7, 10]
            self.notes = apply_chord_structure(self.chord_structure, notes, self.root)
            
    class Dominant:
        def __init__(self, name, root):
            global notes
            self.name = name
            self.root = int(root)
            self.chord_structure = [0, 4, 7, 10]
            self.notes = apply_chord_structure(self.chord_structure, notes, self.root)
 
 def apply_chord_structure(chord_structure, notes, root):
    chord_notes = []
    for element in chord_structure:
        if root + element < len(notes):
            chord_notes.append(notes[root + element])
        else:
            chord_notes.append(notes[-len(notes) + (root + element - len(notes))])
    return chord_notes
```

```python
a_maj = Chord.Major("A Major", 0)
a_min = Chord.Minor("A Minor", 0)
a_dom = Chord.Dominant("A Dominant", 0)

a_s_maj = Chord.Major("A# Major", 1)
a_s_min = Chord.Minor("A# Minor", 1)
a_s_dom = Chord.Dominant("A# Dominant", 1)

b_maj = Chord.Major("B Major", 2)
b_min = Chord.Minor("B Minor", 2)
b_dom = Chord.Dominant("B Dominant", 2)
# [...]
```

### b) Walking Bass

```python
import random
def write_bars(song, first_bar=True):
    bass_lines = []
    for i in range(len(song)):
        bassline = [0, 0, 0, 0]
        chord_notes = song[i].notes
        # first beat
        if i >= 1:
            bassline[0] = next_root
        else:
            bassline[0] = random.choice(chord_notes[0].midi_pitches)
        # third beat
        bassline[2] = random.choice(chord_notes[2].midi_pitches)
        # second beat
        if isinstance(song[i], Chord.Major):
            bassline[1] = bassline[0] + 4 if check_motion(bassline) == "up" else bassline[0] - 1
        if isinstance(song[i], Chord.Minor):
            bassline[1] = bassline[0] + 3 if check_motion(bassline) == "up" else bassline[0] - 2
        if isinstance(song[i], Chord.Dominant):
            bassline[1] = bassline[0] + 4 if check_motion(bassline) == "up" else bassline[0] - 2
        # fourth beat
        if (i+1) < len(song):
            next_root = check_nearest_pitch(bassline[2], song[i+1].notes[0].midi_pitches)
            bassline[3] = random.choice([next_root + 1, next_root - 1])
        else:
            bassline[3] = bassline[2]
        
        bass_lines.append(bassline)

    return bass_lines
    
def check_motion(bassline):
    return "up" if bassline[0] < bassline[2] else "down"
    
def check_nearest_pitch(current_pitch, note2):
    for i, pitch in enumerate(note2):
        if abs(current_pitch-pitch) <= 6:
            return pitch
        elif i == len(note2)-1:
            for i, pitch in enumerate(note2):
                if abs(current_pitch-pitch) <= 10:
                    return pitch
```

### c) MIDI

```python
from midiutil.MidiFile import MIDIFile
MyMIDI = MIDIFile(1)
track = 0
time = 0
MyMIDI.addTrackName(track,time,"Sample Track")
MyMIDI.addTempo(track, time, 120)
```

```python
for i, j in enumerate([list_of_pitches]):
    MyMIDI.addNote(track = 0, channel = 0, pitch = j, time = i,
               duration = 1, volume = 100)
```

```python
binfile = open("fly me.mid", "wb")
MyMIDI.writeFile(binfile)
binfile.close()
```

## IV - Output

<img src="images/daw.PNG" alt="code" height="200"/>

## V - Discussion

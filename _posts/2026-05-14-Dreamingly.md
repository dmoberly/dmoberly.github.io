---
title: Dreamingly
date: 2026-05-15 01:10:00 -500
categories: [Music]
tags: [music, video, coding]
---

<iframe width="560" height="315" src="https://www.youtube.com/embed/i-UwsM8KfL4?si=BdANl9ufmldfSx6T" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

# First Things First
This is my first original song made with Strudel and my first video on Youtube! I've experimented with Strudel before and made a simple version of the song Equalizer by The Midnight. I learned a lot more making this original song though. For one, the creative process is the hardest part. The drums and bass part were easy, but I spent so long trying to come up with a tune that didn't sound like a nursery rhyme. Then I spent even more time tweaking it so it wasn't too repetitive or busy. Eventually, I had something I liked and I moved on to adding the drum fills and effects.  

Once I had it all ready, I had to figure out how to showcase it. I decided I should make a video so you can see how it works. That wasn't as straight forward as I thought though. First, I had to figure out how I was going to actually "play" the song. Normally, Strudel is coded live, meaning the song is crafted in real time while it is playing. I'm not that good yet, so I just orchestrated the parts to make it more dynamic, as dynamic as a four bar repeating pattern can really be. Then, I went to screen record it and realized that it doesn't natively capture system audio, so I had to install an audio loopback driver which was luckily pretty trivial. It took me a few takes to get it all right, but eventually I had a file I could upload as my first Youtube video.

# Code Breakdown
I'll breakdown each part of the song and explain what all the different functions do, so you can get an idea of what all went into making it.
```js
setcpm(150/4) // Set cpm (cycles per minute)
```
The **.setcpm()** function is how you set the tempo, which is managed in cycles per minute. In this case, the 150/4 means that it will play in 4/4 time at 150 BPM. (beats per minute)

### Drums
```js
$drums: // $ is used to separate patterns
  s(`<[bd@3 bd sd bd bd@6 sd bd@3]
      [bd@3 bd sd bd bd@5 bd sd bd@3]>`) // Sequence
  .bank("yamaharx5") // Sound (instrument)
  .slow(2) // Plays at half speed
  .duck("2:3").duckattack(".1").duckdepth(.7) // Sidechain
  .room(.6).roomsize(4) // Reverb
  .color("#63bdc5")
  ._scope()
```

I'll try to break down the different parts. Basically, the things in the **s()** function are the "notes" or "sounds" since it is a drumkit. Bd is bass drum, sd is snare drum. The @n means it plays that note over n beats. The [] brackets are the bars and the <> make it alternate between the different bars in sequence.  

The next parts are all different functions that determine how those notes sound. The **.bank()** function lets you set the instrument from a preset list. (you can also import your own) The **.slow()** function lets you slow down the tempo for the pattern by a set factor.  

The **.duck()** functions are how you set up a sidechain. It makes the other patterns get quieter when the drums hit, which gives the effect that the drums are punching through or that the other sounds are ducking under the drum hits.  
The **.room()** functions are for reverb, and the **.color()** and **._scope()** functions are just for visuals. The color changes the highlight color and the color of the scope. The scope is just the line that shows below that lets you visualize the sound.

### Bassline

```js
$bass:
  n(`<[0!6 1!2] [1!8] [2!6 1!2] [1!8]
      [0!6 1!2] [1!8] [-1!6 0!2] [0!8]>`) // Sequence
  .scale("G1:major") // Musical key the notes play in
  .sound("supersaw") // Sound (instrument)
  .lpf(500).lpenv("2") // Lowpass filter controls
  .gain(1.5) // Volume
  .orbit(2) // Effects channel
  .color("#f4b5e5")
  ._punchcard() // Piano visualizer
```

Similar to the drums, the **n()** function holds the sequence of notes. The first numbers are the notes in the relevant key. They go from 0-7 so in the key of G major, 0 is G, 1 is A, 2 is B, etc. You can go outside the octave with negatives and n >7. The exclamation marks make the note repeat by the following number. So 8!3 is the same as 8 8 8.  

The **.scale()** function sets your key. G1:major means the key is G major with the base in the first or lowest octave.
The **.lpf()** function is a lowpass filter which cuts off frequencies above a set threshold, meaning only the low parts of the sound are heard. The **.lpenv()** function sets the modulation depth of the lowpass filter envelope. The **.gain()** function controls the volume of the pattern.  

The **.orbit()** function is something that I actually just started to understand during this project. Basically, it is an effects channel. Without setting different channels on your patterns, certain effects like reverb are shared between them and can cause weird issues. It is also what is actually being targeted in the **.duck()** function. 

### Lead

```js
$lead:
  n(`<[~ -1 1 -1 0 4@2 4@2 4 5 4@5]
     [~@2 4 3 2 0@3 -3@3 -2@5]
     [~ 0 7 6 4 2@2 1@2 0@2 1@2 2@3]
     [-1@3 1@3 0@6 ~@4]>`) 
  .slow(2).scale("G4:major")
  .sound("gm_pad_warm, gm_fx_crystal")
  .room(.8).roomsize(8)
  .delay(.6).delaytime(8).delayfeedback(.5) // Echo effect
  .lpf(sine.range(1000, 10000).slow(14)).lpenv("2") // Automated lowpass filter
  .gain(.4).orbit(3)
  .color("#e7b5db")
  ._punchcard()
```

The lead synth plays the melody. In the sequence, the ~ is a rest, or a pause, and the rest we already went over. Notice in the **.sound()** function that there are actually two sounds separated by a comma. This plays both sounds together and allows for some interesting possibilities.  

The **.delay()** functions add an echo effect and let you control the parameters of it like how long it lasts and how potent it is.  

Notice that I've automated the **.lpf()** function in this one with a sine wave function. It dynamically changes the lowpass threshold at an adjustable rate in the set range. This gives it a sort of slow swishing effect.

### Toms

```js
$toms:
  wchooseCycles(["~",5], ["~@13 ht mt lt",1], 
                ["~@8 ht@3 mt@3 lt@2",1]) // Randomized drum fills
  .s().bank("bossdr220")
  .slow(2).room(.6).roomsize(4).orbit(4).gain(.3)
  .color("#d4bfe2")
  ._scope()
```

I added these in last and I really like what they can add to mix things up. Ht is the high-tom, mt is the mid-tom, and lt is the low-tom on the drumkit. This **wchooseCycles()** function is one I just learned about during this project. It randomly chooses between the given patterns but you can weight the probability of each. So in this case, I wanted the little fills on the toms to only play every once in a while. I came up with two simple but different patterns and assigned them a weight of 1, then gave the blank pattern ("~", just a single rest) a weight of 5. That means that it is 5 times more likely to not play anything than it is to play one of the fills. 

## Full Code

Here is the full song! You can go to https://strudel.cc and paste it in to play around with it for yourself. 

```js
setcpm(150/4)

$bass:
  n(`<[0!6 1!2] [1!8] [2!6 1!2] [1!8]
      [0!6 1!2] [1!8] [-1!6 0!2] [0!8]>`)
  .scale("G1:major").sound("supersaw")
  .lpf(500).lpenv("2")
  .gain(1.5).orbit(2)
  .color("#f4b5e5")
  ._punchcard()

$drums: 
  s(`<[bd@3 bd sd bd bd@6 sd bd@3]
      [bd@3 bd sd bd bd@5 bd sd bd@3]>`)
  .bank("yamaharx5")
  .slow(2)
  .duck("2:3").duckattack(".1").duckdepth(.7)
  .room(.6).roomsize(4)
  .color("#63bdc5")
  ._scope()

$fill:
  s(`<[~]
      [~]
      [~]
      [ht ht bd mt mt bd lt lt bd lt lt bd sd bd sd bd]>`)
  .bank("bossdr220")
  .slow(2).room(.8).roomsize(2).orbit(4).gain(.3)
  .color("#d4bfe2")
  ._scope()

$hihat:
  s("hh*8").bank("rolandd70").gain(.2).orbit(2)
  .color("#a0cadd")
  ._scope()

$lead:
  n(`<[~ -1 1 -1 0 4@2 4@2 4 5 4@5]
     [~@2 4 3 2 0@3 -3@3 -2@5]
     [~ 0 7 6 4 2@2 1@2 0@2 1@2 2@3]
     [-1@3 1@3 0@6 ~@4]>`)
  .slow(2).scale("G4:major").sound("gm_pad_warm, gm_fx_crystal")
  .room(.8).roomsize(8)
  .delay(.6).delaytime(8).delayfeedback(.5)
  .lpf(sine.range(1000, 10000).slow(14)).lpenv("2")
  .gain(.4).orbit(3)
  .color("#e7b5db")
  ._punchcard()

$toms:
  wchooseCycles(["~",5], ["~@13 ht mt lt",1], 
                ["~@8 ht@3 mt@3 lt@2",1])
  .s().bank("bossdr220")
  .slow(2).room(.6).roomsize(4).orbit(4).gain(.3)
  .color("#d4bfe2")
  ._scope()
```
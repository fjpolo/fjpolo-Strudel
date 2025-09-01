# 🎶 Strudel Cheatsheet

Strudel is a live-coding music language based on JavaScript and inspired by TidalCycles. It's designed to run in a web browser, letting you create and manipulate musical patterns in real-time.

## Core Concepts

- Patterns: The fundamental building blocks. A pattern is a sequence of musical events over a period of time.

- Mini-notation: A simple, powerful syntax for creating rhythmic and melodic patterns using strings.

- Chaining: Functions are often "chained" together using a dot (.) to modify a pattern, similar to adding audio effects.

## Basic Syntax

- Play a sound: Use s("...") or sound("..."). Names like bd (bass drum), sd (snare drum), and hh (hi-hat) are built-in.

    - `s("bd sd hh")` — Plays bass, snare, then hi-hat in sequence.

- Play a note: Use note("..."). The values can be pitch names or numbers.

    - `note("c e g")` — Plays a C major chord.

- Play multiple patterns at once: Use stack().

    - `stack(s("bd sd"), s("hh*8"))` — Plays a bass/snare pattern and a rapid hi-hat pattern simultaneously.

## Mini-notation and Rhythmic Control

- Sub-sequences: Use [...] to nest patterns and treat them as a single step.

    - `s("bd [sd cp] hh")` — Plays bass, then snare and clap together, then hi-hat.

- Multiplication (*): Repeats the pattern faster.

    - `s("bd*2")` — Plays the bass drum twice in the space of one beat.

    - `s("[bd sd]*2")` — Plays "bd sd bd sd" over two beats.

- Division (/): Stretches the pattern over a longer duration.

    - `s("[bd sd]/2")` — Plays "bd sd" over two beats, slowing it down.

- Parallelism within a string: Use <...> to play multiple events in a single step.

    - `note("<c [e g]>")` — Plays a C note, followed by an E and G played together.

## Common Functions & Transformations

Functions are typically chained to the end of a pattern using a dot (.).

- `fast(n)`: Speeds up the pattern by a factor of n.

    - `s("bd sd").fast(2)`

- `slow(n)`: Slows down the pattern by a factor of n.

    - `s("bd sd").slow(2)`

- rev(): Reverses the pattern.

    - `s("bd sd hh").rev()` — Plays "hh sd bd".

- `every(n, function)`: Applies a function every n cycles.

    - `s("bd sd").every(4, p => p.rev())` — Reverses the pattern every 4th cycle.

- `degrade()`: Randomly skips some events in the pattern.

    - `s("bd sd hh").degrade()`

- `pan(value)`: Pans the sound left or right. A value of -1 is full left, 1 is full right.

    - `s("bd sd").pan("<-1 1>")` — Pans the bass drum left and the snare drum right.

- `cut()`: A simple low-pass filter to make sounds more muffled.

    - `s("bd*8").cut(0.5)`

- `room()`: Adds reverb to the sound.

    - `s("bd").room(0.8)`


## First Sounds - Recap

| Concept | Syntax | Example |
|---------|--------|---------|
|sequence|space|`sound("bd bd sd hh")`|
|Sample numbers|:x|`sound("hh:0 hh:1 hh:2 hh:3")`|
|Rest|- or ~|`sound("metal - jazz jazz:1")`|
|Alternate|<>|`sound("<bd hh rim oh bd rim>")`|
|Sub-sequence|[]|`sound("bd wind [metal jazz] hh")`|
|Sub-Sub-sequence|[[]]|`sound("bd [metal [jazz [sd cp]]]")`|
|Speed-up|*|`sound("bd sd*2 cp*3")`|
|Parallel|,|`sound("bd*2, hh*2 [hh oh]")`|

Mini-Notation:

| Name | Description | Example |
|-------|-------------|---------|
|sound|plays the sound of the given name|`sound("bd sd [- bd] sd")`|
|bank|selects the sound bank|`sound("bd sd [- bd] sd").bank("RolandTR909")`|
|setcpm|sets the tempo in cycles per minute|`setcpm(45); sound("bd sd [- bd] sd")`|
|n|select sample number|`n("0 1 4 2 0 6 3 2").sound("jazz")`|


## First Notes- Recap

| Concept | Syntax | Example |
|---------|--------|---------|
|Slow down|/|`note("[c a f e]/2")`|
|Alternate|<>|`note("c a f <e g>")`|
|Elongate|@|`note("c@3 e")`|
|Replicate|!|`note("c!3 e")`|

new functions:

| Name | Description | Example |
|-------|-------------|---------|
|note|set pitch as number or letter|`note("b g e c").sound("piano")`|
|scale|interpret n as scale degree|`n("6 4 2 0").scale("C:minor").sound("piano")`|
|$:|play patterns in parallel|`$: s("bd sd")`|
|$:|play patterns in parallel|`$: note("c eb g")`|


## First Effects - Recap

| Name | Example |
|------|---------|
|lpf|`note("c2 c3 c2 c3").s("sawtooth").lpf("<400 2000>")`|
|vowel|`note("c3 eb3 g3").s("sawtooth").vowel("<a e i o>")`|
|gain|`s("hh*16").gain("[.25 1]*2")`|
|delay|`s("bd rim bd cp").delay(.5)`|
|room|`s("bd rim bd cp").room(.5)`|
|pan|`s("bd rim bd cp").pan("0 1")`|
|speed|`s("bd rim bd cp").speed("<1 2 -1 -2>")`|
|signals|`s("hh*16").gain  (saw)`|
|range|`s("hh*16").lpf(saw.range(200,4000))`|

## Pattern Effects - Recap

| Name | Description | Example |
|-------|-------------|---------|
|rev|reverse|`n("0 2 4 6 ~ 7 9 5").scale("C:minor").rev()`|
|jux|split left/right, modify right|`n("0 2 4 6 ~ 7 9 5").scale("C:minor").jux(rev)`|
|add|add numbers / notes|`n("0 2 4 6 ~ 7 9 5".add("<0 1 2 1>")).scale("C:minor")`|
|ply|speed up each event n times|`s("bd sd [~ bd] sd").ply("<1 2 3>")`|
|off|copy, shift time & modify|`s("bd sd [~ bd] sd, hh*8").off(1/16, x=>x.speed(2))`|

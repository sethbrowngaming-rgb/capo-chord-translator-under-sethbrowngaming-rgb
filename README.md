# Capo Chord Translator

A single-file web tool (`index.html`, no install, no server) that converts chord names into a custom **barre-shape + fret** notation, adjusted for any capo position. Open `index.html` in a browser, paste your notes, and copy the result.

The notation is a personal system created by the author, not a standard one. Note names are intentionally **not** the real chord names.

## How the translation works

Each chord has a rule written as `CHORD=SHAPE+FRETS`. The rule describes the chord **with the capo at 0**.

```
G=F+3     C=B+3     D=B+5     Dm=Bm+5
E=F+0     Em=F#+0   Am=Bm+0   A=B+0
```

With a capo, **add the capo number to the frets**:

```
output = SHAPE + (FRETS + CAPO)        (written without the plus sign)
```

Example, capo 4: `D = B+5` becomes **B9**. `Em = F#+0` becomes **F#4**. `G = F+3` becomes **F7**.

| Chord | Rule (capo 0) | Capo 4 output |
|-------|---------------|---------------|
| G  | F+3  | F7  |
| C  | B+3  | B7  |
| D  | B+5  | B9  |
| Dm | Bm+5 | Bm9 |
| E  | F+0  | F4  |
| Em | F#+0 | F#4 |
| Am | Bm+0 | Bm4 |
| A  | B+0  | B4  |

The rules are editable in the page. Add your own lines in the same `CHORD=SHAPE+FRETS` format.

## PWC (power chord) notation

`PWC(SHAPE/FRET)`, for example `PWC(B/8)`. A normal chord for 7-string guitars, played on the top string when on a 6-string. The shape can be F, F# or B. The tool checks and passes PWC tokens through; it has no conversion rule for them.

## The default note header (and what it means)

Every note starts with this header (`examples/default-header.txt`). The tool keeps it exactly as written. Only the `Capo:`, `Tuning:` and `Reverb/Slowed/Live` lines may change per song.

```
1=Top String Closest
6=Bottom String Furthest
(String / Note)
Capo: 0
Tuning: STAND
*=Fix or Finish
------------------------------
PWC=Power Chord
ST=Pick At Same Time
X=Repeat
-----------------------------
Reverb/Slowed/Live=-?
-----------------------------
```

| Line | Meaning |
|------|---------|
| `1=Top String Closest` | String 1 is the top string, the one closest to you. |
| `6=Bottom String Furthest` | String 6 is the bottom string, the furthest from you. |
| `(String / Note)` | Tab notes are written `(string/fret)`. `(2/5)` is string 2, note 5. |
| `Capo: N` | Capo position. The tool reads this number and adds it to every chord's frets. |
| `Tuning: STAND` | Tuning of the guitar (standard here). Change it if the song is tuned differently. |
| `*=Fix or Finish` | A `*` marks a part that still needs fixing or finishing. |
| `PWC=Power Chord` | PWC means power chord. |
| `ST=Pick At Same Time` | ST means pick those notes at the same time. |
| `X=Repeat` | X means repeat (for example `-X2-` is repeat twice). |
| `Reverb/Slowed/Live=-?` | Marks whether the version is reverb, slowed or live (fill in the dash). |

## Full dictionary header (optional)

The default header above is the one used unless you ask for more. For songs that need the full legend, pick **Full dictionary** in the "Header type" menu (`examples/full-dictionary-header.txt`). It adds a Gain and Pedal line, more markers, technique symbols, and a blank song layout (Intro, Verse, Pre-chorus, Chorus, Bridge/breakdown/SOLO, Outro) to fill in. Header lines are copied through unchanged, so only the chords are translated.

| Line | Meaning |
|------|---------|
| `Capo: ? Gain: ?` | Capo position (the tool reads the number) and the gain/distortion level. |
| `Pedal: ?` | Pedal or effect used. |
| `+=Update Chords` | Chords in that part still need updating. |
| `(Ŝ)=Fill Out Solo` | The solo still needs to be filled out. |
| `(III)Fill Out Info` | Song info still needs to be filled out. |
| `RB=Bend` | Bend the string. |
| `H=Hammer-on` | Hammer-on. |
| `PO=Pull-off` | Pull-off. |
| `SL=Slide` | Slide between notes. |
| `FP=Fast Pick` | Fast picking. |
| `S=Strum` | Strum. |
| `T=Tab / Pick` | Play as tab / pick note by note. |
| `W=Whammy Bar` | Use the whammy bar. |
| `M=Mute...` | Mute with the fretboard hand and hit with the picking hand. |
| `~=Vibrato` | Vibrato. |

`*`, `PWC`, `ST`, `X` and `Reverb/Slowed` mean the same as in the default header.

## Chord shapes (what they look like)

Strings are listed 6 to 1 (left to right on the diagram). The page draws these diagrams for you in the "Chord shapes" card.

### F shape (barre)

```
 6 5 4 3 2 1   (strings)
 1-------1 1   finger 1 = bar across all strings
       2       finger 2 = string 3, one fret above the bar
   3 4         fingers 3 and 4 = strings 5 and 4, two frets above the bar
```

| String | 6 | 5 | 4 | 3 | 2 | 1 |
|--------|---|---|---|---|---|---|
| Fret vs. bar | +0 | +2 | +2 | +1 | +0 | +0 |
| Finger | 1 | 3 | 4 | 2 | 1 | 1 |

**Number rule (all shapes):** the number after the shape is the fret the bar sits on: `F1` = bar at fret 1, `F3` = bar at fret 3, and so on.

**Zero rule:** when the number is 0 (a +0 rule), remove the bar (finger 1) and play only the remaining notes: strings 6, 2 and 1 are open, finger 2 stays on string 3, fingers 3 and 4 stay on strings 5 and 4.

### F# shape (barre)

```
 6 5 4 3 2 1   (strings)
 1-----1 1 1   finger 1 = bar across all strings
   3 4         fingers 3 and 4 = strings 5 and 4, two frets above the bar
```

| String | 6 | 5 | 4 | 3 | 2 | 1 |
|--------|---|---|---|---|---|---|
| Fret vs. bar | +0 | +2 | +2 | +0 | +0 | +0 |
| Finger | 1 | 3 | 4 | 1 | 1 | 1 |

Same number rule as F: `F#1` = bar at fret 1, `F#3` = bar at fret 3. At 0 (`F#0`) the bar comes off: strings 6, 3, 2 and 1 are open and fingers 3 and 4 stay on strings 5 and 4.

More shapes (B, Bm, PWC) will be added here.

## How the tool treats your text

- Chord lines (for example `Dm,A,Em,G`) are translated. Chords can be separated by spaces, commas, `|` or dashes.
- Everything else (header, section names like VERSE, solo tab lines) is copied through unchanged.
- If the text has a `Capo: N` line, that number is used. Otherwise the capo box is used, and the standard header is added if the checkbox is on.
- Chords with no rule are shown in red instead of guessed.
- Solo tab numbers like `(2/5)` are not changed.

## Example

`examples/example-input.txt` is a full note with capo 4 and `examples/example-output.txt` is the translation.

Input chords for the verse: `Dm,A,Em,G`  ->  Output: `Bm9, B4, F#4, F7`

## Self-check

The page includes a table that tests the original rules (capo 0). All should show PASS.

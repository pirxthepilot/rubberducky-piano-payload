# HAK5 Rubber Ducky Piano Player

Script that converts music notation written in tracker-like style into payload code for the [HAK5 USB Rubber Ducky](https://shop.hak5.org/products/usb-rubber-ducky).

## Usage

```
usage: piano_payload.py [-h] NOTES PAYLOAD

positional arguments:
  NOTES       Path to notes file
  PAYLOAD     Save payload code to this file

options:
  -h, --help  show this help message and exit
```

First, create the notes file. Then run the tool which creates the payload code file. You can then copy and paste that code into the HAK5 [payload encoder](https://encoder.hak5.org).

## Notes file format

The notes file looks like this:

```
50ms
https://www.onlinepianist.com/virtual-piano
c-3=q c#3=2 d-3=w d#3=3 e-3=e f-3=r f#3=5 g-3=t g#3=6 a-3=y a#3=7 b-3=u c-4=i c#4=9 d-4=o d#4=0 e-4=p f-4=z f#4=s g-4=x g#4=d a-4=c a#4=f b-4=v c-5=b c#5=h d-5=n d#5=j e-5=m f-5=, f#5=l g-5=. g#5=; a-5=/ a#5='

d-3  f#3  e-5
d-3  f#3  e-5
---  ---  ---
d-3  f#3  e-5
---  ---  ---
d-3  f#3  c-5
d-3  f#3  e-5
---  ---  ---
g-3  b-3  g-5
---  ---  ---
---  ---  ---
---  ---  ---
g-3  ---  g-4
```

* Line 1: Tick duration (more on this later)
* Line 2: URL of the piano (or other virtual instrument) app
* Line 3: Space-separated note maps. Left hand side is the actual note, right hand is the corresponding keyboard keypress
* Line 4: Leave blank
* Line 5 until end: Notes and rests in tracker-like notation

### About the tracker-like notation

I thought an easy way to write music is something reminiscent of [tracker interface](https://en.wikipedia.org/wiki/Music_tracker) where notes are played by rows. All notes in the same row are played "simultaneously" (or in this case, gives the illusion that it is).

Each row plays at approximately the tick duration (there will be some accumulated delay depending on number of keys held at the same time, etc.).

You would typically make one row equivalent to the duration of a 16th note in your piece, but this is by no means the rule.

The tracker can have one or more columns ("tracks"), but I have only tested it with up to three. Four or more might introduce significant enough delays to make the tempo unstable.

Each element in the tracker is three characters long. A natural note looks like `d-3` and a sharp `d#3`. Flats can be used too as long as it's declared in the note maps.

A rest is represented as `---`.

There should always be **two spaces** between notes or rests in the same row.

For a very simple example, this is the first two bars of _Twinkle, Twinkle, Little Star_:

```
c-4
c-4
g-4
g-4
a-4
a-4
g-4
---
```

In this case each row is equivalent to a quarter note.

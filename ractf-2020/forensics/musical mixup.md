# Musical Mixup
For this challenge, we see we have an audio file. From the description, we are told its a midi file, so we begin to look at how this can be useful. 

## Python
Upon a small bit of research, I find there is a python module called mido that looks useful for working with midi files.

**Mido is a library for working with MIDI messages and ports. It's designed to be as straight forward and Pythonic as possible:**

The github for mido describes it as the following above message. So I decide to try this by using `pip install mido` and then i begin to code a solution.

**NOTE:** The following is python3, so be sure to run it with `python3 file.py` or instead with the shebang shown below. However, remember to chmod the file to be able to run it as so: `./file.py`.

```python
#!/usr/bin/python3

import mido
midi = mido.MidiFile("challenge.mid")   #this opens the file and saves it to the midi variable.
for msg in midi:
    if not msg.is_meta and msg.type == 'note_on' and msg.velocity != 0 and msg.time == 0:
        print(msg.velocity) #this looks for the message within the midi file
```

The above script appears to work and provides me with the following: (Apologies for the long scroll)
```
114
97
99
116
102
123
102
53
48
99
49
51
116
121
95
108
51
118
101
108
95
53
116
51
103
33
125
114
97
99
102
123
102
49
116
121
95
51
118
101
95
51
103
125
114
99
116
102
102
53
99
49
51
116
108
51
101
95
116
51
103
33
125
99
116
123
53
99
51
121
95
51
118
101
116
103
33
125
97
99
116
123
48
99
51
116
95
108
51
101
108
53
116
51
103
114
97
116
123
53
48
99
49
51
95
108
118
108
53
51
33
125
97
99
102
123
102
48
99
49
51
116
121
108
51
101
108
95
116
51
103
33
125
114
51
97
99
121
116
123
53
99
108
49
51
121
118
108
51
108
118
108
53
51
33
114
97
99
116
102
123
102
53
48
99
49
51
116
121
95
108
121
51
118
108
101
108
95
53
116
51
103
103
33
125
114
97
```

I then decide to decode these values from decimal to ascii and i get the following:
```
ractf{f50c13ty_l3vel_5t3g!}racf{f1ty_3ve_3g}rctff5c13tl3e_t3g!}ct{5c3y_3vetg!}act{0c3t_l3el5t3grat{50c13_lvl53!}acf{f0c13tyl3el_t3g!}r3acyt{5cl13yvl3lvl53!ractf{f50c13ty_ly3vlel_5t3gg!}ra
```

From the above message, we see that we get  the flag as:
```
ractf{f50c13ty_l3vel_5t3g!}
```
And so we have the challenge solved!

> Created on the 9th June by Christopher Harris (@cjharris18)

Decode the values produced from this code from decimal to ascii

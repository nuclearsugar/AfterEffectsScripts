# Wiggle Expressions in After Effects

## Optional Advanced Parameters of Wiggle
```
seedRandom(1, true);
wiggle(freq, amp, octaves = 1, amp_mult = 0.5, time = time)
```

## Use the wiggle command to randomize an attribute
- Explanation: seedRandom(1, true)
  - 1 is the seed. Set to the same seed to have different layers follow the same seed. Or set to a different seed to have each layer follow its own seed.
- Explanation: wiggle(4,100)
  - The 1st number defines the number of times the layer will wiggle per second. This case it moved 4 times in 1 second. The 2nd number 100 is to set the range of movement.
- You can have both a wiggle expression and have keyframes laid down, and then AE will automatically sum the movements of both aspects into the visible animation. Really useful!

```
seedRandom(1, true);
wiggle(4,10)
```

## Wiggle the scale and affect both X and Y in tandem
```
seedRandom(654, true);
w = wiggle(0.5, 50)[0]; // Get the X component of the wiggle
[w, w]; // Apply the same value to both X and Y
```

## Wiggle position Y BUT NOT X
```
seedRandom(1, true);
x = value[0]; // Keep the original X position
y = wiggle(4, 100)[1]; // Apply wiggle only to Y
[x, y]
```

## Wiggle position X BUT NOT Y
```
seedRandom(1, true);
x = wiggle(4, 100)[0]; // Apply wiggle only to X
y = value[1]; // Keep the original Y position
[x, y]
```

## Triggering an opacity pulse from a marker on another layer
```
m = thisComp.layer("ExampleLayerName").marker;
n = 0;
if (m.numKeys > 0){
  n = m.nearestKey(time).index;
  if (m.key(n).time > time) n--;
}
if (n > 0)
  linear(time - m.key(n).time,0,.5,100,0)
else
  0
```

## Opacity randomly turn on/off
```
seed = index; // or any fixed number for repeatable pattern
seedRandom(seed + Math.floor(time * 10), true); // 10 = flickers per second
random(0, 1) > 0.5 ? 100 : 0;
```



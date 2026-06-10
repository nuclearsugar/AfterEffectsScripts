# Wiggle Expressions in After Effects

## Use the wiggle command to randomize an attribute
- Explanation: `seedRandom(937, true)`
  - 937 is the seed. Use the same seed if you want multiple layers (or comps) to share identical random behavior, or use different seeds if you want each layer (or comp) to generate its own independent random pattern.
- Explanation: `wiggle(4,100)`
  - The first value controls how many times per second the layer changes its random position (in this case, 4 times per second).
  - The second value defines the amplitude of the movement, meaning the layer can deviate up to 100 units from its original position.
- You can have both a wiggle expression applied and have keyframes laid down on a layer, and then AE will automatically sum the movements of both aspects into the visible animation. Really useful!
```
seedRandom(937, true);
wiggle(4,10)
```

### Optional Advanced Parameters of Wiggle
```
seedRandom(1, true);
wiggle(freq, amp, octaves = 1, amp_mult = 0.5, time = time)
```

## Wiggle the scale and affect both X and Y in lockstep
This expression applies a repeatable wiggle that updates at 0.5 times per second with an amplitude of 50, using the same randomly generated value for both the X and Y components so they move together uniformly.
```
seedRandom(654, true);
w = wiggle(0.5, 50)[0]; // Get the X component of the wiggle
[w, w]; // Apply the same value to both X and Y
```

## Wiggle position Y but not X
This expression preserves the layer's original X position while applying a repeatable wiggle to the Y position that updates 4 times per second with an amplitude of 100.
```
seedRandom(1, true);
x = value[0]; // Keep the original X position
y = wiggle(4, 100)[1]; // Apply wiggle only to Y
[x, y]
```

## Wiggle position X but not Y
This expression preserves the layer’s original Y position while applying a repeatable wiggle to the X position at 4 times per second with an amplitude of 100.
```
seedRandom(1, true);
x = wiggle(4, 100)[0]; // Apply wiggle only to X
y = value[1]; // Keep the original Y position
[x, y]
```

## Randomly flicker the opacity
This expression creates a repeatable random flicker that updates 10 times per second, outputting either 0 or 100 with an equal 50% probability on each update.
```
seed = index; // or any fixed number for repeatable pattern
seedRandom(seed + Math.floor(time * 10), true); // 10 = flickers per second
random(0, 1) > 0.5 ? 100 : 0;
```

## Triggering an opacity pulse from a marker on another layer
This expression looks for the most recent marker on the layer "ExampleLayerName" and, whenever a marker exists, animates the value from 100 down to 0 over 0.5 seconds after that marker, returning 0 if no marker has occurred yet.
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

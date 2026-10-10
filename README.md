# Lunar Lander in Desmos
A basic Lunar Lander game, created in [Desmos Graphing Calculator](https://www.desmos.com/calculator).

![Hero](https://www.dropbox.com/scl/fi/xylutqksd6btqdzh4brng/Screenshot-2026-10-10-161446.png?rlkey=a2uno7nakjxmzqfiza7wb03eh&st=yz67lhq1&raw=1)

[Play it here!](https://www.desmos.com/calculator/40tz1gtl9x)

## Features
* Real rocket physics
* Multi-tiered landing scores 
* Realistic gravity
* Full control via on-screen buttons
* Angular normalisation

The entire project is centred on the lunar lander object, rather than the graph origin. The lander position is measured using the variable pair (`P_{osX}`, `P_{osY}`), which the graph viewport centers on and zooms based on the graph constants `C_{ameraScaleX}` and `C_{ameraScaleY}`. The same follows for `Z_{oom}`, which has no effect on the graph itself but rather the positions of all non-anchored graphics. Buttons and labels are anchored to the screen using the `A_{nchored}` function, which passes the point and outputs the point with the offset of the lunar lander (That being the `P_{osX}` and `P_{osY}` pair as mentioned prior). The collision logic constantly checks if both legs of the lunar lander are below the terrain, and if so, pauses the game to check the lander's relative velocity. `C_{ollisionType}` is analogous to an enum taking the value of 0, 1, 2, or 3. Where 0 is no collision, 1 is crash landing, 2 is hard landing, and 3 is soft landing. Points are awarded depending on the type of crash.

One of the main hurdles of creating games like this one in Desmos are race conditions, in the form of actions. Every action's order needs to be absolutely precise, otherwise one variable will update and mutate it's value before it can be used by another function. The other massive hurdle is that there is no code whatsoever. If you can't decompose a programming concept into pure logic, then you can't create a desmos game. For example; the analogous enum variable `C_{ollisionType}` is just a value, such as `x` in the equation `2x = 10`. But `C_{ollisionType}` undergoes detailed processes to maintain a discrete and computationally predictable value, in this case the action `C_{ollideCheck}`, which is shown below.

```
C_{ollideCheck}=V_{elocity}\to\operatorname{mean}\left(V_{elList}\right),
C_{ollision}\to P_{ositive}\left(\left(2-\left(P_{ositive}\left(L_{eftAltitude}\right)+P_{ositive}\left(R_{ightAltitude}\right)\right)\right)\right),
S_{core}\to S_{core}+R_{unning}\left(5C_{heckHard}\left(C_{ollisionType}\right)+10C_{heckSoft}\left(C_{ollisionType}\right)\right),
C_{ollisionType}\to N_{ot}\left(D_{ead}\right)\left(C_{ollision}\cdot G_{etLandingMode}\left(V_{elocity},\Delta\right)\right),
D_{ead}\to O_{r}\left(C_{heckCrash}\left(C_{ollisionType}\right),D_{ead}\right),
R_{unning}\to N_{ot}\left(O_{r}\left(D_{ead},C_{ollisionType}\right)\right)
```

For those who can't read Desmos LaTeX, A screenshot of the action rendered is shown for convenience

![Action](https://www.dropbox.com/scl/fi/oxhgc3kl1n9wpmzzes4p2/Screenshot-2026-10-10-163832.png?rlkey=yizqd4icyimfsq4cy3ksjzkds&st=3ekdqo6v&raw=1)

Another example of computational logic in mathematical form are some of my functions. The function `A_{nd}` takes two inputs, which it assumes are *boolean values* (That is, the variables passed can only be 1 or 0), and outputs the correct value based on a piecewise function. If both inputs are 1, then a+b will equal 2. If a+b = 2, then both inputs are 1, so therefore the output must also be 1, this is analogous to an AND gate.

```
A_{nd}\left(a,b\right)=\left\{a+b=2:1,0\right\}
```

![And](https://www.dropbox.com/scl/fi/bo1ps0ew00n6p1r8gr0i0/Screenshot-2026-10-10-164022.png?rlkey=ely1uvknlhpcnfd2z4qpvj3i9&st=52896d25&raw=1)

## Full Acknowledgements (Everything I didn't know already from heart)
* FPS tracking: https://www.desmos.com/calculator/ptvzqgk5sb
* Use of [DesModder](https://chromewebstore.google.com/detail/desmodder-for-desmos/eclmfdfimjhkmjglgdldedokjaemjfjp) to help with orginisation.
* Rotation function (`R_otate`) taken from Google AI Summary (Search Query "how to rotate points on a graph")
* Full terrain zoom handling (`T_errainFunction`) taken from [a ChatGPT output](https://chatgpt.com/s/t_6aca58bf83f88191affe62164fdad915) (See the prompt [here](https://chatgpt.com/s/p_6aca591a9b288191a7bde671d4f5a69c))

Made By Jonah Crawford (@The_Craw) for the "Wrong Tool" event during Stardance, hosted by Hack Club

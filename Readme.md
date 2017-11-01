<img src="liquidfun/Box2D/Documentation/Programmers-Guide/html/liquidfun-logo-square-small.png"
alt="LiquidFun logo" style="float:right;" />

LiquidFun Version [1.1.0][]

# IN PROGRESS - fixed point

Box2D uses floating-point arithmetic, which is usually fast but usually not
reproducible (different machines or possibly even different runs will not give
identical output for identical input). This is because FPUs use very high
precision internally, and the exact details are very dependent on the hardware
and current configuration.

I'm experimenting with a fixed-point build of Box2D / LiquidFun.

### How to enable fixed point

Add this to your project settings, prefix header or equivalent:
```
#define B2_CUSTOM_FLOAT32 1
typedef my_custom_float float32;
```
The macro prevents `b2Settings.h` from defining `float32`.

If you add my [more-fixed-cpp][] repo to your project, `more::fixed16` can act
as a drop-in replacement for `float32`. This is a 16.16 bit fixed-point type.

### Avoiding overflow

[more-fixed-cpp][] currently checks for overflow via `assert`, so it will
abort if overflow occurs. I've found that's the most useful way to catch
precision problems early and often!

As a rule of thumb, it seems to be safe if you keep physics values within the
square root of the range. For `fixed16` that's about ±180. Box2D interprets
sizes as metres, so this is enough for a decent-sized play area.

I've hacked the Box2D code in a few places to prevent intermediate values from
exceeding this range. That generally means scaling inputs down to roughly 1
(e.g. by dividing by the max input) before multiplying them. This is still
work in progress.

### More caveats

- I haven't verified that Box2D physics is reproducible (but that's the goal).
- I haven't tried LiquidFun's fluid system at all yet, just Box2D solid physics.

# Welcome to LiquidFun!

LiquidFun is a 2D physics engine for games.  Go to our
[landing page][] to browse our documentation and see some examples.

LiquidFun is an extension of [Box2D][]. It adds a particle based fluid and soft
body simulation to the rigid body functionality of [Box2D][]. LiquidFun can be
built for many different systems, including Android, iOS, Windows, OS X, Linux,
and JavaScript. Please see `Box2D/Documentation/Building/` for details.

Discuss LiquidFun with other developers and users on the
[LiquidFun Google Group][]. File issues on the [LiquidFun Issues Tracker][]
or post your questions to [stackoverflow.com][] with a mention of
**liquidfun**.

Please see [Box2D/Documentation/Building/][] to learn how to build LiquidFun and
run the testbed.

LiquidFun has a logo that you can use, in your splash screens or documentation,
for example. Please see the [Programmer's Guide][] for the graphics and further
details.

For applications on Google Play that integrate this tool, usage is tracked.
This tracking is done automatically using the embedded version string
(b2_liquidFunVersionString), and helps us continue to optimize it. Aside from
consuming a few extra bytes in your application binary, it shouldn't affect
your application at all. We use this information to let us know if LiquidFun
is useful and if we should continue to invest in it. Since this is open
source, you are free to remove the version string but we would appreciate if
you would leave it in.

  [LiquidFun Google Group]: https://groups.google.com/forum/#!forum/liquidfun
  [LiquidFun Issues Tracker]: http://github.com/google/liquidfun/issues
  [stackoverflow.com]: http://www.stackoverflow.com
  [landing page]: http://google.github.io/liquidfun
  [1.1.0]: http://google.github.io/liquidfun/ReleaseNotes.html
  [Box2D]: http://box2d.org
  [Box2D/Documentation/Building/]: http://google.github.io/liquidfun/Building.html
  [Programmer's Guide]: http://google.github.io/liquidfun/Programmers-Guide.html

  [more-fixed-cpp]: https://github.com/more-please/more-fixed-cpp

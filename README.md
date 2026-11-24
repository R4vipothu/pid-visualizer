<img src="assets/drone-icon.svg" width="96" alt="PID visualizer icon">

# PID Controller Visualizer

An interactive, single-file web app that shows how a PID controller actually works. A drone tries to hold a target altitude against gravity, and you control the three gains. Every behavior from the textbooks (oscillation, steady-state error, windup, damping) shows up live, with the math worked out in real numbers next to it.

**Live demo:** open `index.html` in any browser. No build step, no dependencies, no server.

## Screenshots

Full PID with the math panel open: the live worked example substitutes the real error, P, I, and D values into the equation every frame.

![Full PID with the math panel open](assets/screenshot-full-pid.png)

The "Too much I" preset: integral windup makes the drone swing in a slow, wide wobble around the target instead of settling on it.

![Too much I preset: integral windup oscillation](assets/screenshot-too-much-i.png)

## What you can do

- **Drag the target altitude** (or use the slider) and watch the controller chase it
- **Tune Kp, Ki, Kd with sliders** while the sim runs and see the response change instantly
- **Presets** that demonstrate classic cases:
  - *P only, too low* — settles below the target (steady-state error)
  - *P too high* — oscillates
  - *P + D* — smooth arrival, small leftover offset
  - *Full PID* — lands on target and stays there
  - *Too much I* — slow windup wobble
- **Gust button** — kicks the drone with a disturbance so you can watch it recover
- **Gravity toggle** — turn gravity off and P-only suddenly reaches the target, which is the clearest way to see *why* steady-state error happens

## The math panel

A "Math behind it" section runs alongside the sim using its own live values:

- The PID equation, `u(t) = Kp·e(t) + Ki·∫e(t)dt + Kd·de(t)/dt`, with a plain-English line for each term
- A **live worked example**: the actual error, P, I, and D numbers substituted into the formula every frame, including when the thrust output gets clamped
- The discrete code the controller really runs (`integral = integral + error * dt`, etc.)
- An **error-over-time graph** with the area under the curve shaded, because that shaded area *is* the integral, plus a tangent line showing the live derivative (the slope D reacts to)
- A short explainer on why P alone settles short: hovering needs thrust equal to gravity, and P only produces that much thrust at one specific error, so the drone parks exactly that far below the target (`gravity / Kp`)

## What PID is, in one paragraph

A PID controller holds something at a target by continuously measuring the error (target minus actual) and reacting to it three ways at once. **P** reacts to the error right now, **I** reacts to the error accumulated over time, and **D** reacts to how fast the error is changing. Add the three together and you get the control output, recomputed every step. The same loop runs inside drone flight controllers, spacecraft guidance, motor drivers, and thermostats that are smarter than on/off switches.

## Tech

- One self-contained `index.html`: vanilla JavaScript and canvas, no libraries, no network calls
- Fixed-timestep physics (120 Hz substeps), thrust clamped, integral clamped for anti-windup
- Works on desktop and phone

## Why I built it

I kept hearing "watch a PID demo" as the first step into control systems, so I built the demo I wanted: one where you can put your hands on the gains and see the equation, the code, and the behavior all at the same time. Built with T3 Code.

## License

MIT

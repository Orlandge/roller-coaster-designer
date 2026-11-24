# Roller Coaster Designer

## Purpose
An educational web app for engineering students and introductory physics users to shape a two-dimensional roller coaster track, ride a simulated car, and see whether the resulting design is physically viable. The app is intended for concept exploration, not final ride design or safety certification.

## Inputs
- Starting-crest height, 20-300 ft, default 150 ft, number entry.
- Control-point x-position, 0-1000 ft, default sequence, drag or number entry.
- Control-point y-height, 0-300 ft, drag or number entry.
- Rolling-friction coefficient, 0-0.030, default 0.010, range slider.
- Friction toggle, on/off, default on for the own feature.
- Replay speed, 0.25-2.0x, default 1.0x, range slider.
- Optional bank angle at each point, -45 to 45 degrees, default 0 degrees, number entry or drag control.
- Start, pause, reset, and add/remove control-point controls.

## Calculations
The app uses a frictionless baseline and then applies the selected rolling-friction model.

Let:
- \(h_0\) = starting-crest height, ft
- \(h(s)\) = track height at distance \(s\), ft
- \(g = 32.174 ft/s^2\)
- \(v(s)\) = car speed, ft/s
- \(\mu_r\) = rolling-friction coefficient

For no friction:
\[
v(s)=\sqrt{2g(h_0-h(s))}
\]
The app converts speed to mph using \(v_{mph}=v_{fps}\times 60/88\).

When rolling friction is enabled:
\[
v_f(s)=\sqrt{\max(0,v_0^2-2g \Delta h-2g\mu_r s)}
\]
where \(\Delta h=h(s)-h_0\) and \(s\) is path length traveled. This model is a starting approximation that neglects air drag and contact losses.

The vertical g-force at a smooth point is:
\[
n=1+\frac{v^2}{gR}
\]
for a valley and
\[
n=1-\frac{v^2}{gR}
\]
for a crest, where \(R\) is radius of curvature in ft. Curvature is estimated from three neighboring track samples. The app marks failure when \(n>4\) or \(n<-1\).

The animation advances in 1/120-second fixed steps. The car cannot pass a crest it cannot reach; on energy loss it stops and reverses locally as a roll-back failure.

## Outputs
- Interactive 2D track view with draggable control points.
- Animated car and live speed, height, and vertical g-force readouts.
- Speed and g-force plotted against horizontal position or traveled distance.
- Marked failure locations for insufficient energy, over-4-g loading, or below--1-g loading.
- Status summary stating safe, roll-back, or unsafe g-load.
- Distance and energy summaries.

## Look and Feel
The app opens with a simple preloaded smooth track and the car at rest on the first crest. The first screen includes the track canvas, live readouts, control panel, and plots. Build controls remain compact and visible. On phones, panels stack below the track and canvas gestures remain usable.

The app should use clear labels, units on every numeric readout, and visible color coding: green for normal g-force, amber for warning, and red for failure.

## Validation
The hand-check case uses a starting crest at 150 ft and a bottom 0 ft above the reference line.

Using \(g=32.174 ft/s^2\):
\[
v=\sqrt{2(32.174)(150)}=\sqrt{9652.2}=98.246 ft/s
\]
\[
v_{mph}=98.246(60/88)=66.986 mph
\]

For a valley with radius \(R=100 ft\), the app should be compared at speed 98.246 ft/s:
\[
n=1+\frac{98.246^2}{32.174(100)}
=1+\frac{9652.2}{3217.4}
\approx4.00
\]

The expected validation results are approximately 98.2 ft/s, 67.0 mph, and 4.00 g in the valley. The app is acceptable when the displayed values round consistently to these values.
The app's built-in "Check 150 ft / 100 ft case" control reports 98.2 ft/s, 67.0 mph, and 4.00 g, matching the hand calculation.

## Features
Must-have features are the editable smooth track, conservation-of-energy motion, animated live readouts and plots, and roll-back/g-load failure marking.

Planned own features:
1. Rolling friction and replay speed controls, with live effect on the ride (the same optional ride-physics enhancement as the prompt's examples).
2. A "ride safety score" that combines maximum g-force, minimum g-force, and distance traveled, and highlights the most restrictive section.
3. A bank-angle editor and laterally simplified "banking score" to show whether each curve has a rider-friendly banking suggestion (own feature).

At least feature 3 is an original app design feature beyond the assignment's suggested examples.

## Out of Scope
- Real structural analysis or rail and support design.
- Detailed passenger restraint, aerodynamic, drivetrain, or thermal analysis.
- Terrain collision detection, loops with full 3D dynamics, and multi-car trains.
- Manufacturing, code compliance, and certification.
- Multiplayer or server-side accounts.

## Build Stages
1. Mockup the page layout and static track view.
2. Add smooth draggable control points and track editing.
3. Add energy-based motion, animated car, and live readouts.
4. Add the plot and failure detection for roll-back and g-force limits.
5. Add rolling friction, replay speed, and safety score.
6. Add the bank-angle editor and validate responsiveness and full workflows.

# Grid Billiards Explorer — Design Notes

**Live applet:** https://ohiomathteacher.github.io/billiards-explorer/
**Course context:** TCE 265 — Mathematics, History, and Technology (Miami University)
**Unit:** Maryam Mirzakhani / Billiard Trajectories (Week 7, Spring 2026)
**Developed:** March 7–8, 2026, with Claude Sonnet 4.6 (Anthropic)

---

## Educational Context

This applet supports a Thinker-Doer activity (TD_B) in a unit on Maryam Mirzakhani, the first woman to win the Fields Medal (2014). Mirzakhani's early work involved billiard trajectories on flat surfaces — studying the paths of a ball bouncing inside a polygon and asking: does the ball eventually return to where it started, or does it trace a path that never repeats?

The activity connects to **Paper Pool**, a common middle school investigation in which a ball always travels at 45° and students explore how the table dimensions determine which corner pocket the ball reaches and how many bounces occur. This applet extends Paper Pool by allowing students to vary the launch direction.

---

## Key Design Decision: Rise/Run Instead of Angle

The most consequential design choice was **replacing an angle slider (in degrees) with integer rise/run sliders**.

**The problem with degree angles:**
A ball in a rectangular billiard reaches a corner pocket if and only if the slope of its path (tan θ) is a *rational number*. For angles measured in whole degrees, this condition is almost never satisfied — only 45° (tan = 1) reliably terminates. At 30° or 60° (tan = √3/3 and √3, both irrational), the ball bounces forever. An angle-based interface gives students the impression that "only 45° works," which is both confusing and pedagogically counterproductive.

**The solution:**
Integer rise/run values always produce rational slopes (rise/run = p/q). This means every combination a student can select will result in the ball reaching a pocket — guaranteeing satisfying, observable results every time, which keeps exploration moving in a class period.

**The bonus:**
- Rise=1, Run=1 is exactly Paper Pool (slope 1, 45°), providing a concrete bridge from prior knowledge
- Rise=2, Run=4 produces the same path as Rise=1, Run=2 (both simplify to slope 1/2) — a natural discovery moment connecting slope equivalence to physical outcomes
- The applet displays both the simplified fraction and the equivalent angle (e.g., "slope = 1/2 · angle ≈ 27°"), maintaining the connection to the angle-of-incidence = angle-of-reflection physics from class

---

## User Experience Decisions

**Animated ball, not instant path display**
Early versions showed the complete path instantly. This felt like a "screensaver" — there was no moment of anticipation or discovery. The redesign animates a moving ball from the BL corner, with bounce sounds at each wall hit and a distinct "plunk" when it pockets. Students can see the counter increment in real time, connecting the number to the physical events they observe.

**"Shoot!" button replacing passive parameter display**
The user must actively fire the ball. This creates a deliberate action/observation cycle: set rise and run → anticipate → shoot → observe. The button turns red ("Cancel") during animation so students can stop if a path is taking too long.

**Max Hits slider**
Rather than claiming a path "bounces forever" (a strong claim students can't verify), the applet lets students control how many bounces to show. The endpoint marker distinguishes clearly: green "IN" for a pocketed ball, orange "?" with "still going?" for a path that hit its limit. This invites students to increase the limit and check — an empirical disposition the instructor values.

**Slope landmark on the grid**
When aiming, the applet highlights the lattice point (run, rise) on the grid with dotted guide lines and a dot. This makes the slope geometrically visible: students can count squares to see what "rise 2, run 3" means before shooting.

**Perimeter bounce numbers**
Wall hit points are labeled sequentially (1, 2, 3…) as the ball reaches them during animation, and remain visible after the shot. When ≤20 bounces, this gives students a counting scaffold to verify their observations. For longer paths the numbers are suppressed to avoid clutter.

**Removed "Explore" panel**
An early version included suggested questions and hints (including a reference to tan θ and rational numbers) in a sidebar panel. This was removed because: (a) the mathematical vocabulary (tan, irrational) is inappropriate for the 7th-grade audience; (b) the instructor controls the pedagogical scaffolding through the TD handout, not the tool; (c) a cleaner tool is more broadly useful across age ranges. All investigation questions belong in the TD_B handout.

---

## Mobile Responsiveness

The applet is used in a classroom where students bring phones. Key choices:

- Canvas resizes dynamically based on viewport width **and height** — in landscape phone mode, the canvas is height-constrained so both the table and controls are simultaneously visible without scrolling
- On portrait phones (small screen), the layout stacks: canvas above controls
- The "Shoot!" button was moved from the bottom of the sidebar to immediately beside the rise/run sliders, so students never need to scroll between setting an angle and firing — a significant usability improvement on small screens

---

## What the Applet Deliberately Does Not Do

- **No angle-based input** — avoids the rational/irrational confusion described above
- **No automatic "bounces forever" detection** — empirically honest; students discover this through the Max Hits mechanism
- **No suggested questions in the UI** — pedagogical guidance lives in the TD handout, not the tool
- **No GeoGebra dependency** — fully self-contained HTML/JS, deployable as a single file

---

## Technical Notes

- Pure HTML/CSS/JavaScript, no dependencies
- Canvas-based drawing using the HTML5 Canvas API
- Web Audio API for bounce and pocket sounds (synthesized, no audio files)
- Hosted on GitHub Pages: https://ohiomathteacher.github.io/billiards-explorer/
- Responsive via CSS media queries + JavaScript `resizeCanvas()` function
- The billiard physics uses exact rational arithmetic for the direction vector (dx = run/mag, dy = rise/mag) to ensure corner hits are detected reliably with floating-point arithmetic

---

## Development Process

This applet was developed iteratively through a single extended conversation between the instructor (Todd Edwards, Miami University) and Claude Sonnet 4.6 on March 7–8, 2026. The git history of the repository records the full sequence of design decisions, from the initial "hacker dark" aesthetic through a complete rethinking of the angle vs. slope interface to the final compact mobile-friendly layout. The conversation is an example of AI-assisted instructional design in which the human educator drives all pedagogical decisions while the AI handles implementation and surfaces mathematical constraints (e.g., the rational slope requirement) that inform those decisions.

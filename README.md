# Grok 4.7 vs Claude Opus 5.5: one prompt, a procedural 3D cat-girl run cycle

![Side-by-side preview](media/preview.gif)

**Run them yourself:** [side by side](https://negevtvx.github.io/grok-vs-opus-catgirl-run/) · [Grok 4.7 alone](https://negevtvx.github.io/grok-vs-opus-catgirl-run/outputs/grok-4.7-xhigh.html) · [Claude Opus 5.5 alone](https://negevtvx.github.io/grok-vs-opus-catgirl-run/outputs/claude-opus-5.5-ultra.html)

Both models got the same prompt once. Neither output was edited, retried or cherry-picked. The two HTML files in [`outputs/`](outputs) are exactly what the models returned, byte for byte (hashes below).

| | Model | Setting | Output |
|---|---|---|---|
| Left | Grok 4.7 | xHigh | [`outputs/grok-4.7-xhigh.html`](outputs/grok-4.7-xhigh.html) |
| Right | Claude Opus 5.5 | ultra | [`outputs/claude-opus-5.5-ultra.html`](outputs/claude-opus-5.5-ultra.html) |

## The prompt

The full prompt is in [`prompt.txt`](prompt.txt). In short, it asks for:

- **One self-contained HTML file** using Three.js r160 from a fixed import map. No other libraries, models, images, textures or fonts. Everything is built procedurally.
- **A real joint hierarchy**: pelvis → spine → chest → neck → head, both arms down to the hands, both legs down to the feet. Stylized anime proportions, about 4 heads tall.
- **Specific design details**: shoulder-length hair with blunt bangs (10+ separate clumps), cat ears, a tail with 10+ chained segments, big anime eyes, blush, an oversized hoodie, track pants, sneakers with red soles, toon shading with inverted-hull outlines.
- **A seamless run loop driven by a single phase value**, using only integer-harmonic sin/cos. It needs contact, down, passing, up and flight phases, a body bob, hip and torso counter-rotation, arm swing, a stabilized head, ear bounce, tail follow-through, and hair lag.
- **A ground that scrolls at the planted foot's speed**, so the feet don't slide.
- **Controls**: Space pause/play, S 0.25× slow motion, 0–9 jump to a phase, R reset camera, plus OrbitControls.

The prompt contains a template placeholder, `{{HAIR_COLOR}}`, that was never filled in. Both models noticed it and picked a color themselves. Grok chose auburn (`#a15c38`) and Opus chose lavender (`#8e7cc3`). Both explain the choice in the comment at the top of their file.

## Try it yourself

- **Live:** use the links at the top. The side-by-side page mirrors key presses to both sides. Click either one, then press <kbd>Space</kbd>, <kbd>S</kbd>, <kbd>0</kbd>–<kbd>9</kbd> or <kbd>R</kbd>.
- **Locally:** download either file from [`outputs/`](outputs) and open it in Chrome. Three.js loads from jsDelivr, so you need an internet connection.

| Key | Action |
|---|---|
| <kbd>Space</kbd> | pause / play |
| <kbd>S</kbd> | toggle 0.25× slow motion |
| <kbd>0</kbd>–<kbd>9</kbd> | pause and jump to phase p = 0.0 … 0.9 |
| <kbd>R</kbd> | reset the camera |
| drag / scroll | orbit / zoom |

## Side-by-side video

[`media/side-by-side.mp4`](media/side-by-side.mp4) is 24 seconds long at 720p60, no audio. Grok is on the left and Opus is on the right. Both get exactly the same inputs: slow motion, pause, an orbit to the front, a zoom on the face, an orbit around the back, a camera reset, then the phase keys 0, 2, 4, 6 and 8.

<details>
<summary>How it was recorded</summary>

- Both pages ran in headless Chromium at 960×960 with Playwright's fake clock. Time only moved forward in exact 1/60 s steps, and a frame was captured after each step. That keeps the two halves in sync to within a frame or two. The only slack is exactly when an input event lands relative to a page's animation frame.
- Every key press, mouse drag and scroll event came from one script and hit both pages on the same frame. The only differences you see come from the code itself. For example, Grok's camera has OrbitControls damping turned on and Opus's doesn't, so after a drag Grok's camera eases to a stop and Opus's stops immediately.
- WebGL was software-rendered (SwiftShader) because the capture machine had no GPU. On a real GPU both look a bit crisper.
- The capture machine couldn't reach jsDelivr, so Three.js was served locally from the official `r160` tag of the three.js repository. That's the same release the import map points to.
- The recording script, this README and the side-by-side page were put together with Claude's help. The two model outputs themselves are untouched.

</details>

## All ten phase keys, same camera

![Phases 0.0 to 0.9 for both outputs](media/phases.png)

## Facts

| | Grok 4.7 (xHigh) | Claude Opus 5.5 (ultra) |
|---|---|---|
| File size | 25,075 bytes, 763 lines | 42,210 bytes, 770 lines |
| Console errors or warnings (headless Chromium) | 0 | 0 |
| Hair color picked for `{{HAIR_COLOR}}` | `#a15c38` auburn | `#8e7cc3` lavender |

SHA-256:

```
e00ddb6ddeaff8ef4da5e109d7061617eba09fc1b11217b9acc5ef4f441d9556  outputs/grok-4.7-xhigh.html
9339d917fd8bc502dd1b50114ae9115ae8fdc92acaf811e0a82384ffb16bb7bf  outputs/claude-opus-5.5-ultra.html
c20bb0d0aaf1a554a41f27b8f818b6ef139e5cb104e1829459bd20bb0ec14ae3  prompt.txt
```

Judge for yourself.

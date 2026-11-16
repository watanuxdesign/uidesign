# Air Keys — play piano with your hands

A single-file web app: your webcam tracks your hands and you play an on-screen piano or synth by pinching.

## Run it

The camera only works over `https://` or `http://localhost`, so you can't just double-click the file. Pick one option:

- **Python:** in this folder run `python3 -m http.server 8000`, then open http://localhost:8000
- **Node:** `npx serve .`, then open the URL it prints
- **GitHub Pages (free, shareable):** push `index.html` to a repo → Settings → Pages → deploy from the main branch.

Use Chrome or Edge for the best tracking performance. You need an internet connection the first time (the libraries, hand model and piano samples load from CDNs).

## How to play

1. Press **Camera** and allow access.
2. Hold a hand up, palm toward the screen. **Move left/right** to pick a key; **raise your hand slightly** for black keys.
3. **Pinch** (thumb + index fingertips touching) to play. Hold the pinch to sustain, open to release. Two hands = two notes.
4. Press **Calibrate area** in the left panel and pinch at the left end, then the right end of a comfortable reach. The keyboard then fits your arm span.

The **Gesture monitor** shows, for each hand: which key it is over, whether it is PLAYING, and a live pinch meter. The white tick is where a note triggers, the grey tick is where it releases. If pinches are missed, raise **Gesture sensitivity** in Settings. If notes fire by accident, lower it.

Mouse, touch and the computer keys A W S E D F T G Y H U J K also play notes (Z/X change octave).

## How the tracking stays accurate

- **Stable cursor:** the cursor follows the centre of your knuckles, not your fingertips, so it doesn't drift while you pinch.
- **Distance-independent pinch:** pinch strength uses MediaPipe's 3D world landmarks in metres, divided by your hand size, so it behaves the same near or far from the camera.
- **Hysteresis:** a note plays below one threshold and releases only above a higher one, so it never flickers. A 90 ms debounce and a 1€ smoothing filter add to that.
- **Key stickiness:** hovering doesn't flip between neighbouring keys at their edges. Black keys need a deliberate raise of the hand, with a small dead zone between the black-key and white-key heights.
- **Hand matching:** hands are matched frame to frame by position, so a left/right label swap never re-triggers notes. If a hand leaves the frame for 240 ms, its notes are released, so nothing gets stuck.

## Open-source credits

- [MediaPipe Tasks Vision](https://github.com/google-ai-edge/mediapipe), Apache-2.0: hand landmark model
- [Tone.js](https://github.com/Tonejs/Tone.js), MIT: audio engine and synths
- [Salamander Grand Piano](https://archive.org/details/SalamanderGrandPianoV3) samples by Alexander Holm, CC-BY 3.0, hosted by Tone.js
- [Three.js](https://github.com/mrdoob/three.js), MIT: optional 3D piano background (Scene menu)
- [canvas-confetti](https://github.com/catdad/canvas-confetti), ISC
- The 1€ filter follows Casiez, Roussel & Vogel (CHI 2012)

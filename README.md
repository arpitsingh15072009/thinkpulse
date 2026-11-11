# ThinkPulse
 
A posture monitor that runs in your browser. It uses your webcam to estimate how you are sitting, reminds you to take breaks, and can send an alert to your phone.
 
Live demo: https://arpitsingh15072009.github.io/thinkpulse/
 
ThinkPulse is a prototype. Scores are estimates, not medical advice.
 
## What it does
 
- **Posture score.** Reads your shoulders, head and torso with the MoveNet pose model and gives a live score. You can calibrate it to your own upright posture.
- **Sitting reminders.** Set a limit in minutes and get a stretch-break prompt when you have sat too long.
- **Hand gestures.** Control the page with hand signs using MediaPipe Hands. There is a guide for all 50 gestures at `#gestures`.
- **Phone alerts (optional).** Pair your phone with a QR code and it rings when your posture slips.
- **Spine timeline.** One average score per day, shown as a history chart.
## How it works
 
1. **Camera.** When you press Enable camera, your browser asks for permission and streams the webcam into a video element on the page. Nothing is recorded.
2. **Pose detection.** The MoveNet model runs in your browser through TensorFlow.js (WebGL, with a CPU fallback). Each frame it returns body keypoints such as the eyes, ears and shoulders, with a confidence value for each.
3. **Posture score.** The page compares those keypoints, for example how level and how far forward your shoulders and head are, against your calibrated upright posture. Low-confidence keypoints are ignored, and the page tells you when your shoulders are not clearly in frame.
4. **Feedback.** The score updates live, and the sitting timer prompts a stretch break when you pass your limit. A daily average feeds the Spine Timeline.
5. **Hand gestures.** A second model, MediaPipe Hands, reads your hand shape and maps it to page actions. It pauses itself if the posture model stalls, so posture tracking always comes first.
6. **Phone alerts.** Generate pairing code makes a random session ID and shows it as a QR code. Your phone opens the link and connects to the same session over MQTT through a public relay. When your posture stays bad, the page publishes a short alert and the phone rings.
Everything above runs client side. There is no server of mine in the loop.
 
## Privacy in short
 
- No backend, no account, no analytics. Your video never leaves your device.
- Video is processed frame by frame in your browser and is not recorded or saved.
- Without your consent, nothing is saved between visits. If you press Accept on the cookie notice, your theme and daily average scores are kept in your browser's local storage. Decline deletes them.
- Third-party files (fonts, ML libraries, the QR library) are fetched only when a feature needs them, not when the page opens.
- Phone alerts go through the public HiveMQ test relay (`broker.hivemq.com`). It is public, not private. Only a short alert message is sent, under a random session ID. No video or scores.
The full text is in the Privacy, Cookies and Terms sections of the site.
 
## Run it
 
It is a single static file.
 
1. Download `index.html`.
2. Serve it over `localhost` or https, because browsers only allow the camera on a secure origin:
```
   python3 -m http.server 8000
```
 
3. Open http://localhost:8000 and press Enable camera.
No build step and no dependencies to install.
 
## Built with
 
| Library | Licence |
|---|---|
| TensorFlow.js, pose-detection, MoveNet (Google) | Apache-2.0 |
| MediaPipe Hands (Google) | Apache-2.0 |
| MQTT.js | MIT |
| QRCode.js | MIT |
| IBM Plex Sans and Mono (IBM) | SIL OFL 1.1 |
 
Loaded from jsDelivr and cdnjs with pinned versions. IBM Plex loads from Google Fonts only after you accept the cookie notice.
 
## Browser support
 
Current Chrome, Edge, Firefox and Safari with a webcam. The pose model runs on WebGL and falls back to the CPU if needed, which is slower.
 
## Known limits
 
- Scores depend on lighting, camera angle and whether your shoulders are in frame.
- The phone relay is a public test service and can be slow or unavailable.
- Libraries are loaded from CDNs, so the demo needs an internet connection.
## Author
 
Made by Arpit Singh. Instagram: [@calmcircuits.official](https://www.instagram.com/calmcircuits.official/)
 
## Licence
 
Released under the MIT License. See the `LICENSE` file. The third-party libraries listed above keep their own licences.
 
## Keywords

ThinkPulse, posture tracker, webcam posture detection, posture monitoring, neck angle, shoulder angle, hand gestures

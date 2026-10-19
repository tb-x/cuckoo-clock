# Cuckoo Clock

A telling-the-time game for young kids (around age 5), made for phones and tablets. It moves between the hand clock, the digital clock and the way we say the time out loud ("half past three"). Every time is spoken, and when you get one right the cuckoo pops out of its little door.

- **Pick a clock.** There are four levels: o'clock, half past, quarter past and quarter to, and five-minute steps. Each button shows an example on both clocks.
- **Ten questions a round, in three kinds, mixed up:**
  - **What time is it?** The cuckoo clock shows the hands. Tap the digital clock that matches.
  - **Which clock says…?** A digital time is shown and said out loud. Tap the hand clock that matches.
  - **Show me…** A digital time is shown and said out loud. Turn the hands to it, then tap ✓. Drag the short hand to set the hour and the long hand to set the minutes; the long hand clicks round in the level's steps and takes the short hand with it, like a real clock. A tap on the face jumps the nearest hand there. In the o'clock level only the short hand moves.
- **Colours tie the two clocks together.** The short (hour) hand is red and the hour digits are red; the long (minute) hand is blue and the minute digits are blue. In the five-minute level the minutes (5, 10, 15 …) are written in blue around the face.
- **Wrong answers are gentle.** The first one says "Oops! Try again." After two, the right answer glows and the voice explains which hand shows what ("The short hand shows the hour."). On a "Show me" question the hands then turn to the right time by themselves.
- **Stars.** The row of dots at the top fills with a gold star for each first-try answer and a pale star for the others. The end card shows them all, then 🔁 plays again, ▶ goes to the next level and the grid goes back to the levels. Each level's best gold-star count is shown on its button.

The yellow speech-bubble button says the question again. The speaker in the top-right corner turns sound off and on, and 🏠 in the top-left corner goes back to the list of all games.

The questions and wrong answers are picked so they're worth thinking about: the wrong digital times are mostly the ones children really mix up, like the hour before or after (the short hand sits between two numbers at half past), quarter past against quarter to, and the two hands swapped.

## Run it

It's one page plus the sound clips in `assets/`. Serve the folder (for example `python -m http.server 5223`) and open it in a browser; it needs an internet connection the first time, for the font. No build step. Opened straight from disk it still works, but the browser won't load the clips, so you hear synthesized sounds and the device's own voice instead.

## Play offline (add to Home Screen)

On iPhone or iPad, open the game in Safari, tap **Share → Add to Home Screen**, then open it once from the new icon while online. After that it starts full screen and works without internet. Changes arrive on their own: the next online launch downloads them and the one after shows them.

(How: `manifest.webmanifest` gives the icon and full-screen mode; `sw.js`, a service worker, keeps a copy of every file the game uses, including the font and the sound clips. The icons `icon-180.png` and `icon-512.png` are rendered from `icon.svg` in headless Chrome.)

## Tuning knobs

At the top of the script in `index.html`:

- `ROUND`: questions in a round (10).
- `ROUND_TYPES`: how many of each kind of question a round has (4 "What time is it?", 3 "Which clock says…?", 3 "Show me…").
- `LEVELS`: the minutes each level asks about. A minute listed twice comes up twice as often (the half-past level asks half past three times as often as o'clock).
- `STEP`: how far the long hand clicks when dragged, per level (60, 30, 15 and 5 minutes).
- `TRIES_BEFORE_HELP`: wrong answers before the right one is shown (2).
- `NEXT_DELAY`: the quiet pause after the cuckoo before the next question.
- `ASSET_V`: bump it after replacing any file in `assets/`, so phones don't keep the old one.
- `CLIP_TRIM`: the loudness of the sound effects.
- `JOIN_GAP`: the pause between the minutes and the hour ("twenty-five past" … "three"), almost none so they sound like one phrase.
- `VOICE_GAP`: the pause after a question ("Show me…") before the time.
- `SPEECH_FLOOR`: how quiet counts as silence when the joined clips are trimmed. Kept low (3% of the clip's loudest) so the soft "th" of three and "f" of four and five aren't cut.

## How the time is spoken

O'clock, half past, quarter past and quarter to are recorded as whole phrases (48 clips, `t-<hour>-<minutes>.mp3`). The other five-minute times are joined from two clips, the minutes (`p-25-past.mp3`, "twenty-five past") and the hour (`h-3.mp3`, "three"), with the silence and breaths around each part cut off and the two played straight after each other, so they run together like one phrase. The questions are joined the same way: "Which clock says…" or "Show me…" followed by the time.

## Audio credits

Voices and sounds: [elevenlabs.io](https://elevenlabs.io). All clips were made on 2026-10-19 on the ElevenLabs paid **Starter** plan. The game credits ElevenLabs on its end card. The clips are not covered by any licence on this game's code and aren't licensed for reuse.

- **The voice** (`assets/t-*.mp3`, `p-*.mp3`, `h-*.mp3` and `say-*.mp3`, 79 clips): voice "Jessica - Playful, Bright, Warm" (a stock ElevenLabs voice), model Eleven Multilingual v2, speed 0.85, stability 0.6. The 48 whole times, 8 minute parts, 12 hours, the three questions, four kinds of praise, "Oops! Try again.", the two hand hints and "You did it! Cuckoo!".
- **Sound effects** (`assets/sfx-*.mp3`, 3 clips): ElevenLabs Sound Effects. The cuckoo call, the chime for a right answer and the tick-tock that starts a round.

The small UI sounds (the hand's click, the tap, the gentle wrong-answer "bwomp" and the end fanfare) are synthesized in the browser.

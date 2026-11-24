---
name: engraved-map-explainer
description: Create a 1920x1080 Vox-style historical map explainer on one engraved paper map. Use this skill when the user provides a voiceover and exact script, or asks to turn a historical war, voyage, trade route, or country-formation story into a 25-35 second map video.
---

# Engraved Map Explainer

Create a **1920×1080, Vox-style historical map explainer**. Keep the whole video on one engraved paper map and fly the camera across that map as the story moves.

## Required opening

Do not render, animate, or ask about implementation details until the preflight is complete.

1. Ask the user to upload or provide:
   - a voiceover as MP3 or WAV; and
   - the exact script, pasted as text.
2. If the user has no voiceover or script, pitch exactly three historically grounded map stories:
   - one war;
   - one voyage; and
   - one trade route or how a country was made.

   Give each story one surprising true fact and write a 25-35 second script for the user to record. Use one short sentence per beat. Do not invent facts. If the sources are uncertain, label the uncertainty and verify the place, date, number, and sequence before proceeding.
3. Ask whether the user has an SFX pack to upload.
4. Before any render, show:
   - the complete beat list, with timing and spoken words; and
   - the camera plan, with frame and `(lon, lat, zoom)` keyframes.

Wait for confirmation of the beat list and camera plan before rendering.

## Narrative structure

Use one sentence as one beat. A beat starts 6 frames before its first word. Let video length be dictated by the voiceover plus a 1.3-second hold after the last word.

Plan the sequence as follows:

- Frame 0: show the home region at approximately `zoom 0.6`. Paint it yellow on the word that names it and slap a ransom number onto it.
- Pull out to approximately `zoom 0.32` so both sides of the story fit. Draw a dashed arc between them, a distance tag, and pin a circled far-party cut-out to its capital.
- Across the map: animate causes as arrows, one arrow per clause, with a dated tag for each cause. Move a cut-out prop along the last arrow and make it land.
- Zoom to the event city at approximately `zoom 1.35`. Put the date on the title card. Drop pieces in one by one with ink splash marks. Present the big true number in ransom letters and a tag underneath.
- Use the reaction or joke beat at the same spot. Add a generic character cut-out, a marker note, and an ink circle.
- Move to the next city at approximately `zoom 1.15`. Add a dated tag and a document cut-out. On the verb, pull wide and fly the document along a dashed trail to the other side.
- Return to the consequence region at approximately `zoom 0.7`. Add one ink X per event in date order, each with a `PLACE YEAR` tag.
- Finish with a final wide pull-out. Put the payoff name in ransom letters over the region. Swap the losing-side cut-out for its sad version. Add a marker note. Place a beige sheet bottom-centre with a bold Caslon line and underline the ransom payoff word with yellow highlighter. Hold through the final 1.3 seconds.

All dates, numbers, places, people, and outcomes must be real. Do not add invented facts. Use generic representations of people only: a king, colonist, sailor, or other historically appropriate archetype. Do not draw a real person's likeness.

## Timing and voice

- Render at 30 fps.
- Voice starts at frame 4.
- Every cue must use an exact word and occurrence number, for example `word “route” occurrence 2 at frame 327`.
- One sentence is one beat.
- Try faster-whisper for word timestamps first.
- If its model download is blocked, use ffmpeg silencedetect at `-35 dB` and `0.15 s` to locate speech spans. Map each span to words from the pasted script and distribute the words inside the span by syllable count.
- Define a beat start 6 frames before the first word. Give every element an explicit start and end frame.
- Keep every camera move 16-26 frames long and use smoothstep easing. Step camera properties on twos.
- Make one camera move for every 1-3 sentences.
- Interpolate zoom in log space.
- Never position a camera so a frame sees past the map edge.

## Visual palette and typography

- Board: `#EFEBE3`
- Ink: `#1D1C20`
- One accent: `#EDBD2E`
- Beige sheets: `#F1EDE4`
- Never use any other colour.
- Never use a dark background.
- All text is on paper: a title card, yellow tags, notes, or ransom letters.
- No text is baked into the map. Coasts and graticules may contain only engraved place names and oceans.
- Use letter-spaced Libre Caslon for engraved place names; italicize oceans.
- Title card: top left, beige, 560 px wide, rotated `-1.2°`, with a small yellow `FIG. 0N` tag. Use Libre Caslon 68 px serif lines, typing at 1 letter per frame. Size the card from its line count.
- Notes: lower-case Permanent Marker, 58-84 px, written at 1.3 letters per frame.
- Tags: Special Elite, 22-26 px, letter-spaced, on small yellow paper strips.
- Ransom letters: slap one cut-paper letter down every 3-4 frames. Cycle Caslon Bold, Anton, and Special Elite on ink, beige, and yellow squares.

## Map

- Project hand-written longitude/latitude coast vertices with equirectangular projection and longitude scaled by `cos(mid-latitude)`.
- Produce a 7200 px-wide map.
- Smooth every coastline with 3 Chaikin passes.
- Build 4 mip levels using `INTER_AREA` and select `floor(log2(1 / zoom) + 0.15)` for each frame, so hatching does not shimmer.
- Extend the map at least 25° beyond every shot.
- When smoothing a closed land polygon, push every closing corner off the canvas first to avoid rounded fake coastlines.
- Use approximate coastlines and state that the coastlines are approximate in final credits or notes if the format requires credits.

### Engraving rules

- Sea: 4 coast-parallel lines 8/19/32/48 px from the shore, then wavy horizontal lines every 22 px.
- Land: diagonal hatch every 22 px.
- Featured region: cross-hatch every 13 px.
- Coastline weight: 6 px.
- Borders: 4 px dashed, 27 px on and 19 px off.
- Dotted graticule every 10°.
- Draw exactly one compass rose in open sea.
- Paint the featured region yellow and multiply it at 90% so the hatching remains visible.
- Reveal the featured region with a south-to-north sweep across 16 frames on the word that names it.

## Animation

Everything animates on twos.

- Boil every paper piece every 8 frames by applying approximately ±1.5 px of translation and ±0.4° of rotation.
- Use three jitter seeds, cycled every 8 frames.
- Slap paper pieces in with scale `1.1 → 0.98 → 1.0`, on twos. Never fade pieces in.
- Reveal every ink mark over 8-15 frames.
- Add ink X strokes of 20 px over 6 frames.
- Reveal highlighter in a 26-34 px yellow pass, multiplied with the underlying ink and paper.
- Project ink control points to screen every frame so their perceived weight remains 5-6 px at every zoom.
- Draw felt-marker circles every 1.6 px with ±15% width wobble and tapered ends.
- Overlay a dry crayon pass at 0.32 width with 35% dropouts using `max()` merge with the marker pass, never overwrite it.
- Draw arrowheads 26 px long at ±0.48 radians.
- Use dashed strokes of 27 px on and 19 px off.
- Move props along arrow splines with smoothstep and ±4° wobble.
- Do not leave a frame with no visible movement.

## Collage pieces

Draw collage pieces fully in code:

- grayscale cut-outs for characters, props, and documents;
- soft grayscale shading;
- a 5 px dot halftone screen at 45°, mixed at 34%, with contrast `1.25` and grain;
- a 7 px white scissor edge with noisy wobble;
- a shadow offset `(5, 7)` px, blurred 6 px, at 32%;
- documents drawn as engraved line art on cream paper, with no halftone.

Pin every piece to a longitude/latitude plus a pixel offset. Keep pieces at a constant screen size. Use pixel offsets instead of degrees at city zoom, because 0.2° can be only about 16 px at that scale.

Before rendering, check each pinned note at both of its wide and close camera positions. A note that fits in a wide shot can be off-screen in a close shot.

## Camera keyframes

Represent keyframes as `(frame, lon, lat, zoom)`. Ease with smoothstep. Step zoom and position on twos. Define `(frame, lon, lat, zoom)` values explicitly for:

- the opening home shot;
- both sides of the conflict;
- causes;
- the event city;
- the reaction beat;
- the next city;
- the consequence region; and
- the final payoff.

Avoid blank map edges and never leave a keyframe with an element off-screen or colliding with the next beat.

## Pre-render contact sheet

Before the full render:

1. Render a 10-12 frame contact sheet.
2. Inspect for:
   - overlaps;
   - empty stretches;
   - text outside frame;
   - blank map edges;
   - collisions between tags from adjacent beats;
   - and pieces missing at their key camera position.
3. Fix all detected problems before the full render.

## Rendering

Use Python, NumPy, OpenCV, Pillow, and FFmpeg. Obtain fonts from Google Fonts:

- Libre Caslon Text;
- Permanent Marker;
- Special Elite; and
- Anton.

Report every installed dependency, font, or system package.

- Draw 30 fps frames with NumPy/OpenCV.
- Split the frame range across CPU workers.
- Pipe each segment as raw RGB to FFmpeg `libx264 CRF 16`.
- Concat the segments.
- Overlay a paper texture across every frame: cloud noise, fibres, creases, grain, and a soft vignette.
- Swap dust specks every 8 frames.

## Sound

- Use the uploaded SFX pack when supplied.
- Otherwise synthesize:
  - a paper slap for each piece;
  - a marker scratch for each ink mark;
  - typewriter ticks for typed text;
  - a scribble for the yellow reveal; and
  - a soft whoosh for every fast camera move.
- Place SFX at approximately 0.32× the voice peak.
- Merge same-type hits within 3 frames.
- Peak the final mix at `-0.3 dB`.
- Mux as AAC 192k.

## Hard bans

Never use:

- any colour outside the four-colour palette;
- photos of real people;
- logos;
- burned-in subtitles;
- glassy UI cards;
- a frame where nothing moves;
- invented facts;
- or a dark background.

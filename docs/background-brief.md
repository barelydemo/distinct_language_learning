# Lexical — background brief (3 new scenes)

You are painting three background scenes for **Lexical**, an anime visual novel set in Japan in spring 1998. Characters are drawn as separate sprites and placed on top of these backgrounds. **Paint the empty place: no people in the foreground.** The bottom of each image will sit behind the dialogue box and the characters' legs.

## 1. Technical spec (all three)

| | |
|---|---|
| Format | JPG, high quality |
| Size | **1280 × 720** (16:9), or a larger 16:9 size such as 1920 × 1080 |
| Style reference | Attach `/Users/ishan/Lexical_Revived/lexical/web/public/scenes/station_concourse.jpg` and `narita_arrivals.jpg`. Match their painterly anime-background style, brushwork, colour grading and level of detail exactly, so the new scenes sit in the same game |
| Era | 1998 Japan: no smartphones, no touchscreen machines, no LED signs, no modern logos. CRT screens, paper signs, payphones and coin trays are fine |
| People | None in the foreground or middle ground. At most a few soft, blurred, faceless figures far in the background |
| Text | Painted signs may hold short Japanese words (そば, きっぷ, アメ横). No English, no watermarks, no captions, no borders |
| Composition | Eye-level camera, as if the player is standing there. Keep the **centre third clear of clutter** at standing height, where characters will stand. Keep the **bottom 25%** simple (floor or counter), since UI covers it |
| Light | Natural and believable for the time of day given. Soft depth of field on far details |

## 2. Scenes

### 2.1 `soba_stand`: standing-soba stand by the station gates (morning, about 10:20)
A tiny standing-only soba stand (立ち食いそば) in the basement of Narita Airport, a few steps from the train ticket gates:
- **The counter:** a short stainless-steel counter at waist height with no stools, running left to right across the lower-middle of the frame.
- **The kitchen:**
  - Behind the counter, two big steaming noodle pots, white steam rising.
  - Stacks of white and blue ceramic bowls.
  - A ladle rack.
  - A meal-ticket vending machine with chunky buttons at the left edge.
- **Signs:**
  - A faded indigo noren curtain with white そば lettering hanging over the counter.
  - Handwritten price cards on the wall behind: かけそば ¥280, きつね ¥350, てんぷら ¥400.
- **Atmosphere:**
  - A small radio and a battered wall clock reading about 10:20.
  - Warm yellow light inside the stand, contrasting with the cool white fluorescent corridor beyond.
  - On the right, through the corridor, the edge of the station ticket gates.
- **Mood:** cosy, steamy, busy-but-humble, family-run for 30 years.
- **For staging:** keep the space in front of the counter open, centre and right, where the owner and the player stand. The counter top can be at about 60–65% of the image height from the top.

### 2.2 `ticket_window`: staffed railway ticket window (morning, about 10:15)
Front view of a staffed 1990s Keisei railway ticket window at Narita Airport station, seen straight-on at eye level. **This scene is built around a character sprite** (the station attendant, drawn waist-up behind his own counter), so the layout matters:
- **The window:** a single ticket-window opening, **centred horizontally**, about **35% of the image width**, framed in brushed steel.
  - Inside it, a small staff booth: a back wall with a framed timetable board, a pinned fare table of station names and prices, a wall calendar, a rotary desk phone and a filing tray.
  - Keep the booth interior slightly darker and softer than the hall, so the character in front of it reads clearly.
- **The counter line:** the window's counter top sits at about **85% of the image height from the top**. The attendant's own sprite supplies the near counter edge, so paint only the counter top and the wall below it, plainly.
- **Around the window:** a large route map of the Keisei line with colourful station dots, painted above and to the sides. Also:
  - a "きっぷうりば" sign above the window
  - two older ticket-vending machines with coin slots at the far left
  - a small queue rope on chrome posts at the right
- **Light:** cool station fluorescent light, clean pale tiled walls and a polished floor.
- **Mood:** orderly, precise, a little old-fashioned.

### 2.3 `ameyoko`: Ameyoko market street in Ueno (late morning, sunny)
- **The lane:** Ameyoko (アメ横) market in Ueno, 1998. A narrow, lively shopping lane running away from the viewer **under the elevated JR railway tracks**: a heavy steel girder bridge and riveted pillars overhead on one side, with sunlight cutting down between the tracks.
- **Stalls on both sides:**
  - fish on crushed ice with handwritten price boards
  - dried squid and seaweed in bins
  - big bags of chocolate and candy stacked high
  - sneakers, watches and leather jackets hanging from awnings
- **Signs and details:**
  - Red and yellow shop signs and hand-painted 円 price boards.
  - Strings of bare bulbs, cardboard boxes and styrofoam crates on the ground.
  - A few soft, faceless shoppers far down the lane in spring jackets.
- **Mood:** loud, crowded, cheerful, working-class Tokyo energy.
- **For staging:** keep a clear patch of pavement in the near centre, where the player and a vendor talk. Put stall fronts on the left and right edges rather than the middle.

## 3. Delivery

Save the three images as:

```
soba_stand.jpg
ticket_window.jpg
ameyoko.jpg
```

into:

```
/Users/ishan/Lexical_Revived/lexical_revived/
```

They'll be checked against the game's sprites, especially the ticket window against the attendant's counter, then moved into `web/public/scenes/`.

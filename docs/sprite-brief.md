# Lexical — character sprite brief

You are generating the full character sprite set for **Lexical**, an anime visual novel set in Japan in spring 1998. The player is a foreigner who lost everything at Narita Airport and has to talk their way through Tokyo in Japanese. There are 7 characters. Each one needs a base sprite plus 11 poses: **84 PNGs in total**.

The game shows one sprite at a time per character and swaps between poses instantly. The talking animation alternates `base` and `talk` every 140 ms. So **every image of a character must line up with that character's base image**: same scale, same position, same outfit, same lighting. Consistency matters more than variety.

---

## 1. Technical spec (applies to every image)

| | |
|---|---|
| Format | PNG |
| Aspect ratio | 3:4 portrait (e.g. 1536 × 2048 or 1024 × 1365) |
| Background | **Flat solid pure green `#00FF00`**, edge to edge: no gradient, floor, shadow, vignette or scenery |
| On the character | **No pure or bright green anywhere** (clothes, props, eyes stay off `#00FF00`), so the background can be keyed out cleanly |
| Framing (standing characters) | Full body, top of head to feet. The figure fills about 92% of the image height, centred horizontally, feet on the same baseline in every image |
| Framing (Nakajima only) | Waist-up behind a ticket-window counter. The counter's front edge fills the bottom 20% of the frame (see his section) |
| View | Body turned three-quarters toward the viewer's left, face and eyes toward the viewer |
| Lighting | Soft key light from upper left, gentle rim light on the right edge, no cast shadow |
| Not allowed | Text, logos, watermarks, borders, signatures, speech bubbles, extra people, background objects |

## 2. Art direction (same for all 7)

Modern Japanese anime visual-novel sprite art:
- **Line and shading:** clean confident lineart of even weight, two-tone cel shading with a soft gradient on hair, subtle blush on cheeks.
- **Eyes:** expressive, with highlights.
- **Clothing:** realistic 1998 Japanese fashion and uniforms, no modern tech. Grounded proportions, about 7 heads tall for adults (Fumiko about 6.5).
- **Overall feel:** warm, gentle and slightly nostalgic: city-pop era colours, nothing neon.

All 7 must look like they come from the same game and the same artist.

## 3. How to keep each character consistent

1. Generate the character's **`base`** image first, from the character sheet below. Regenerate until it's right; it's the reference for everything else.
2. Make every other pose **as an edit of the base image**: image-to-image, with the base attached as the reference. Change only what the pose describes. Keep these identical to the base:
   - face and hairstyle
   - outfit and props
   - colours
   - scale, framing and baseline
3. `talk` must be the base image with **only the mouth changed** (open mid-word). Nothing else moves.
4. If the tool supports it, reuse the same seed for every image of one character.

## 4. Poses (make all 12 for each character)

Expression poses keep the base's body position unless the description says otherwise.

| File suffix | What changes from `base` |
|---|---|
| `base` | Relaxed neutral standing pose, arms at the sides or a natural resting gesture from the character sheet; calm, friendly neutral face |
| `talk` | Identical to `base`; mouth open mid-word, a small natural "ah/eh" shape |
| `happy` | Big warm smile, eyes softly curved into crescents, slight lift of the shoulders |
| `sad` | Eyes lowered, brows tilted up in the middle, small downturned mouth, shoulders slightly dropped |
| `surprised` | Eyes wide, small "o" mouth, eyebrows raised, upper body leaning back a little, one hand lifted |
| `worried` | Brows knitted, small frown, hands held together in front of the chest |
| `thinking` | One hand to the chin, eyes looking up and to the side, mouth closed |
| `annoyed` | Arms crossed, flat unimpressed look, one eyebrow slightly lowered |
| `embarrassed` | Blushing cheeks, sheepish smile, one hand rubbing the back of the head, eyes glancing aside |
| `point` | One arm fully extended, index finger pointing off to the viewer's left (giving directions), looking that way, mouth slightly open |
| `bow` | Polite Japanese bow, about 30° from the waist, hands at the sides (women may fold them in front), eyes down. The feet stay on the same baseline |
| `wave` | One hand raised beside the head, palm out, mid-wave, friendly smile |

Character-specific adjustments are listed in each sheet.

## 5. Characters

### 5.1 `kenji`: Kenji Tanaka (田中 健二), 26
**Role:** Narita Airport security guard, finishing a twelve-hour night shift.
**Build:** Tall and broad-shouldered, athletic, slightly tired posture.
**Face and hair:** Short spiky black hair, a little messy at the end of the night. Warm hazel eyes. Friendly strong jaw, faint stubble, slight under-eye tiredness.
**Outfit:**
- Navy airport-security jacket (#1f2a44) with a small reflective strip and a shoulder patch reading "AIRPORT SECURITY"
- Black crew-neck T-shirt underneath, dark navy work trousers, black boots
- A white ID card on a navy lanyard
- A rubber-banded stack of English flashcards sticking out of the left breast pocket
- A small radio clipped to the belt

**Colour accents:** navy, black, soft blue (#60a5fa).
**Personality in the face:** Big-brother energy, calm and quietly funny, a little anxious under the jokes.
**Base pose:** Standing easy, one hand on the back of his neck, an approachable half-smile.
**Pose notes:**
- `bow`: a crisp, practiced work bow.
- `point`: confident directions, like he does this all day.

### 5.2 `aiko`: Aiko Nakamura (中村 愛子), 20
**Role:** University student who works part-time at a konbini in Ueno.
**Build:** Petite, lively.
**Face and hair:** Shoulder-length dark brown-black hair with bright pink tips (#f472b6), a small hair clip, big round brown eyes, expressive brows.
**Outfit:**
- Oversized pastel peach-and-cream knit sweater, sleeves past her wrists
- Pleated cream skirt, white socks, brown loafers
- A canvas tote bag with a cat keychain
- A dog-eared photocopied English short story held in one hand, covered in pencil question marks

**Colour accents:** peach, cream, pink.
**Personality in the face:** Bubbly, curious, endlessly kind, a little scatterbrained.
**Base pose:** Weight on one foot, holding the photocopy against her chest, a bright open smile.
**Pose notes:**
- `happy`: an excited hop-ready energy, both hands up.
- `embarrassed`: hides half her face behind the photocopy.

### 5.3 `yumi`: Officer Yumi Sato (佐藤 由美 巡査), 29
**Role:** Police officer at the Narita kōban (police box).
**Build:** Medium height, straight upright posture.
**Face and hair:** Sleek black chin-length bob, sharp amber eyes behind thin silver rectangular glasses, composed face.
**Outfit:**
- 1990s Japanese police uniform: light-blue uniform blouse, navy tie, navy trousers, black shoes
- Navy police cap held under one arm, or worn
- A silver badge on the chest
- A ballpoint pen behind one ear
- A clipboard with a lost-dog flyer clipped to it (a white shiba with a red collar)

**Colour accents:** light blue, navy, a touch of lavender (#a78bfa).
**Personality in the face:** Precise, by-the-book, dry humour, fair. Rarely smiles big.
**Base pose:** Standing straight, clipboard held at the waist, a neutral professional look.
**Pose notes:**
- `happy`: a small, controlled smile; she's not effusive.
- `annoyed`: taps the pen on the clipboard.
- `bow`: a precise, formal bow.

### 5.4 `hiroshi`: Hiroshi Yamamoto (山本 博), 31
**Role:** Salaryman at a trading company, commuting into Tokyo on a Saturday.
**Build:** Slim, average height, slightly stiff posture.
**Face and hair:** Neatly combed dark-brown hair with a side part, black rectangular glasses, clean-shaven, serious but kind face.
**Outfit:**
- Crisp white shirt with the tie loosened (muted green-grey #5b7a6a, not bright green)
- Charcoal suit trousers, black leather shoes
- Suit jacket folded over one arm
- A thick pocket diary, and a folder of contract printouts with little English sticky notes

**Colour accents:** white, charcoal, muted teal-grey.
**Personality in the face:** Reserved and formal at first, secretly a softie.
**Base pose:** Standing politely, folder held in front with both hands, a slight formal nod.
**Pose notes:**
- `worried`: mouthing his English line, nervous.
- `bow`: a textbook business bow.
- `thinking`: pushes his glasses up.

### 5.5 `emily`: Emily Carter (エミリー・カーター), 23
**Role:** Australian backpacker who works the desk at an Asakusa hostel.
**Build:** Medium height, relaxed, energetic.
**Face and hair:** Wavy chestnut hair in a loose high ponytail with loose strands, bright blue-green eyes (keep them teal or blue, not bright green), freckles across the nose, sun-kissed skin.
**Outfit:**
- Light-blue zip hoodie over a white T-shirt
- Faded jeans rolled at the ankle, worn white sneakers
- A woven travel bracelet
- A battered pocket notebook of Japanese words with doodles

**Colour accents:** light blue, denim, warm yellow (#fbbf24) on a small hair tie.
**Personality in the face:** Optimistic and adventurous, tough love. A little brittle under the jokes.
**Base pose:** Hip cocked, notebook in one hand, a confident grin.
**Pose notes:**
- `point`: playful.
- `wave`: a big, open wave.
- `sad`: a quiet, homesick look.

### 5.6 `nakajima`: Shūhei Nakajima (中島 修平), 52 — counter framing
**Role:** Keisei railway station attendant at the Narita Airport station ticket window.
**Framing (different from the others):**
- Waist-up, seen from the front through a ticket window.
- The window counter's front edge fills the **bottom 20%** of the image: a waist-high grey counter top with a small coin tray, a notepad and a pencil on it.
- He sits or stands behind it; his hands may rest on the counter.
- No glass reflections, so his face stays clear.
- Everything outside the counter and his body is the flat `#00FF00` background.

**Face and hair:** Neat short grey hair, rimless glasses on a thin cord, a gentle precise face with faint smile lines.
**Outfit:**
- Navy railway uniform jacket (#1f2a44) with brass buttons, white shirt, navy tie
- Navy railway cap with a small emblem
- A polished name badge reading 中島

**Colour accents:** navy, brass, steel blue (#4a7fb5).
**Personality in the face:** Gentle, endlessly patient, immovable on the rules, quietly proud of knowing every timetable.
**Base pose:** Hands folded on the counter, a calm, attentive look.

**Pose notes** (he never leaves the counter):
- `point`: points to the side with a pencil, toward the gates.
- `bow`: a small polite nod and dip of the head and shoulders.
- `thinking`: runs a finger down a timetable on the counter.
- `wave`: a small raised hand.
- `happy`: lights up, as if asked about timetables.

### 5.7 `fumiko`: Fumiko Tanabe (田辺 ふみこ), 68
**Role:** Runs a tiny standing-soba stand in the airport basement.
**Build:** Small and wiry, energetic, about 6.5 heads tall, a slight forward lean of someone always working.
**Face and hair:** White hair tied back under a blue cotton bandana, sharp lively eyes, deep smile lines, reading glasses hanging on a beaded chain (never worn).
**Outfit:**
- A stained white cooking apron over a faded mustard cardigan with the sleeves pushed up
- Dark slacks, rubber kitchen sandals
- A small towel over one shoulder

**Colour accents:** white, mustard, warm brown (#b5651d), blue bandana.
**Personality in the face:** Brusque and warm at the same time. Scolds you for not eating, but would feed anyone who works.
**Base pose:** Hands on hips, chin up, sizing the viewer up with a half-scowl that's almost a smile.
**Pose notes:**
- `annoyed`: wags a ladle at the viewer.
- `point`: jabs a thumb toward the sink, as if putting someone to work.
- `happy`: a rare, crinkly full laugh.
- `sad`: looks at the towel in her hands, thinking of her son.
- `bow`: a quick, no-nonsense shop bow.

## 5b. Side characters (8 more)

Same spec and art direction as above. Each needs **6 images**: `base`, `talk`, `happy`, `worried`, `point`, `bow`. Three of them are framed **waist-up behind a counter**, like Nakajima (§5.6): `ono`, `master` and `receptionist`. The counter's front edge fills the bottom 20% of the frame.

| id | Who | Look | Base pose |
|---|---|---|---|
| `mori` | Kenzō Mori (森 健蔵), 74, owner of Hostel Sakura in Asakusa | Short and stooped, cropped white hair, thick black-rimmed glasses, grey cardigan over a white undershirt, wooden geta sandals; holds a small teacup | Arms folded, a gruff sideways look, secretly kind eyes |
| `ono` | Ms. Ono (小野), 34, airport information-counter clerk; counter framing | Neat bun, pale-blue airline-style uniform with a small silk scarf, name badge, phone headset around her neck; a folded terminal map on the counter | Polite smile, hands together on the counter |
| `gensan` | Gen-san (源さん), 71, old street musician in Ueno Park | Wiry and weathered, knitted watch cap, patched olive army-surplus coat, fingerless gloves, old acoustic guitar on a cord, warm creased eyes | Holding a can of warm tea, relaxed and smiling |
| `taisho` | The Taishō (大将), 55, owner-cook of a small izakaya | Big and broad, twisted white headband, navy happi coat over a white T-shirt, sleeves rolled, a towel at the belt | Hands on hips, booming grin |
| `kaneko` | Kaneko-san (金子), 46, market vendor at Ameyoko | Stocky, short buzz cut, rubber apron over a quilted vest, money pouch at the waist, a wooden price board under one arm | Mid-shout with one hand cupped to his mouth, cheerful |
| `master` | The Master (マスター), 63, owner of a retro kissaten; counter framing | Slim, silver hair slicked back, neat moustache, white shirt, black bow tie, dark vest; a glass siphon coffee maker on the counter | Quietly polishing a cup, faint smile |
| `receptionist` | Clinic receptionist, 40; counter framing | Pink-white nurse-style uniform, low ponytail, a pen on a cord; a clipboard with a form on the counter | Gentle, attentive, pen poised |
| `ishida` | Dr. Ishida (石田), 58, neighbourhood clinic doctor | Tall, grey at the temples, round wire glasses, white coat over a cardigan, stethoscope, kind tired eyes | Calm, reassuring half-smile, hands in coat pockets |

Name the files the same way, e.g. `mori_base.png`, `mori_talk.png`, `ono_point.png`. That's 48 images. Do `base` and `talk` for all 8 first.

## 6. Delivery

Name each file `<id>_<pose>.png`, all lowercase, using the ids above:

```
kenji_base.png   kenji_talk.png   kenji_happy.png   kenji_sad.png
kenji_surprised.png   kenji_worried.png   kenji_thinking.png   kenji_annoyed.png
kenji_embarrassed.png   kenji_point.png   kenji_bow.png   kenji_wave.png
… and the same 12 for aiko, yumi, hiroshi, emily, nakajima, fumiko
```

Save every file into:

```
/Users/ishan/Lexical_Revived/lexical_revived/
```

That's 84 files. If time is short, deliver in this order:
1. All 7 `base` images.
2. `talk` for each character.
3. `happy`, `worried`, `surprised`, `point`, `bow`.
4. The rest.

Partial sets work: the game falls back to the base image for any missing pose.

Then turn them into game sprites by running this from `/Users/ishan/Lexical_Revived/lexical`:

```
node scripts/prepare-sprites.mjs /Users/ishan/Lexical_Revived/lexical_revived
```

The script removes the green background, trims each image and fits it to the game's 432 × 578 sprite frame. It writes the base sprites to `web/public/characters/<Name>.png` and the poses to `web/public/characters/<id>/<pose>.png`. It also records which poses each character has in `server/content/courses/ja/story.toml`. Nothing else needs editing; restart the game server to pick up the new poses.

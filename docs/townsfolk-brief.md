# Lexical — townsfolk sprite brief (12 generic background people)

These are **generic passers-by**: the stranger you ask for directions, a shop clerk, a station worker. The game uses whichever one fits whenever an unnamed person speaks. They must read as **background people**, clearly less important than the main cast, so they are drawn with **less detail**.

Six are for the Japan course (Tokyo, spring 1998) and six for the New York course (Manhattan, spring 1998). Each needs **one image only** (`base`): **12 PNGs in total**.

## 1. Technical spec

Same as the main cast, so they line up with everyone else on stage:

| | |
|---|---|
| Format | PNG |
| Aspect ratio | 3:4 portrait (e.g. 1024 × 1365) |
| Background | **Flat solid pure green `#00FF00`**, edge to edge: no gradient, floor, shadow or scenery |
| On the character | **No pure or bright green anywhere**, so the background keys out cleanly |
| Framing | Full body, top of head to feet; the figure fills about 92% of the image height, centred, feet at the bottom |
| View | Body turned three-quarters toward the viewer's left, face toward the viewer |
| Not allowed | Text, logos, watermarks, borders, speech bubbles, extra people, props bigger than a bag |

Style reference: attach `/Users/ishan/Lexical_Revived/lexical/web/public/characters/Kenji_Tanaka.png` for scale and line style only.

## 2. Art direction: simpler than the main cast

Same anime visual-novel style as the main cast, but **pared down**:
- **Shading:** flat colours with **one shadow tone**: no gradients, no rim light, no blush, no fabric folds beyond the essential.
- **Faces:** simplified. Small, plain eyes without big highlights, a simple mouth, a calm neutral-friendly expression. Friendly but forgettable.
- **Colour:** a **muted, slightly desaturated palette** (greys, navies, beiges, soft browns), so the main cast always stands out against them.
- **Clothing:** plain outfits with a few clear shapes: no logos, patterns, jewellery or small accessories.
- **Pose:** relaxed and standing, arms at their sides or holding one simple item.

They should look like the same game's "NPC" tier: clean and pleasant, never detailed enough to look like a main character.

## 3. Japan course (Tokyo, 1998)

| File | Who | Look |
|---|---|---|
| `jp_salaryman.png` | Office worker, about 40, man | Charcoal suit, plain tie, short neat hair, a briefcase in one hand |
| `jp_officelady.png` | Office worker, about 28, woman | Beige skirt suit, shoulder-length hair, a small handbag over the shoulder |
| `jp_student.png` | High-school student, about 16 | Navy gakuran (boys' school uniform) with a stand-up collar, a school bag on the shoulder |
| `jp_obaachan.png` | Elderly woman, about 75 | Short and slightly stooped, a plain cardigan over a long skirt, a cloth shopping bag |
| `jp_clerk.png` | Shop or konbini clerk, about 25 | A plain striped shop apron over a white shirt, a name badge with no text |
| `jp_stationstaff.png` | Railway station worker, about 45 | Dark-navy railway uniform and cap, white gloves |

## 4. New York course (Manhattan, 1998)

| File | Who | Look |
|---|---|---|
| `ny_businessman.png` | Commuter, about 45, man | Grey suit, beige trench coat, a folded newspaper under one arm |
| `ny_businesswoman.png` | Office worker, about 30, woman | Navy blazer and slacks, hair pulled back, a tote bag |
| `ny_student.png` | College student, about 20 | Oversized flannel shirt over a plain tee, baggy jeans, a backpack on one shoulder |
| `ny_oldman.png` | Retiree, about 70 | Flat cap, a cardigan, slacks, a paper grocery bag |
| `ny_clerk.png` | Deli or shop worker, about 30 | A plain apron over a plain T-shirt, a pencil behind the ear |
| `ny_transit.png` | Subway station worker, about 50 | Navy transit-worker jacket with an orange reflective vest, no logos |

## 5. Delivery

Save the 12 PNGs, named exactly as above, into:

```
/Users/ishan/Lexical_Revived/lexical_revived/
```

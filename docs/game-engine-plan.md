# Game-state engine: plan (draft)

Status: **draft, collecting requirements.** Not built yet.

## 1. The problem

Today the AI director decides almost everything that matters, and the code only clamps it afterwards:

| The AI decides | What goes wrong |
|---|---|
| Money (`yen_delta`) | The police officer "re-checks the chart" four turns running; amounts drift |
| Items gained / lost | Things appear or vanish without a real transaction |
| Story flags (`flags_set`) | "Has ticket" set by a side effect; the train taken without paying |
| Scene done (`objective_complete`) | Stuck at a place whose goal was clearly met, or let through early |
| Where you go (`go_to`) | Sent somewhere unreachable, or held back from somewhere you were pointed |
| What's possible at all | Kenji tells you to chase the red-jacket thief, which the game can't do |

Each fix so far has been another rule in the prompt or another patch in code. The AI keeps inventing.

## 2. The principle

**Code owns the world. The AI is the actors and the narrator.**

- Everything that can *happen* (buying, borrowing, working, getting a bed, a place opening) is declared in the story data and executed by code, under rules the code checks.
- The AI only:
  1. **reads** what the player said: which declared action they're attempting and whether they made themselves understood;
  2. **acts out** the result the code already decided, in character.
- The AI can never change money, items, facts, goals or location. If it isn't declared, it can't happen, and the AI is told it can't.

## 3. What the story data declares

### 3.1 Items and money
- **Items:** id, name, icon; whether they're *unique* (one passport), *consumable* (onigiri) or *documents* (the fare slip).
- **Money:** a number only code changes, through actions.

### 3.2 Facts (replacing free-form flags)
Named facts about the world, of three kinds:
- **progress:** `reported_theft`, `has_bed`. Set only by actions.
- **knowledge:** `knows_hostel`, `knows_fare`. Set by actions, or by being told (a declared "who can tell you this").
- **world state:** `thief_gone`, `saturday`, `embassy_closed`. Fixed, or changed by triggers.

### 3.3 People
What each person *can* and *cannot* physically do, as rules, not prose:
- Kenji: can point the way and write an incident report. **Can't** leave his post, give money or walk you anywhere.
- Officer Sato: can take a report, lend ¥1,000 once, and top up ¥300 once.

### 3.4 Places: actions (affordances)
Each place lists the actions possible there. Each action declares:

| Field | Example (ticket window → *buy ticket*) |
|---|---|
| who performs it | `nakajima` |
| requires | money ≥ ¥1,270, fact `knows_fare` (optional) |
| costs | ¥1,270 |
| gives | item `train_ticket`, fact `has_ticket` |
| limits | once |
| player must convey | what they want, and where to (any language level) |
| refusal reasons | "not enough money" → the clerk says the fare and points at the coin tray |

Other examples:
- **Police box:**
  - *report theft*: once; gives `theft_report` and fact `reported_theft`
  - *loan*: ¥1,000, once, requires `reported_theft`
  - *top-up*: ¥300, once, requires `knows_fare` and a return visit
- **Soba stand:** *work the lunch rush*, which pays ¥500 and once a day.
- **Hostel:**
  - *ask for a bed*: requires asking politely, gives `has_bed`
  - *pay rent*: ¥3,500 a time

### 3.5 Places: impossible things
A short list the AI must never offer or imply here: "chase the thief (long gone)", "call home (no money for international calls)", "buy a ticket at the machines (kanji only, exact coins)". The code also filters suggested replies against it.

### 3.6 Goals and exits as conditions
- **Scene goal:** a condition, e.g. `has train_ticket`. Code checks it every turn; the AI no longer declares a scene finished.
- **Exits:** conditions, e.g. the train needs `train_ticket`, and the Asakusa guesthouse needs `knows_hostel`.

### 3.7 The story graph (a Detroit-style flowchart)
The story is a graph, not prose:
- **Nodes** are story beats with an entry condition and a completion condition, e.g. *Report the theft*, *Get the missing ¥270*, *Buy the ticket*, *Reach Tokyo*, *Find a bed*.
- **Branches** are the different ways through a node, each tied to declared actions at particular places. For *Get the missing ¥270*:
  - the police top-up (police box)
  - Aiko's loan (outside the terminal)
  - working Fumiko's lunch rush (soba stand)
  - Hiroshi covering the difference (station)
  - strangers' coins (station)
- **Active nodes** are the player's current objectives: entry met, completion not. The objective shown on screen is the active node's goal, never improvised.
- **Suggested replies come from the graph.** At any moment the code lists the branches open to the player *here and now*, meaning their actions' requirements hold right now. The AI turns one or two of them into lines at the player's level, plus a question or small talk. With ¥1,300 at the station, the open branch is *go to the ticket window and buy*, so "I don't have enough money" can't be offered.
- **The flowchart screen.** After each scene, as in Detroit: Become Human, the player sees the node they just finished, which branch they took, the branches they didn't (shown, but not spoiled), and what's ahead. It's also a natural place for the score.

### 3.8 Episodic memory (what happened to the player)
An append-only log **written by code**, never by the AI, whenever the engine changes the world:
- an action resolved: "Officer Sato topped you up ¥300. Money: ¥1,300."
- an item gained or lost: "Bought a ticket to Ueno (−¥1,270)."
- a node completed or a branch taken: "Got the missing fare: police top-up."
- a place visited for the first time, or a trigger firing.

Each entry records where, when (scene and turn), who, what, and the money after.

Every turn the AI gets:
- **The story so far:** recent entries in full; older ones summarised per chapter.
- **Facts the code works out now**, so the AI never has to do arithmetic:
  - "Money ¥1,300: enough for the ¥1,270 ticket."
  - "Has the ticket."
  - "Knows the fare."
  - "Owes Aiko ¥500."

This is separate from **character memories** (section 2 of the current build): what each person subjectively remembers about the player. Both feed the AI.

## 4. One turn, in the new engine

1. **Read (AI call 1, small and fast).** Given what the player said and this place's action list, return:
   - `attempting`: one action id, or `none` (small talk, questions);
   - `understood`: yes / partly / no;
   - the coaching verdict on their language.
2. **Resolve (code).** Check the rules for that action. Execute it (money, items, facts, goal check, exits) or refuse it with a declared reason. Nothing else changes.
3. **Log (code).** Append what happened to the episodic memory, then recompute the derived facts and the active story-graph nodes.
4. **Act (AI call 2).** It gets:
   - the outcome ("loan given: ¥1,000" or "refused: not enough money, short by ¥270")
   - the story so far and the current facts
   - who's here, with their motives and memories
   - the impossible list
   - the **open branches**

   It writes the beats and phrases the suggested replies from those open branches. It may only *describe* what code did.
5. **Check (code).** Validate the suggested replies (each one is an open branch, a question or small talk; real exits only; nothing impossible), then save.

The AI still owns everything that makes it feel alive:
- the words and the emotion
- small talk, jokes and backstory
- inner thoughts and hints
- memories
- small trust changes, within limits

## 5. Migration (proposed stages)

1. **Engine core:**
   - facts, items, money, actions, goal and exit conditions
   - the episodic log and derived facts
   - the story graph for Act 1
   - the read → resolve → log → act turn
   - replies generated from open branches

   Japan course, Act 1 only (airport to train), as the pilot.
1b. **Flowchart screen** after each scene.
2. **Japan course:** the rest of the places converted to declared actions.
3. **English course:** converted the same way.
4. **Remove the old direct-state fields** (`yen_delta`, `flags_set`, `objective_complete`, `go_to`) from the AI's output entirely.

## 6. Open questions (for Ishan)

- [ ] Waiting for more notes from Ishan (items, inventory, anything else the engine should cover).
- [ ] Should items be usable or showable by the player from the inventory (show the fare slip to the clerk, hand over the dog's lead)? That would make them a player action, not just something a character asks for.
- [ ] Should the action list be visible to the player in any form (hints, a "things I could do here" list), or stay hidden and only be reached by speaking?
- [ ] Time of day / days passing as a real system (shops open by day, rent due weekly), or keep it story-driven?
- [ ] Is the extra half-second per turn for the separate "read" call acceptable?
- [ ] Flowchart: show untaken branches as greyed-out silhouettes (Detroit style), or hide them until replay?
- [ ] Should a branch the player skipped stay open later (go back and work at the soba stand anyway), or close once the node is done?

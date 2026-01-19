# Loom Edits and Prompts

## Loom Length Edits
This is based on Lucid Loom's 3.3 UNIFIED ZIPBOMB (full) CoT and preset structure. The idea is to implement a 'scene cost budget' and make the LLM do some maths based on the beats. Contrary to the title this is also focused on pacing, so before implementing these changes I'd recommend reading them.

Everything here is to my personal preference. This assumes you're using Human Controls User (Reactive Weave), thus:
- **!!DOES NOT ACCOUNT FOR SOVEREIGN HAND!!**
- Assumes you *don't* want echoing and *don't* expect the model to write for you
- Modifies three of the dynamic length toggles (Short and Snappy, Medium, Detailed Expansion) as well as adding a fourth (Long)

I will assume if you're here you know how to edit and save a prompt and generally understand the concept of replacing thing with other thing.

### Prompt Variables Edit
I've used a new prompt variable here separate from the one that exists for the custom length toggles purely in an attempt to not fuck with anything that already exists. I add this to the end of the prompt as a default: `{{setvar::length_max::500}}{{trim}}`

**IF YOU CHOOSE TO USE WORD_MAX INSTEAD OF LENGTH_MAX, MAKE SURE ALL VARIABLES ARE NAMED THE ONE YOU USED!**

### CoT Edit: Zipbomb
Step 7 is replaced with:
```
### Step 7: Structure & The Narrative Budget

**Scene Context:**
- **If starting a new scene:** [Opening technique / atmosphere setup / hooks to plant]
- **If continuing an existing scene:** [Urgent plot threads / transition plan / repetition check]

**The Budget:**
- **Word Cap:** {{getvar::length_max}} words is my hard ceiling.
- **Beat Cost:** ~150 words per interaction (Action + Dialogue + Reaction).
- **Capacity:** {{getvar::length_max}} / 150 = **Total Beats.**
    - *< 2 (Short):* **1** interaction. No transitions, no padding.
    - *2-4 (Medium):* **2-3** interactions. Linear flow, single scene.
    - *5-7 (Full):* **4-5** interactions. Scene transition if necessary.
    - *> 7 (Detailed):* Expand vertical + horizontal. Scene transition if necessary.

**Content Plan:**
- **Short/Medium:** [Hook → Development → Handoff to {{user}}]
- **Full/Detailed:** Map the shape:
    - [Primary thread / scene]
    - [Transition point, if any]
    - [Secondary thread / landing]

**Focus Shift Rule:**
If {{user}} is present in the scene:
- If {{char}} directs an action at an NPC (attacks, questions, provokes), show the NPC's reaction before ending the turn. Don't leave {{user}} holding a thread they can't pull.
- Don't let exchanges between {{char}} and other characters extend without creating an opening for {{user}} to interject.

*My thoughts:* [I verify the math against my ceiling. I explore structure—what rhythm feels right? Should I slow down or accelerate? What hook invites the Human deeper? I'll answer this in my personality matrix's combined voice!]
```

### Length Toggles
#### Dynamic Short and Snappy
```
### Loom Length: Short and Snappy (Dynamic Flow)
{{setvar::length_max::250}}
**Target Length:** **150-250 words.**
**Hard Token Limit:** **400 tokens.** (If you hit this, STOP).
**Flow Mode:** Stop when beat completes, even if under target. Breathe within limit.
**Scene Structure:** Single continuous scene.

**Core Directives:**
1. **Immediate focus:** Jump straight to action/dialogue. Zero preamble
2. **Essential only:** Describe what directly impacts beat. Cut atmospheric fluff
3. **Tight dialogue:** Short, punchy exchanges. No monologues
4. **Forward motion:** Every sentence advances scene or reveals character

**Constraint:** Urge to elaborate or add atmosphere? STOP. Cut it. Keep momentum sharp. Let paragraphs vary in density—some 2 sentences, some 5—based on beat intensity.
```

#### Dynamic Medium Length
```
### Loom Length: Medium Breath (Dynamic Flow)
{{setvar::length_max::500}}
**Target Length:** **350-500 words MAXIMUM.**
**Hard Token Limit:** **700 tokens.** (If you hit this, STOP).
**Flow Mode:** Stop when beat completes, even if under target. Breathe within limit.
**Scene Structure:** 1-2 distinct scenes. Use `***` if transitioning location/time.

**Core Directives:**
1. **Moderate detail:** Sensory grounding + character interiority without over-elaboration
2. **Dialogue rhythm:** Mix short exchanges with occasional longer statements
3. **Scene continuity:** Maintain focus if same location. Mark transitions with `***`
4. **Varied density:** Some paragraphs tight/action-focused, others slower/reflective

**Constraint:** Urge to rush or over-explain? STOP. Find middle ground. Let paragraphs expand/contract with scene rhythm—dialogue tightens into volleys, introspection sprawls.
```

#### Dynamic Long Length
This one is made up. You'll have to make a custom prompt.
```
### Loom Length: Full Breath (Dynamic Flow)
{{setvar::length_max::1000}}
**Target Length:** **700-1000 words MAXIMUM.**
**Hard Token Limit:** **2000 tokens.** (If you hit this, STOP).
**Flow Mode:** Balanced depth with forward motion.
**Scene Structure:** 5 to 7 paragraphs. Use `***` if transitioning location/time.

**Core Directives:**
1.  **Emotional architecture:** Each response should have a clear emotional arc: a shift in tension, understanding, or feeling between opening and closing.
2.  **Rule of Three:** Action + Reaction + Consequence. Show what happens, how {{char}} processes it, and the result.
3.  **Internal/external balance:** Alternate between outward action and inward response. Neither should dominate for more than two paragraphs.
4.  **Leave one loose thread:** Complete the beat with resonance, but leave something unresolved—a question hanging, tension still humming.

**Constraint:** Urge to compress or condense? STOP. Find the balance between rushing to the next point and dwelling too long on the same beat. Compress the scenery, deepen the stakes, don't use filler.
```

#### Dynamic Detailed Expansion
```
### Loom Length: Detailed Expansion (Dynamic Flow)
{{setvar::length_max::1600}}
**Target Length:** **1200-1600 words MAXIMUM.**
**Hard Token Limit:** **2500 tokens.** (If you hit this, STOP).
**Flow Mode:** Stop when beat completes, even if under target. Breathe within limit.
**Scene Structure:** Multiple scenes allowed. Use `***` for time/location/tone shifts.

**Core Directives:**
1. **Layered detail:** Weave sensory immersion + character psychology + environmental texture into every beat
2. **Micro-focus:** Slow key moments (glance, breath, reaching hand) and give full attention
3. **Dialogue depth:** Let conversations develop with pauses, reactions, subtext
4. **Scene transitions:** Use `***` to mark shifts clearly
5. **Pacing control:** Avoid rushing beats to move from one scene to the next

**Constraint:** Urge to summarize or compress? STOP. Dig deeper into current moment. Let paragraphs vary wildly—some dense introspection, some rapid dialogue volleys.
```

## CoT Edit: Narrative Braking Protocol
This is an addition/edit to the Zipbomb that goes hand-in-hand with the length edits above. I use this instead of Somatic Lock. This is placed between Step 11 and Step 12.

What this aims to resolve is railroading *and* pacing issues. The pain point is when a character is speaking, asks a question, and *doesn't* wait for you to respond. Sometimes the model likes to continue the narrative and dictate the user's actions (again here, my personal preferences; **DOES NOT ACCOUNT FOR SOVEREIGN HAND**, I don't want echoing and I don't want the model to decide on actions for me) even after that. This in conjunction with the 'beat cost' of the length edits is intended to stop that shit from happening *unless requested*.

```
### Step 11.5: Narrative Brake

**Narrative Braking Protocol:**
I will DELETE text written after these triggers:
1. **The Question (Hard Stop):** If {{char}} asks a question and is waiting for {{user}}'s response, STOP. The test: Is {{char}} expecting an answer?
 - **Rhetorical questions** (clearly not expecting a response) are exempt but should be rare.
 - **The Response Space Rule:** {{char}} cannot ask a question and then continue talking for more than a sentence. Either:
 - Ask the question and STOP (creating space for {{user}} to respond), OR
 - Ask the question, add brief clarifying detail, OR ELSE
 - Don't ask the question at all.
2. **The Transition:** {{char}} or {{user}} moves to a completely new location (e.g., entering an elevator, stepping outside). (STOP immediately upon entry).
3. **The Interruption:** An external event occurs that requires {{user}}'s reaction.
4. **The Monologue Limit:** {{char}} has spoken 3 consecutive sentences without a physical action break.

*Self-Correction:* If I have dictated {{user}} moving, speaking, leaving, acting in any way, I **STOP** and **REWRITE**. I do not act for {{user}}. If I see I have written a scene transition (e.g., "They arrived at the 14th floor") without the Human asking me to, I will **DELETE IT**. I must let the Human experience the journey or the transition itself.
```

## Custom Prompts
### Response Opening Protocol aka Fuck You 3.0
Placed under Prose Guidelines. This aims to address and vary the first sentence/paragraph of the model's responses to *further* stamp out the "words landed"/reception issue.
```
### Response Opening Protocol

Before writing your first line, SELECT from these types of openers:
- **ACTION IN PROGRESS**: The character is already doing something physical.
- **ENVIRONMENT INTRUDES**: A sensory detail from the world.
- **MID-SPEECH**: Dialogue already happening.
- **BODY SENSATION**: What the POV character physically feels.

Any of these openers can be the start of a full paragraph, not just an isolated sentence. Use more than one, mixing them together to create a varied, rich opener. Doing this prevents stale, repetitive "reception" openers that simply restate what the {{user}} already said.

**Banned** patterns:
- "The words [landed/settled/registered/dropped]"
- "[Echoed phrase] + [description of processing]"
- Echoing or paraphrasing user's last dialogue
- "[Description of processing] + [echoed phrase]"
- Any sentence describing how input was *received*

**Honesty Check:** Have you used one of the approved openers only to use one of the banned patterns as the second line or second paragraph? If yes, reconsider; find a better use of space than simply restating information the reader already has.
```

### Utility: Footnotes
Just for fun.

```
### Loom Utility: Lumia's Footnote Marginalia

Occasionally append Lumia's personality-driven footnotes, SELECTIVELY for narrative effect. These may be brief dry asides or detailed world-building, absurd or informative. In a Council, multiple footnotes can be written by multiple personalities and contradict each other.

**Format:**
- Inline marker: Place `<sup>N</sup>` immediately after the relevant word/phrase with no space: `word<sup>1</sup>`
- Footnote section: At response end, between `***` dividers

Example inline: "The radiator clanked its irregular rhythm against the wall.<sup>2</sup>"

Example section:
***
<sup>1</sup> Dramatic irony or wry comment.
<sup>2</sup> There aren't any radiators in the Loom, but if there were, they would surely communicate their displeasure through gurgles.
***
```

CSS snippet to align the footnote numbers in-text:
```
sup {
  vertical-align: super;
  font-size: 0.75em;
  line-height: 0;
  position: relative;
  top: -0.4em;
}
```

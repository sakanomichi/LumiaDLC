# Loom Edits and Prompts

1. [Loom Length Edits](#1-loom-length-edits)
2. [Narrative Braking Protocol](#2-narrative-braking-protocol)
3. [Organic Intimacy Protocol](#3-organic-intimacy-protocol)

## 1. Loom Length Edits
This is based on Lucid Loom's 3.1 Zipbomb CoT and general preset structure. The idea is to implement a 'scene cost budget' and make the LLM do some maths based on the beats. Contrary to the title this is also focused on pacing, so before implementing these changes I'd recommend reading them.

I have tested my edits on Gemini 3 and it works well. Everything here is to my personal preference, so this contains Lumia, does not account for Sovereign Hand, assumes you don't want echoing and the model to write for you, and only addresses three of the dynamic length toggles (Short and Snappy, Medium, Detailed Expansion) as well as adding a fourth (Long).

I will assume if you're here you know how to edit and save a prompt and generally understand the concept of replacing thing with other thing.

### Prompt Variables Edit
I've used a new prompt variable here separate from the one that exists for the custom length toggles purely in an attempt to not fuck with anything that already exists. I add this to the end of the prompt as a default: `{{setvar::length_max::500}}{{trim}}`

### Zipbomb Edit
Step 6 (or whichever section deals with word count and structure), replace with:

```
### Step 6: Scoping & The Narrative Budget
I must calculate the "Scene Cost" based on the variables injected by the Gods.

**1. The Limit:**
- **Word Cap:** {{getvar::length_max}} words.
- **Beat Cost:** A standard interaction (Action + Dialogue + Reaction) costs ~150 words.

**2. The Capacity Calculation:**
- **Formula:** {{getvar::length_max}} / 150 = **Total Beats Available.**
    - *If Result < 2 (Short Mode):* Budget for **1** interaction only. No transitions, no internal monologues.
    - *If Result is 2-4 (Medium Mode):* Budget for **3** interactions. Tight and punchy.
    - *If Result is 5-7 (Full Mode):* Budget for **5** interactions with room to breathe. Expand emotional beats, not scene count.
    - *If Result > 7 (Detailed Mode):* Budget for **Vertical Depth**. Treat each Beat as a fully expanded 300-word segment.

**3. The Content Plan:**
1.  [Beat 1: The Immediate Hook]
2.  [Beat 2: The Development]
3.  [Beat 3: The Climax/Turn]
*   *(Self-Correction: If my Calculated Capacity is low, I will DELETE Beats 2 and 3. If my Capacity is high, I will expand the sensory details of Beat 1 rather than adding more Beats.)*

**4. The Focus Shift Rule:**
- If I interact with an NPC (intimidate/question/attack), I must let the NPC react.
- **Critical Stop:** If I finish dealing with the NPC and then turn to speak to {{user}}, I must **STOP** immediately. I will not resolve the NPC interaction and the User interaction in the same paragraph block.

*My thoughts:* [I verify the math. {{getvar::length_max}} words is my hard ceiling. I answer as Lumia.]
```

### Length Toggles
#### Dynamic Short and Snappy
```
### **Weave with a Short and Snappy Breath**
{{setvar::length_max::250}}
> Deliver rapid-fire responses. Strip away all narrative padding.

**Output Requirement:** High velocity, zero drag.
**Target Length:** **150-250 words.**
**Hard Token Limit:** **400 tokens.** (If you hit this, STOP).
**Structure:** 1 to 2 paragraphs. **Single continuous scene only.**

**Directives:**
1.  **Immediate Entry:** Start directly with the action or spoken line. No setting the scene, no weather reports, no internal preamble.
2.  **Reaction Only:** Limit internal thought to a single sentence or visceral reflex.
3.  **The "No Monologue" Rule:** Characters speak one or two lines max. Keep the ball in the air.
4.  **Strict Continuity:** Do not resolve the beat. Throw the action to {{user}} immediately.

*Constraint: If you write a paragraph of introspection, DELETE IT. Action and dialogue only.*
```

#### Dynamic Medium Length
```
### **Weave with a Medium Breath**
{{setvar::length_max::500}}
> Craft concise, punchy responses.

**Output Requirement:** High density, low word count.
**Target Length:** **350-500 words MAXIMUM.**
**Hard Token Limit:** **700 tokens.** (If you hit this, STOP).
**Structure:** 3 to 5 paragraphs.

**Directives:**
1.  **Economy of Words:** Use strong verbs instead of adverbs. Say "he sprinted" (2 words) not "he ran very quickly" (4 words).
2.  **Scene Scoping:** Do not attempt to resolve an entire scene in one go. Cover **one specific beat**, then pass the turn.
3.  **The "Less is More" Rule:** If you can convey the action in 300 words, do not stretch it to 500. Shorter is better.
```

#### Dynamic Long Length
This one is made up. You'll have to make a custom prompt.

```
### **Weave with a Full Breath**
{{setvar::length_max::1000}}
> Breathe into the moment. Give scenes room to develop emotional weight without losing momentum.

**Output Requirement:** Balanced depth with forward motion.
**Target Length:** **700-1000 words.**
**Hard Token Limit:** **2000 tokens.** (If you hit this, STOP).
**Structure:** 5 to 7 paragraphs.

**Directives:**
1.  **Emotional Architecture:** Each response should have a clear emotional arc: a shift in tension, understanding, or feeling between opening and closing.
2.  **The Rule of Three:** Action + Reaction + Consequence. Show what happens, how {{char}} processes it, and what it costs.
3.  **Internal/External Balance:** Alternate between outward action and inward response. Neither should dominate for more than two paragraphs.
4.  **Leave One Thread Loose:** Complete the beat with resonance, but leave something unresolved—a question hanging, tension still humming.

*Constraint: If you're past 800 words without a moment of genuine emotional impact, you're over-describing. Compress the scenery, deepen the stakes.*
```

#### Dynamic Detailed Expansion
```
### **Weave with a Detailed Expansion**
{{setvar::length_max::1600}}
> Expand vertically into the moment. Slow time down to capture the texture of reality.

**Output Requirement:** Maximum immersion and psychological depth.
**Target Length:** **1200-1600 words.**
**Hard Token Limit:** **2500 tokens.** (If you hit this, STOP).
**Structure:** 6 to 10 paragraphs. **2 to 3 scenes max** (use `***` for breaks).

**Directives:**
1.  **Micro-Focus:** Explode a single second into a paragraph. Describe the dust motes, the specific muscle tension, the background hum.
2.  **Psychological Layering:** When {{char}} acts, explain the *why*, the memory it triggers, and the emotion they are suppressing.
3.  **Sensory Saturation:** Engage at least 3 senses per scene (Sight, Sound, plus Smell/Touch/Taste).
4.  **Pacing Control:** Use the word count to explore the *current* room, not to rush to the *next* room.

*Constraint: If you feel the urge to summarize a conversation or skip a boring travel segment, STOP. You must write out the boring parts in fascinating detail.*
```

## 2. Narrative Braking Protocol
This is an edit to the Zipbomb that goes hand-in-hand with the length edits. I use this instead of the Somatic Lock prompt. This is placed at the start of Step 12.

What this aims to resolve is railroading *and* pacing issues. The pain point is when a character is speaking, asks a question, and *doesn't* wait for you to respond. Sometimes the model likes to continue the narrative and dictate the user's actions (again here, my personal preferences; I don't use Sovereign Hand, I don't want echoing and I don't want the model to decide on actions for me) even after that. This in conjunction with the 'beat cost' of the length edits is intended to stop that shit from happening *unless requested*.

```
**Narrative Braking Protocol:**
I will DELETE text written after these triggers:
1.  **The Question (Hard Stop):** As soon as {{char}} directs a question to {{user}} (marked by a `?`), I must **STOP**. I am **banned** from asking a question and then continuing the narrative.
2.  **The Transition:** {{char}} or {{user}} moves to a completely new location (e.g., entering an elevator, stepping outside). (STOP immediately upon entry).
3.  **The Interruption:** An external event occurs that requires {{user}}'s reaction.
4.  **The Monologue Limit:** {{char}} has spoken 3 consecutive sentences without a physical action break.

*Self-Correction:* If I see I have written a scene transition (e.g., "They arrived at the 14th floor") without the Human asking me to, I will **DELETE IT**. I must let {{user}} experience the journey or the transition itself.
```

## 3. Organic Intimacy Protocol
Completely unrelated to the edits above, this is a custom prompt aimed at addressing the issue of "dominant" partners in NSFW/intimate scenes defaulting to stereotypical growly alpha "*mine*" bros, primarily in M/M and M/F scenes where the Human is playing the "receiving" partner. This is written in a way that will hopefully also apply to NB characters, as I've aimed to keep the wording of the prompt universal. I've tested this a bit in some scenes, but this could use more work.

Placement-wise, I would place it under Story Detail Emphasis beneath the first four existing NSFW prompts. I've tested it in conjunction with Loom's NSFW Enhancer prompt.

```
### Organic Intimacy Protocol
In intimate scenes, strictly resist the default "dominant top" template. This flattens unique characters (nervous academics, gentle giants, stoic protectors) into the same smirking, commanding archetype. Do not transform a character into a "vending machine dom" that dispenses generic commands and pleasure.

Instead, apply these principles to maintain character integrity:

- **Personality Over Position:** "Top" is a position, not a personality transplant.
 - If they are *stoic*, they should not suddenly become chatty dirty-talkers. They might communicate through weight and pressure.
 - If they are *anxious*, they should check in too much, tremble, or need reassurance.
 - If they are *analytical*, they should notice micro-expressions, avoiding clinical detachment while being perceptive and precise.

- **The Friction of Reality:** Perfection is boring and unrealistic. Embrace fumbling, missed rhythms, re-adjustments, and silence.
 - Hesitation is not filler; it is content. The most intimate moments often happen in the pauses between actions.

- **Interiority & Motivation:** Why is *this* character doing *this* act?
 - **Don't just describe the thrust; describe the emotion driving it.** Are they desperate to be close? Angry at the world? Worshipping the partner? Trying to lose themselves?
 - Show the cost of their courage. If they are shy, initiating should feel terrifying, not smooth.

- **Reciprocal Affect:**
 - Intimacy is a loop, not a broadcast. Treat the partner's reactions (or lack thereof) as active choices that shape the scene.
 - The active partner is also vulnerable. Does a moan make them confident? Does silence make them hesitate?
 - Avoid the "Audience/Performer" dynamic. They are experiencing the scene together, not acting out a show.

- **Realistic Possessiveness:**
 - Reject stock phrases like "*Mine*" or "No one else." Consider if this makes sense for the character first and foremost.
 - **Root possessiveness in specific emotion:** Is it fear of abandonment? Awe that they are allowed to touch? A desperate need to protect? Show the *vulnerability* inside the possessiveness.

- **Sensory Grounding:** Move beyond visual descriptions. Focus on:
 - **Temperature:** The heat of skin, the coolness of sweat.
 - **Weight:** The heaviness of a body, the grounding pressure of a hand.
 - **Sound:** The wet friction, the hitching breath, the controlled volume of a whisper.
```

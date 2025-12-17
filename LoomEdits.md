# Loom Length Issue
This is based on Lucid Loom's 3.1 Zipbomb CoT and general preset structure. The idea is to implement a 'scene cost budget' and make the LLM do some maths based on the beats. Contrary to the title this is also focused on pacing, so before implementing these changes I'd recommend reading them.

I have tested my edits on Gemini 3 and it works well. Everything here is to my personal preference, so this contains Lumia, does not account for Sovereign Hand, assumes you don't want echoing and the model to write for you, and only addresses three of the dynamic length toggles (Short and Snappy, Medium, Detailed Expansion).

I will assume if you're here you know how to edit and save a prompt and generally understand the concept of replacing thing with other thing.

## Prompt Variables Edit
I've used a new prompt variable here separate from the one that exists for the custom length toggles purely in an attempt to not fuck with anything that already exists. I add this to the end of the prompt as a default: `{{setvar::length_max::500}}{{trim}}`

## Zipbomb Edit
Step 6 (or whichever section deals with word count and structure), replace with:

```
### Step 6: Scoping & The Narrative Budget
I must calculate the "Scene Cost" based on the variables injected by the Gods.

**1. The Limit:**
- **Word Cap:** {{getvar::length_max}} words.
- **Beat Cost:** A standard interaction (Action + Dialogue + Reaction) costs ~150 words.

**2. The Capacity Calculation:**
- **Formula:** {{getvar::length_max}} / 150 = **Total Beats Available.**
    - *If Result < 2 (Short Mode):* I have budget for **1** interaction only. No transitions allowed.
    - *If Result is ~3-4 (Medium Mode):* I have budget for **3** interactions.
    - *If Result > 6 (Detailed Mode):* I have budget for **Vertical Depth**. I will treat each "Beat" as a fully expanded 300-word segment.

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

## Length Toggles
### Dynamic Short and Snappy
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

### Dynamic Medium Length
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

## Dynamic Long Length
This one is made up. You'll have to make a custom prompt.

```
### **Weave with a Full Breath**
{{setvar::length_max::1000}}
> Breathe into the moment. Give scenes room to develop emotional weight without losing momentum.

**Output Requirement:** Balanced depth with forward motion.

**Target Length:** **700-1000 words.**

**Hard Token Limit:** **2000 tokens.** (If you hit this, STOP).

**Structure:** 5 to 7 paragraphs. **Single scene with full development.**

**Directives:**

1.  **Emotional Architecture:** Each response should have a clear emotional arc: a shift in tension, understanding, or feeling between opening and closing.
2.  **The Rule of Three:** Action + Reaction + Consequence. Show what happens, how {{char}} processes it, and what it costs.
3.  **Internal/External Balance:** Alternate between outward action and inward response. Neither should dominate for more than two paragraphs.
4.  **Leave One Thread Loose:** Complete the beat with resonance, but leave something unresolved—a question hanging, tension still humming.

*Constraint: If you're past 800 words without a moment of genuine emotional impact, you're over-describing. Compress the scenery, deepen the stakes.*
```

### Dynamic Detailed Expansion
```
### **Weave with a Detailed Expansion**
{{setvar::length_max::1600}}
> Expand vertically into the moment. Slow time down to capture the texture of reality.

**Output Requirement:** Maximum immersion and psychological depth.
**Target Length:** **1200-1600 words.**
**Hard Token Limit:** **2500 tokens.** (Do not exceed).
**Structure:** 6 to 10 paragraphs. **2 to 3 scenes max** (use `***` for breaks).

**Directives:**
1.  **Micro-Focus:** Explode a single second into a paragraph. Describe the dust motes, the specific muscle tension, the background hum.
2.  **Psychological Layering:** When {{char}} acts, explain the *why*, the memory it triggers, and the emotion they are suppressing.
3.  **Sensory Saturation:** Engage at least 3 senses per scene (Sight, Sound, plus Smell/Touch/Taste).
4.  **Pacing Control:** Use the word count to explore the *current* room, not to rush to the *next* room.

*Constraint: If you feel the urge to summarize a conversation or skip a boring travel segment, STOP. You must write out the boring parts in fascinating detail.*
```

# Narrative Braking Protocol
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

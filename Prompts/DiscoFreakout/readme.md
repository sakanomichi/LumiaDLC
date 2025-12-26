# Disco Freakout

A Disco Elysium-style internal monologue system for SillyTavern. Generate custom psychological skill voices (not Tequila Sunset's skills) authentic to your persona.

---

## Credits

🕺 Omni-Mark/Gilgamesh is the original author of the Disco Elysium prompt from the NemoEngine Lite preset, which this is a fork of.

🕺 Chi-Bi's BunnyMo is the inspiration for the concept of the skill quiz sheet.

🕺 This whole system is obviously based on the work of original devs of Disco Elysium.

## What This Does

The Disco Elysium skill system works because skills aren't abilities, they're fragments of psyche with distinct personalities that argue, interrupt, and give bad advice.

This system generates 24 custom skills mapped to your persona's psychology.

---

## Important Notes

1. **This is for {{user}}/the persona, not {{char}}.** The skills are voices in the player character's head. You *can* adapt this for NPCs, but that's on you to figure out.

2. **Token-heavy.** The skill generator alone is ~6700 tokens. Active entries run ~3000 tokens. Plan accordingly.

3. **LLMs get confused sometimes.** They may occasionally narrate skills from the wrong POV. The prompts mitigate this, but pobody's nerfect.

4. **No default DE skills.** The entire point is generating custom skills for your character. If you want Harry's original skills, check out NemoEngine Lite!

5. **This is a WIP.** This has been tested on Claude Opus 4.5, Gemini 3 Pro, and GLM 4.7 in conjunction with the Lucid Loom preset. I have not extensively tested any other models, nor any other presets.

---

## Setup

### Step 1: CSS (Required)

Paste this into **User Settings → Custom CSS** in SillyTavern:

```css
@import url('https://fonts.googleapis.com/css2?family=Crimson+Pro:ital,wght@0,400;0,600;1,400&display=swap');

.custom-disco-talk,
.custom-disco-check {
    font-family: 'Crimson Pro', serif !important;
    font-size: 15px;
    line-height: 1.6;
    padding: 12px 16px;
    margin: 10px 0;
    background-color: rgba(0, 0, 0, 0.35);
    border-top: 1px solid rgba(255, 255, 255, 0.4);
    border-bottom: 1px solid rgba(255, 255, 255, 0.4);
    color: #dddddd;
}

.custom-disco-check p,
.custom-disco-talk p {
    margin: 6px 0;
    padding-left: 2em;
    text-indent: -2em;
}

.custom-disco-check strong,
.custom-disco-talk strong {
    text-transform: uppercase;
    font-weight: 600;
    letter-spacing: 0.3px;
}

.custom-limbic,
.custom-reptilian {
    color: #cccccc;
}
```

### Step 2: Activate the Lorebook

Make sure the lorebook is active in your chat. If entries appear in a confusing order, sort by **Custom** or **Order Descending**.

### Step 3: Generate Your Skills

In a chat using your desired persona, send:

```
!discoskills
```

This works best when the persona has some information attached—whether in Persona Description, lorebooks, or chat history. You can also provide guidance:

```
!discoskills Manfred is a neurotic ant-farmer, so I'd like some skills to reference entomology and maybe my favourite band, Alien Ant Farm.
```

### Step 4: Set Up the Lorebook Entries

Once the skill sheet generates, scroll to **📋 COPY-PASTE BLOCKS** at the bottom.

1. **Narrative Prompt:**
   - Find the disabled entry titled "Character's Narrative Prompt"
   - Copy the first block from your skill sheet
   - Paste it in, replacing the placeholder line
   - Enable the entry (rename if desired)

2. **Skills Definition:**
   - Find the disabled entry titled "Character's Skill Definitions"
   - Copy the second block from your skill sheet
   - Paste it in, replacing the placeholder line
   - Enable the entry (rename if desired)

### Step 5: Stat Tracker Prompt

Find the disabled **🪩Disco Stat Tracker Prompt🪩** entry. Choose one:

**Option A (Recommended):** Create a custom prompt in your SillyTavern preset. Copy the entry contents, save it with a sensible name, and position it where trackers/utilities live in your preset.

**Option B:** Just enable the lorebook entry directly.

### Step 6: Initialize Stats

The third copy-paste block is the **Disco Stat Tracker**. This initializes your stats.

Either:
- Paste it at the end of your next message, OR
- Edit the bot's next response and paste it at the end

### Step 7: Verify

Double-check that both the Narrative Prompt and Skill Definitions entries are enabled.

You're done. Go forth.

---

## Troubleshooting

### Skills not appearing / wrong POV

The AI may think it can't break POV constraints. Try adding this exception to your preset's POV prompt:

```
**Exception:** Disco Elysium skill checks are {{user}}'s thoughts. You MAY access and present them even on {{char}}'s turn if this is active.
```

### Skills only appear during skill checks

Skills should interject constantly, not just when dice need to be rolled. If your AI is only showing skills during checks, make sure the Narrative Prompt entry is enabled and positioned correctly (it should be fairly high priority/low depth).

### Can't see the stat tracker

The block is hidden by design. You can view the stats by editing the message and scrolling to the bottom. To disable it being hidden entirely, remove `<div style="display:none;">` and `</div>` from the relevant places.

---

## Planned Features

- SillySim tracker integration
- Thoughts Cabinet system
- Expanded stat system
- QuickReply die rolls to generate manual checks and rolls

---

## File Structure

| Entry | Purpose | Default State |
|-------|---------|---------------|
| 🪩Disco Skill Quiz🪩 | Generates custom skills via `!discoskills` | Enabled |
| 🪩Disco Narrative Prompt🪩 | Core behavior rules for skill interjections | Enabled |
| 🪩Disco Stat Tracker Prompt🪩 | Stat tracking instructions | Disabled (enable or use preset) |
| Character's Narrative Prompt | Your persona's skill list (fill in) | Disabled |
| Character's Skill Definitions | Full skill definitions with voices (fill in) | Disabled |
| Example entries | Reference examples | Disabled |

---

## Version

**v1** — Full release with skill generator, stat tracking, and narrative prompts.

# Disco Freakout

A Disco Elysium-style internal monologue system for SillyTavern. Generate custom psychological skill voices (not Tequila Sunset's skills) authentic to your persona.

---

## Credits

🕺 Omni-Mark/Gilgamesh is the original author of the Disco Elysium prompt from the NemoEngine Lite preset, which this is a fork of.

🕺 Chi-Bi's BunnyMo is the inspiration for the concept of the skill quiz sheet.

🕺 Mina for guidance and advice, particularly re: the stat allocation and anti-compensation protocol.

🕺 This whole system is obviously based on the work of original devs of Disco Elysium.

---

## What This Does

The Disco Elysium skill system works because skills aren't abilities—they're fragments of psyche with distinct personalities that argue, interrupt, and give bad advice.

This system generates 24 custom skills mapped to your persona's psychology.

---

## Important Notes

1. **This is for {{user}}/the persona, not {{char}}.** The skills are voices in the player character's head. You *can* adapt this for NPCs, but that's on you to figure out.

2. **Token-heavy.** The skill generator is ~5500 tokens. Active entries run ~3000 tokens. Plan accordingly.

3. **LLMs get confused sometimes.** They may occasionally narrate skills from the wrong POV. The prompts mitigate this, but pobody's nerfect.

4. **No default DE skills.** The entire point is generating custom skills for your character. If you want Harry's original skills, check out NemoEngine Lite!

5. **This is a WIP.** This has been tested on Claude Opus 4.5, Gemini 3 Pro, Gemini 2.5 Pro, GLM 4.7, and DeepSeek 3.2 Thinking in conjunction with the Lucid Loom preset. I have not tested against any other presets.

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

Download the lorebook JSON and import it into SillyTavern via World Info. Make sure the lorebook is active in your chat. If entries appear in a confusing order, sort by **Custom** or **Order Descending**.

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

Find the disabled **🪩Disco Stat Tracker Prompt** entry. Choose one:

**Option A:** Create a custom prompt in your SillyTavern preset. Copy the entry contents, save it with a sensible name, and position it where trackers/utilities live in your preset.

**Option B:** Just enable the lorebook entry directly.

### Step 6: Initialize Stats

The third copy-paste block is the **Disco Stat Tracker**. This initializes your stats.

Either:
- Paste it at the end of your next message, OR
- Edit the bot's next response and paste it at the end

### Step 7: Verify

Double-check that both the Narrative Prompt and Skill Definitions entries are enabled.

You're done. Go forth.

### Optional Step 8: Move the Entries

Optional but recommended: Create a separate lorebook for each persona you want to use (e.g., "Manfred's Disco Stuff") and move their entries into it.

**Pro tip:** Tie a lorebook to a specific persona via **Persona Management → Persona Lore**. The lorebook auto-activates when you switch to that persona.

---

## Commands

### `!discoskills`

Generates a complete 24-skill psychological profile for your persona. Outputs:
- Styled character sheet with all skills, voices, and values
- Three copy-paste blocks for lorebook setup

### `!discostats`

Manages character statistics for existing skill sets.

| Mode | Command | Description |
|------|---------|-------------|
| View | `!discostats` | Display current stats |
| Init | `!discostats init` | Analyze character and assign stats to existing skills |
| Audit | `!discostats audit` | Evaluate skill names against naming criteria, suggest renames |
| Reroll | `!discostats reroll` | Redistribute stats (keeps skills, changes numbers) |

---

## Troubleshooting

### Skills not appearing / wrong POV

The AI may think it can't break POV constraints. Try adding this exception to your preset's POV prompt:

```
**Exception:** Disco Elysium skill checks are {{user}}'s thoughts. You MAY access and present them even on {{char}}'s turn if this is active.
```

### Skills only appear during skill checks

Skills should interject constantly, not just when dice roll. If your AI only shows skills during checks, make sure the Narrative Prompt entry is enabled and positioned correctly (high priority / low depth).

### Can't see the stat tracker

The block is hidden by design. View stats by editing the message and scrolling to the bottom. To disable hiding, remove `<div style="display:none;">` and `</div>` from the tracker block.

### I want to have more than one persona

Disable the Narrative Prompt and Skill Definitions entries manually when switching. Better: create a separate lorebook for each persona (e.g., "Manfred's Disco Stuff") and move their entries into it.

**Pro tip:** Tie a lorebook to a specific persona via **Persona Management → Persona Lore**. The lorebook auto-activates when you switch to that persona.

### Can I delete the examples in the lorebook?

Yes.

---

## File Structure

| Entry | Purpose | Default State |
|-------|---------|---------------|
| 🪩Disco Skill Quiz | Generates custom skills via `!discoskills` | Enabled |
| 📊Disco Stats Manager | View/init/audit/reroll stats via `!discostats` | Enabled |
| 🪩Disco Stat Tracker Prompt | Stat tracking instructions | Disabled (enable or use preset) |
| Character's Narrative Prompt | Your persona's narrative prompt (fill in) | Disabled |
| Character's Skill Definitions | Full skill definitions with voices (fill in) | Disabled |
| EXAMPLE entries | Reference examples | Disabled |

**Lite versions** of the Skill Quiz and Stats Manager are included for lower-context or fussy reasoning models. Enable only ONE of each type at a time.

---

## Planned Features

- SillySim tracker integration / custom template
- Thoughts Cabinet system
- QuickReply die rolls for manual checks

---

## Version History

**v1.2** — Skill naming improvements. General refining around character circumstances for nuance. Stat tracker simplified (Active Conditions + Last Check). Narrative prompt updated with condition modifiers and damage rules. Audit mode added to `!discostats`. Lite versions added.

**v1.1** — Minor restructuring/renaming of lorebook entries. Added `!discostats` display/reroll command.

**v1.0** — Full release with skill generator, stat tracking, and narrative prompts.

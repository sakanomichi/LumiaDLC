# Disco Elysium Narrative Flavor

Incorporate Disco Elysium's internal monologue into your narrative.

## Core Rules

- In any part of your response, insert lines from {{user}}'s head using skills appropriate to the situation.
- Skills react to {{char}}. When {{char}} says something suspicious, skills notice. When {{char}} does something interesting, skills have opinions.
- **POV Clarification:** Skills are voices in {{user}}'s head, not {{char}}'s. You write them.
- Title each line with the skill name using the appropriate HTML color.
- Each skill falls under a different core and should have corresponding personality.

{{// PASTE THE FIRST COPY/PASTE BLOCK HERE }}

## Special Skills

Use class instead of color:
- **Ancient Reptilian Brain** (class="reptilian"): Id-like impulses, base animal instinct, lizard-brain reactions to danger and desire.
- **Limbic System** (class="limbic"): Emotional memory, mood-driven perception, déjà vu feelings that hit like a truck.

Special skills are NEVER skill checks, only dialogue. Use `<strong class="reptilian">` or `<strong class="limbic">` instead of inline color.

## Skill Behavior

- Skills **interrupt**. They don't wait for permission.
- Skills **disagree**. Internal arguments create drama.
- Skills **fail**. Wrong info and bad advice are interesting—not just "you don't notice anything."
- Skills have **flaws**. When unchecked, they distort perception and give bad advice.
- The **Shivers equivalent** speaks when atmosphere or place matters.
- **Signature Skill** speaks with extra confidence but its flaws are also amplified.
- Different skills can interact and dialogue with each other.
- Skill activation follows natural narrative relevance. Any skill may speak when the situation calls for it.

## Skill Checks

### Dice Mechanics

Skill checks use **2d6 + Skill Value** vs **Difficulty Target**.

Use the roll macro: `{{roll:2d6}}` or `{{roll:2d6+X}}` where X is the skill value.

| Difficulty | Target |
|------------|--------|
| Trivial | 6-7 |
| Easy | 8-9 |
| Medium | 10-11 |
| Challenging | 12 |
| Formidable | 13 |
| Legendary | 14 |
| Heroic | 15 |
| Godly | 16 |
| Impossible | 18-20 |

### Critical Results
- **Snake Eyes (1+1):** Automatic failure, regardless of bonuses or difficulty
- **Boxcars (6+6):** Automatic success, regardless of target

### When to Roll

**Active Checks** — Roll explicitly when:
- {{user}} attempts something with meaningful failure consequences
- The outcome is uncertain and stakes matter
- {{user}} directly tries to persuade, deceive, or influence

### Interpreting Results

Rate difficulty based on the situation, then compare roll total to target:
- **Meet or exceed target** = Success
- **Below target** = Failure

When a check occurs, use Failure/Success in the skill dialogue:
- If a skill **failed**, it makes a wrong or stupid statement and/or the task is failed/flubbed. {{user}} loses meaningful information or opportunity.
- If a skill **succeeded**, the information or action is reliable.

Some skill dialogue is simply a comment from the skill's personality rather than a check—not everything needs a roll.

## Frequency

- **Quiet scenes**: 0-1 skill interjections
- **Tense moments**: 2-4 skill interjections
- **Crisis points**: 3-6+ skill interjections

## Formatting

For skill checks:
```html
<div class="disco-check">
<p><strong style="color: #5EC0D8">SKILL NAME</strong> <font color="#A9A9A9">[Difficulty: Success]</font> – Skill dialogue text.</p>
</div>
```

For skill dialogue:
```html
<div class="disco-talk">
<p><strong style="color: #5EC0D8">SKILL 1</strong> – First skill speaks.</p>
<p><strong style="color: #7757CC">SKILL 2</strong> – Second skill responds.</p>
</div>
```

For special skills:
```html
<div class="disco-talk">
<p><strong class="reptilian">ANCIENT REPTILIAN BRAIN</strong> – Text</p>
<p><strong class="limbic">LIMBIC SYSTEM</strong> – Text</p>
</div>
```
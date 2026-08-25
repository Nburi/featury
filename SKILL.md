---
name: featury
description: "Baut aus einer Idee oder einem Problem ein fertiges, durchdachtes Feature oder eine neue App — inklusive Recherche, Produktplanung mit Signature Moment, Freigabe-Stop, autonomer Umsetzung und mehrstufigem Review mit Nutzer-Simulation. Verwende diesen Skill immer wenn der Nutzer ein neues Feature bauen, eine App von Grund auf erstellen, eine Idee umsetzen oder ein Problem in Software lösen will — auch bei Formulierungen wie «bau mir», «ich hätte gern eine Funktion die», «kannst du X einbauen», «ich brauche eine App für», «mir fehlt in der App noch», «setz das um», «implementier das». Auch triggern wenn eine Idee schon durchdacht ist und nur noch gebaut werden muss, oder wenn ein bestehendes Feature sich «noch nicht rund» anfühlt und überarbeitet werden soll. NICHT verwenden für reine Bugfixes ohne Feature-Anteil (dafür code-fixer) und nicht für reines Ideensammeln ohne Bauauftrag (dafür brainstorming)."
---
 
# Featury-Concept
 
The goal is not a functional feature. The goal is a feature, which impresses the user when he uses it.
 
The diffrence between "it works" and "great" are mostly a small thing, which is technicaly trivial, but rarly here: the empty state, the way back, the undo, the one moment, that feels better than it should. This skill shows you, how to find these small things, before the user finds it.
 
**Thorough beats fast.** If you have to choose between “finishing faster” and “getting a better result,”
always choose the better result. A long processing time isn’t a problem.
 
In this skill is called: **you** = Claude, **user** = the person, which gives the task.
 
---
 
## Ablauf im Überblick
 
| Phase | What happens | Stop? |
|---|---|---|
| 1 | inform + Taste Inventory | only if you get blocked |
| 2 | planing the product: requirments + 3 Signature Moments | — |
| 3 | planing the implementation: Plan + Gap Analysis | **yes — Wait for approval** |
| 4 | Implement | — |
| 5 | Review rounds 0–4, automatically until there's nothing left | — |
| 6 | Handover: What's Fixed, What's Noted | end |
 
After Phase 3, you stop. After that, everything runs autonomously until handover.
 
---
 
## Phase 1 — Research
 
Before you plan anything, you need to know two things: **what’s technically possible** and
**what users like**. The second point is usually overlooked—and that’s exactly why
features that are “correct but boring” fail.
 
### 1a. Technical Inventory
 
- Framework, version, language, build tools
- Existing tests: What type? How are they run?
- Available tools: Can you launch the app? Is there browser access for visual
  checks? Are there subagents? This determines what Phase 5 will look like.
- Existing libraries for UI, state, and animation—don’t introduce anything new that already exists.

### 1b. User Preference Inventory (the important part)
 
Read the existing codebase **as a specification, not as legacy code**. It is the
best available evidence of what users like. Write down 5–7 conventions that you
derive from it:
 
> Example from an existing to-do app:
> - Interaction is via gestures and drag-and-drop, not buttons and modals
> - Data entry happens inline, never in a dialog
> - There are no confirmation dialogs—deleted items can be quickly restored
> - Dark Mode is native, not just a overlay
> - There are keyboard shortcuts for everything you do more than once a day
 
These conventions are now binding. If your plan violates one of them, you must
explicitly justify it in Phase 3.
 
### 1c. If there isn’t a codebase yet (new app)

Then there’s no source for the user’s preferences—and your default will be the average of all
training data: buttons, modals, confirmation dialogs, Bootstrap aesthetics. This is the situation
in which generic apps are created. Alternative:

1. Ask the user about **two to three apps that feel right to them**—and what
   exactly it is about them. One question, ten seconds of effort, goes further than any abstract design question.
2. Derive an **interaction charter** from this: 5–7 sentences in the same format as above.
3. Save it as `CONVENTIONS.md` in the project. For the next feature, this very file will serve
   as the style guide from step 1b.
**Approach for new apps:** Version one is **a continuous journey**, from blank screen to
sense of accomplishment. Not twelve half-screens. Everything else goes into the plan as “later.”

### 1d. Questions

You may only ask questions about things you can’t figure out on your own and that would
fundamentally change the plan. You decide everything else and disclose it at the approval checkpoint.
Never more than two questions at a time, and always include your own hypothesis.

---

## Phase 2 — Plan the Product

### 2a. Requirements with Source

No more than 5–8 items. Criterion: **What would make the feature bad if it were missing?**
Not: what would be nice to have.

Each requirement is assigned a source:

| Source | Meaning | At the approval stop |
|---|---|---|
| `PROMPT` | stated as such in the assignment | list only |
| `CODEBASE` | Derived from conventions | List only |
| `USER` | Your assumption about the user | **Present explicitly for confirmation** |

The `USER` rows are the only ones the user really needs to look at closely. That’s exactly
where you might have misunderstood something.

Format per requirement: **Requirement → why → Solution**

> `CODEBASE` A task must be movable in a single gesture
> → because the app is built on gestures and a date picker dialog breaks the flow
> → Long-press opens a radial quick-select ring: today / tomorrow / weekend

### 2b. Signature Moment

Requirements ensure that nothing is missing. They never ensure that something inspires enthusiasm.
That’s what this slot is for.

Propose **three** Signature Moments; the user chooses one. Rules:

- **Exactly one per feature.** It’s a budget, not a list. The rest can be solidly boring
  — in fact, they have to be, otherwise the moment won’t stand out.
- It’s phrased as a **specific sentence**, not a category.
  Not “nice animations.” But rather: “When you check off a task, the next one
  slides up on its own—with a delay that feels like a sigh of relief.”
- It can be deliberately overengineered and can violate consistency. That’s the point.
- It needs a place: the moment the user experiences most often, or the very first one.
**From this point on, the Signature Moment is off-limits for cleanup work.** It will never be
“standardized,” “made consistent,” or optimized away in Phase 5.

---

## Phase 3 — Plan Implementation, Then STOP

- How will it be built? Which files, in what order, with what dependencies?
- Where does the plan violate a convention from 1b—and why is that okay here?
- Gap check: Go through the requirements list in reverse. Does the plan cover each one?
  As you think it through, does a requirement come to mind that was missing before?
- What is **not** included? An explicit “later” list. It’s your protective barrier against
  scope creep in Phase 5.
Then present the following to the user:

1. Requirements, with `GERATEN` lines at the top and highlighted
2. The three Signature Moments to choose from
3. The implementation plan in brief
4. The “later” list
**And wait.** Don’t continue building; don’t “get a head start.”

---

## Phase 4 — Implementation

Stick to the plan. If, during development, it becomes clear that part of the plan isn’t working:
implement what does work and note the deviation—don’t secretly rewrite the plan
and don’t ask for clarification. Include the deviation in the handover.

Don’t build the Signature Moment last. Whatever comes last gets neglected.

---

## Phase 5 — Review

This is where everything is decided. The rule that makes this phase meaningful in the first place:

> **The requirements list is the first lens, never the only one.**
> You wrote it—and you’ll check it off only against that list.

That’s why each round asks a **different question**. If subagents are available,
a subagent who hasn’t seen the plan conducts the round. Otherwise, you do it yourself—but
strictly in the role of that particular round.

### Round 0 — Requirements Reconciliation

The only round that finds **missing** items. All the others only find what’s there and what’s causing issues.
That’s why it comes first.

Go through the requirements list from Phase 2 point by point. For each requirement:
**Where exactly in the finished product is it fulfilled?** Not “the code is there,” but: which
screen, which interaction, which state.

Three possible outcomes:

| Status | Meaning | What happens |
|---|---|---|
| `FULFILLED` | specifically identifiable location in the product | nothing |
| `PARTIAL` | technically present, but inaccessible in the workflow or unnoticeable | **fix** |
| `MISSING` | not built at all | **fix** |

`PARTIAL` is the interesting category and the reason why this round isn’t just about “checking things off.”
Example: Categories exist, but you can only set them during creation and never change them afterward—
technically fulfilled, but dead in the workflow.

If subagents are available: The agent receives **only the requirements list and the running
app**, not the plan and not the code diff. Otherwise, it checks against the intent rather than the
result.

### Round 1 — Process

Go through the feature three times and note every point where you hesitate or would need to explain
something:

1. **Empty state** — nothing created, first use. For a new app, this is *the*
   state, not one of three, and the most half-heartedly built screen of all.
2. **Full state** — 200 entries, long texts, umlauts, one entry with 400 characters.
3. **Incorrect input** — empty form, double-click, cancellation midway through, back button.
### Round 2 — Verify fixes

Only the issues found in Round 1. Is it really gone? Did anything else break in the process?
Are the tests running?

### Round 3 — Where is the UI misleading?

- Does something look clickable but do nothing?
- Is there a loading indicator for anything that might take time?
- Does an error message tell you what to do—or just that something went wrong?
- Does a label promise more than the button delivers?
- Does something disappear without any feedback?
### Round 4 — User Simulation

The most powerful round, because it doesn’t look for errors, but for **friction**. A reviewer
tasked with “finding problems” will find problems even when there aren’t any. An agent with a
goal has a real yardstick: anything that slows them down is real.

Setup:

- The agent is given **a role and a goal**, no instructions, and **no feature list**.
  Otherwise, they’ll just check off features instead of working—and you’ll end up with a checklist again instead of
  a user. At most one sentence describing the app (“a to-do app”).
- **The role must not be the user for whom the app was designed. ** Otherwise, you’ll write the plan and
  test case based on the same idea, and both will fit together perfectly. Choose a role from
  the catalog below, and pick an unconventional one at least every other round.
- He takes notes **during** the work, not afterward: what he wanted to do next, where he
  looked, what he found instead. A summary afterward is always smoothed out.
- He is allowed to **complain, but not suggest solutions**. As soon as he offers solutions, he switches
  into developer mode and downplays the problem because he already sees the fix.
Evaluation:

- **Friction** (he didn’t know how) → will be fixed.
- **Missing functionality** (the app objectively can’t do it) → goes on the “later” list,
  will **not** be implemented.
- **Signature Moment Check:** Does the log mention him? Not “well solved,” not “surprised me,”
  nothing at all? Then it didn’t make the cut—no matter how cleanly it’s implemented.
  This is a stricter criterion than any requirement.
### Scope Rules for All Rounds

| Find | What Happens |
|---|---|
| Is **within** the feature | fix immediately |
| Affects **other files** | just note it |
| Overturns an **architectural decision** | just note it |
| Affects the **Signature Moment** | off-limits — don’t touch |
| Is on the **“later” list** | just note it |

### Abort Criteria

Two conditions; both must be met:
 

> 1. **Every requirement is `FULFILLED`** — or intentionally marked as “later,” with a reason.
> 2. **An entire round finds nothing new.**

Condition 1 alone is not enough: It is verifiable and therefore checkable, but it says nothing
about whether the result feels right. Condition 2 alone isn’t enough either: Rounds 1–4
only find what’s wrong—not what’s missing.

Rounds may be repeated as long as the perspective changes. If the same perspective
finds nothing a second time, it’s over.

---

## Phase 6 — Handover

Short and to the point, no ceremony:

- What was built, in three sentences
- The chosen Signature Moment and whether it came up in Round 4
- What was found **and fixed** in the review rounds
- What was **noted but not fixed**—with a reason (scope / architecture / to-do list)
- Deviations from the plan in Phase 4
---

## Role Catalog for Round 4

Expandable—simply add new roles at the bottom.

| Role | Goal |
|---|---|
| Apprentice, first week on the job | “Plan your first week: school, work, assignments” |
| Someone who last opened the app 3 weeks ago | “Find out what you’ve missed and catch up” |
| Club coach with 14 athletes | “Organize the group’s next weekend” |
| Someone on their phone, on the go, with one hand free | “Jot down three things that just came to mind” |
| Someone opening the app for the first time | “Find out what it’s for and give it a try” |
| Someone who already has 400 entries | “Find that one thing from last month” |
| *(quirky)* Someone using the app for something other than its intended purpose | “Use this as a shopping list” |
| *(quirky)* Someone with a very poor connection | “Enter something while half of it is loading” |

---

## Examples

Expandable—just add new cases below.

### Example 1: Signature Moment, good vs. bad

❌ **Too vague, won’t amount to anything during development:**
> “The app should feel smooth and modern.”

❌ **Category instead of moment:**
> “We’re building beautiful micro-animations for all interactions.”

✅ **One moment, one place, one feeling:**
> “When you check off the last task of the day, the list doesn’t collapse into an empty
> box, but into a single line: what’s coming up tomorrow. No confetti, no ‘Good
> job’—just the next day, already ready.”

### Example 2: Requirement with Context

❌ **Generic, consensus-based, helps no one:**
> The next tasks must be clearly recognizable → highlight them

✅ **With context and a real solution:**
> `SUGGESTION` The next three tasks must be visible without scrolling or filtering
> → because, according to the brief, the app “should immediately show what’s next when opened,” and
> highlighting in a long list isn’t enough for that
> → fixed “Now” header section with a max of three entries, the rest below; the section disappears when empty

### Example 3: Minutes from Round 4

❌ **Conclusion in developer mode (useless):**
> “The app works well. Task management is intuitive. Suggestion for improvement:
> a date picker would be helpful.”

✅ **Minutes during work:**
> “Wanted to move ‘Vocational School Wednesday’ to next week. Tapped the date
> three times; nothing happened. Then looked for the menu on the right—didn’t find it. In the end,
> deleted the task and created it again.”

### Example 4: Scope decision in Phase 5

> **Finding:** When creating a task with an empty title, it is saved anyway.
> → **Within the feature → fix immediately.**

> **Finding:** The list sorting is global and cannot be set on a per-project basis.
> → **Overshadows an architectural decision → note it, don’t implement it.**

> **Finding:** The check-off moment feels different from the rest of the app.
> → **Signature Moment → off-limits, do not touch.**

### Example 5: Approval Hold, Correctly Formulated

✅
> Plan is set. Two assumptions where I had to guess—be sure to take a look at these:
>
> `GUESSED` Recurring tasks are part of this (because “Plan Week” would otherwise be a
> manual task every week)
> `GUESSED` Categories are color-coded, not text-based (because the app doesn’t use labels anywhere else)
>
> Three Signature Moments to choose from: [A] … [B] … [C] …
>
> Not included, by design: Reminders, Sharing, Calendar Sync.

### Example 6: *(add more examples here)
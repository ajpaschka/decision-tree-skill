---
name: decision-tree
description: Maps a recurring strategic decision into an honest yes/no decision tree, rendered as Mermaid, with unanswerable questions marked as open gaps that double as a reading list. Use for recurring judgment calls, not one-off choices.
---

# /decision-tree — Map a Decision into a Yes/No Tree

Turns a fuzzy judgment call into an explicit, reusable map. The output is a dated
markdown artifact with a Mermaid flowchart (renders natively on GitHub; VS Code
needs a Mermaid preview extension).

The diagram is the cheapest part. What it forces is the product:
1. Every branch becomes an observable question (a fact you can check), not a judgment word
2. Every question you can't currently answer gets marked as an OPEN GAP: the tree
   doubles as a map of what still needs to be learned or read. A finished tree with
   zero gaps means the decision is understood; gaps show exactly where understanding stops.

## Input

A decision to map, e.g. `/decision-tree can this client data enter an AI tool?`
If no argument: ask which recurring decision to map. Push back if it's a one-off
choice. Trees are for decisions that recur; a one-off gets a recommendation, not a tree.

## Method

### 1. Frame the root
Pin down conversationally, not as a form:
- The decision, stated as a single question with a bounded set of outcomes
- Who decides (a tree for someone else's decision gets framed as a recommendation map)
- Reversibility: reversible decisions get shallow trees and a bias to act;
  irreversible ones earn depth

### 2. Force the questions
- Every node must be answerable by an observable fact, not a judgment.
  "Is it sensitive?" hides judgment; split it into checkable criteria.
- Yes/no only. A three-way fork is two questions; split it.
- Order by cheapness and kill-power: cheapest-to-answer, most-decisive
  questions go closest to the root.
- Mark open gaps: when the user or the facts on hand can't answer a node,
  do NOT guess. Style the node as an open gap and record what would answer it
  (a document to read, a person to ask, a term to verify).
- Unknown is its own answer, not a soft no. Gap edges are labeled "no / unknown"
  and route to the protective branch, never the permissive one. Folding unknown
  into no is how trees lie about what's actually in hand.
- Sort item questions from world questions, and send them opposite directions.
  Item questions change with every case ("does this contain identifiers?") and
  are what branches are for. World questions are the same for every case
  ("are we on enterprise terms?", "do the agreements permit processing?") —
  they're preconditions, not branches. List them in a Preconditions block above
  the tree; verifying one doesn't just close a gap, it deletes that branch and
  becomes a standing rule. (Contributed by community review, 2026-07-12.)
- Depth cap ~6 levels. Deeper means the root is several decisions; split the tree.

### 3. Classify the leaves
Every path ends in an action or outcome, never "it depends."

### 4. Write the artifact
Save as `decision-trees/YYYY-MM-DD-<slug>.md` in the owning project. Format:

    # Decision: <root question>
    **Made for:** <who decides> · **Reversibility:** ... · **Mapped:** YYYY-MM-DD
    **Status:** complete | N open gaps

    ## Preconditions (world questions — verify once, then they're standing rules)
    | # | Question | Verified? | What would answer it |

    ```mermaid
    flowchart TD
        A{First question?} -- yes --> B{Second?}
        A -- no --> STOP[Outcome: ...]
        B -- yes --> GAP1{{"OPEN GAP: what would answer this"}}
    ```

    ## Open gaps
    | # | Node | What would answer it | Where |

    ## Notes
    <anything that didn't fit a binary, honestly logged, not forced into the tree>

Mermaid conventions: {...} diamonds for questions, [...] rectangles for outcomes,
{{...}} hexagons for open gaps, yes/no labels always explicit.

### 5. Maintain, don't multiply
A tree gets updated in place when a gap closes (note the date in the Status line).
Superseded logic gets struck through or moved to Notes, never silently deleted.
If the same tree gets consulted three times, consider hardening it into a
permanent rule in your instruction files instead of leaving it as a map.

## Rules

- Never invent an answer to close a gap. A wrong tree is worse than an incomplete one.
- The tree records what was decided and why the branches exist; it is not a
  substitute for the policy doc or memo it may feed.
- If a tree disagrees with an existing standing rule, stop and reconcile
  before saving.

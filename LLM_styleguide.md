# LLM style guide

Notes for anyone — human or model — writing prose for this repository. Every "avoid" below
was cut from a real draft in this project. The paired examples are actual before and after
text, not invented illustrations.

---

## 1. Never define something by what it is not

This is the single most common failure. The construction sets up a strawman, knocks it down,
and leaves the reader without a definition.

| Cut | Kept |
|---|---|
| "The schema is not bookkeeping — it is prompt engineering." | *(deleted; the surrounding text already explained what the schema does)* |
| "Guarantees, not suggestions:" | "What the graph guarantees:" |
| "The artifact is not the answer. It is the argument." | "The output is the argument, which a reviewer can audit faster than re-deriving the answer." |
| "There is no framework — it is a `for` loop and a dispatch table." | "No library needed — we can implement this ourselves using a for loop and a dispatch table." |
| "Greedy decision making is not a compromise here — it is the correct algorithm." | "Greedy, one-step-at-a-time decision making makes this the right framework." |
| "What changes is everything around it." | *(replaced with a table mapping each node onto the problem)* |

Related: "X, not Y" in tables and captions. "Every reported result is verified | the graph,
not the prompt" became "| the edge from solve to verify" — which names the actual mechanism.

**Rule:** state what a thing is and how it works. If a contrast genuinely helps, make it a
separate sentence that stands on its own.

---

## 2. Do not tell the reader what is interesting, important, or the point

| Cut |
|---|
| "That is the point — with the problem out of the way you can see the raw shape of each framework." |
| "The bridge is the interesting part." |
| "The model's prior is almost right, which is the interesting case." |
| "That division is the entire pattern." |
| "That contract is the whole product." |
| "The gap between those two rates is the entire point of adaptive meshing." |
| "This handle pattern is the single most reusable idea in the talk." |
| "The point of a graph is what happens when things go wrong." |

Replacements state the fact and let it carry its own weight: "That gap between the two rates
is what adaptive meshing buys you." / "The handle pattern generalises."

**Rule:** no "the point is", "the entire X", "the whole X", "what's interesting here",
"the single most". If it is important, the reader will see it from the content.

---

## 3. No enumerator openers

Avoid "One catch.", "One caution:", "Two things to notice.", "Three things that will bite
you." They read as filler and they promise a list the reader did not ask for.

| Cut | Kept |
|---|---|
| "One catch. `await` is normally only legal inside an `async def`…" | *(folded into the paragraph: "Colab and Jupyter execute each cell inside an event loop, which is why `await` works here. In a `.py` script it is a `SyntaxError`.")* |
| "Three things that will bite you:" | "Three things worth knowing:" |

If a list is genuinely a list, use a heading or bullets and skip the announcement.

---

## 4. No insider asides or knowing winks

| Cut |
|---|
| "An honest detour, because it is the mistake everyone makes on their first MPC agent." |
| "this is the easiest of the four patterns to fool yourself with" |
| "**You will see this trigger.**" |
| "An agent recovering from its own bad tool call is the system working." |
| "None of these are things you want a language model deciding on a given Tuesday." |
| "whatever the model felt like doing that morning" |

**Rule:** the reader is a colleague, not an audience for a performance.

---

## 5. No editorialising about your own honesty or plainness

| Cut |
|---|
| "**The caveat, stated plainly:**" |
| "This is the honest way to answer…" |
| "The uncomfortable baseline." |
| "and worth saying out loud" |

Say the thing. Claiming to be honest is not the same as being honest, and it draws attention
to the writing rather than the subject. Actual honesty reads like the author's own line:
"(code hasn't been tested)".

---

## 6. No glib punchlines to close a section

| Cut | Kept |
|---|---|
| "Use `scipy.optimize` and go home." | "The problem is small and smooth, where scipy.optimize will beat this." |
| "MPC degenerates into the LLM grading its own homework: slower than ReAct and equally wrong." | "Scoring candidates with the same LLM that proposed them costs more without adding information." |
| "When 'it usually does the right thing' is not good enough, you stop asking and start wiring." | *(deleted)* |
| "a graph is ceremony — use ReAct" | "…that machinery buys you nothing, and ReAct is the better fit." |
| "You have built an expensive way to do a grid search." | "…it would be a grid search with extra steps." |
| "Earn each step." | *(deleted)* |

---

## 7. Write complete sentences

Bolded fragment leads followed by more fragments read like ad copy.

**Cut:**
> **The model decides.** What an agent is: a model in a loop with tools it can ask you to run.
> A tool is a Python function plus a JSON schema, and the schema is the only thing the model
> can see.

**Kept:**
> This notebook introduces agents. An agent is a language model running in a loop with access
> to tools, which are ordinary Python functions that the model can ask your code to run on its
> behalf.

Also cut: "Seven notebooks, meant to be run in order." → "The course is seven notebooks,
meant to be run in order."

**Rule:** subject, verb, object. No telegraphic fragments, no headline-style noun phrases
standing in for sentences.

---

## 8. Define every term and tool at first use

Two failures of this kind, both caught late:

- **scikit-fem appeared cold.** A notebook opened with `MeshTri`, `BilinearForm` and an
  L-shaped domain with no introduction. Fixed by naming the library, saying why it was chosen
  (pip-installable, pure Python, runs in Colab), stating the PDE, and listing the five steps
  the code performs.
- **"A graph is an XXX"** — an early draft gestured at the idea instead of defining it. Fixed
  with: "A graph is a directed graph in the standard sense: a set of nodes, plus directed
  edges specifying which node may follow which."

Jargon that was cut for being undefined at the point of use: *prior*, *exemplar*, *diffable*,
*handle-free*, *the four patterns* (a taxonomy from a document that no longer existed).

**Rule:** if a reader would have to look it up, define it in the sentence where it first
appears. Prefer a definition grounded in an instance the reader already knows — the
`CAD → mesh → solve → post` pipeline works well for this audience.

---

## 9. No forward or dangling references

- "Conditions compose with `|`, so we stop on whichever fires first" appeared before the code
  it described. Moved next to the code and expanded to explain that the SDK overloads the
  operator to mean *or*.
- Section headings referred to "Part 2 to 5" material inside a notebook that no longer used
  any of it.
- A slide promised "in the notebook we run ReAct vs. MPC head to head" after that comparison
  had been deleted from the notebook.

**Rule:** every cross-reference must resolve at the moment the reader hits it. Re-check them
whenever content moves.

---

## 10. Prefer the mechanism over the metaphor

Explanations that survived tend to name the actual object and what it does:

- "`route` is the only branch in the graph. It returns `END` when `verified` is true, `END`
  again when `attempts` has reached `MAX_ATTEMPTS`, and otherwise the string `"repair"`."
- "The LLM proposes plausible candidates from context. The simulator scores them against the
  forward model."

Explanations that were cut leaned on personification or flourish:

- "The simulator contributes what it is good at: being right."
- "Note what the LLM never does…"

---

## 11. Give real numbers

Vague quantities get replaced with measured ones wherever possible.

| Cut | Kept |
|---|---|
| "it cannot talk its way into a fourth attempt" | "`compute` is called four times — the first pass plus three repairs" |
| "the model's prior is almost right" | "Without drag the optimum is 45°. With drag it drops to about 38°." |
| "a limited number of requests per minute" | "15 requests per minute, 250,000 tokens per minute, and 1,000 requests per day" |

If a number cannot be verified, say so and link to where the reader can check it, rather than
rounding it into a vague phrase.

---

## 12. Headings name the content, not the rhetoric

| Cut | Kept |
|---|---|
| "LangGraph — what you actually bought" | "LangGraph — guarantees follow from structured graph computation" |
| "First, why the naive version is degenerate" | "The forward model" |
| "The exemplar: a design review" | "A question worth arguing about" |
| "TODO A1: uniform-refinement agent" | "A1: uniform refinement" |

---

## What good looks like here

Positive examples, all written by the repository's author:

> Tools allow us to switch an LLM from statistical inference on its training set to evaluating
> conventional code. Math can be done on a calculator, text can be regexed, and high
> consequence engineering tasks can be launched through conventional HPC.

> In many scientific workflows, however, we must progress through a rigid sequence of logic.
> For example, in finite elements, we go through a `CAD -> mesh -> solve -> postprocess`
> pipeline.

> This last task is a simple (but crucial!) example of why we use agents. They allow us to
> offload critical computation onto trusted, conventional software.

The common features: first person plural, a concrete engineering instance immediately after
the general claim, motivation framed as what the technique buys you, and a light practical
aside where one is genuinely useful ("we don't want to blow through our token budget!",
"not OK in > 2D!").

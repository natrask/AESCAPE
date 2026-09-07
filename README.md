# Agentic AI for Science & Engineering — AESCAPE 2026 Short Course

Materials for **SC03: Introduction to Agentic Workflows**, a half-day (morning) hands-on
tutorial at [AESCAPE 2026](https://aescape2026.usacm.org/home) — *AI-Empowered Simulations
& CAE Applications via Engineering Software 2.0 and 3.0*.

| | |
|---|---|
| **Event** | AESCAPE 2026 (USACM), Sep 8–11, 2026 |
| **Course** | [SC03 — Introduction to Agentic Workflows](https://aescape2026.usacm.org/short-courses) |
| **Date** | Tuesday, September 8, 2026 (morning) |
| **Venue** | Dallas/Fort Worth Airport Marriott, Dallas, TX |
| **Instructors of record** | Nathaniel Trask (Penn), Miguel Bessa (Brown) |
| **Format** | Hackathon-style: short talks interleaved with hands-on notebook work |
| **This block** | Session 1 (Trask), ~2 hours of a 9am-5pm day shared with three invited speakers |

---

## 1. Objectives

By the end of the morning, a participant who arrives knowing FEM but not agents should be able to:

1. **Say what an LLM agent actually is** — an LLM + tools + a loop — and draw the round trip
   (model emits `function_call` → we dispatch a Python function → we feed the result back as an
   observation → repeat until the model emits text).
2. **Get an API key and make a call.** Provision a key, store it safely, and run a single
   prompt through a provider SDK. A no-cost path is provided for participants without a key.
3. **Wrap an arbitrary Python function as a tool.** Write the JSON schema the model reads,
   understand which schema fields actually steer tool selection (`name`, `description`, `enum`,
   `required`), and dispatch by name with `try/except` so errors become observations.
4. **Place their problem on the framework landscape** — ReAct, multi-agent state machines
   (LangGraph / AutoGen / CrewAI), MPC-style plan-and-replan, and MCP as the tool-transport
   standard — and say *when each one beats plain ReAct*.
5. **Expose a simulation stack as a tool surface.** Review the conventional
   CAD → mesh → solve → post-process workflow in [scikit-fem](https://scikit-fem.readthedocs.io/),
   then re-cut it into JSON-callable tools with IDs, guardrails, and a state registry.
6. **Drive a real numerical experiment with an agent** and judge the result: agent-driven
   adaptive mesh refinement on the L-shaped Poisson problem, recovering the
   `O(h^2/3)` vs `O(N^-1/2)` gap that motivates adaptivity in the first place.
7. **Not trust the agent.** Run a second, independent *verifier agent* (manufactured solutions,
   tighter-tolerance oracle integrator) against the first agent's claim.

The through-line is the last point. The survey (§3) says the audience's stated pain is
*reliability*, not novelty — so verification is treated as a first-class pattern, not an appendix.

---

## 2. Structure

Session 1 (Trask) is the foundations + hands-on core. It is followed by three invited talks
that each show what the foundations look like at production scale in a different part of the
CAE stack.

| # | Session | Presenter |
|---|---------|-----------|
| 1 | **Foundations of agentic AI** — what agents are, frameworks, keys & API calls, tools from Python functions, scikit-fem as a tool surface, hands-on hackathon | Nathaniel Trask (Penn) |
| 2 | **Agentic frameworks for material modeling** | Miguel Bessa (Brown) |
| 3 | **A production agentic framework** | Jake Koester (Aperi Consulting) |
| 4 | **Geometry & meshing agents** | Steve Owen (Sandia National Laboratories) |

Session 1 generalizes a lecture + hackathon given in **ENM5320 (Penn, Spring 2026)**; the source
material is in [enm5320_material/](enm5320_material/) and is the starting point, not the
deliverable — see §5 for what changes.

---

## 3. Who is in the room (pre-workshop survey)

Raw responses: [survey/](survey/). n = 6 at time of writing.

**Roles.** Industry (2), grad student (2), principal engineer (1), government/national lab (1).
A professional, not student, audience.

**FEM fluency — high.** 5 of 6 answered "100% confident" on the CAD → mesh → solve → post
workflow; 1 wanted a brief refresher.
→ *Do not teach FEM.* The scikit-fem segment is a short re-framing of a workflow they already
own, aimed squarely at "what does this look like as a tool surface for an LLM."

**ML fluency — mixed.** Actively work in ML (3), poked around (2), no background at all (1).
→ Avoid assuming PyTorch/autodiff literacy. Nothing on the core path should require training a model.

**Agentic AI — bimodal, and this is the central design constraint.** Exactly half answered
"I use it and am taking this course to learn advanced techniques"; the other half answered
"I have heard of it, but am hoping for a complete crash course."
→ Every hands-on block needs a **working reference cell** (the crash-course half runs and reads it)
**and a stretch goal** (the advanced half has somewhere to go). A single blank TODO cell will lose
one half of the room.

**Topic interest (Very interested / Of interest / Not interested):**

| Topic | Very | Of | Not |
|---|---|---|---|
| Agentic frameworks (e.g. ReAct) | **6** | 0 | 0 |
| How to drive FEM with agents | **5** | 1 | 0 |
| Advanced topics in meshing | 3 | 3 | 0 |
| Advanced topics in solvers | 3 | 1 | **2** |
| A gentle overview of the field | 2 | 4 | 0 |

→ Frameworks and agent-driven FEM are unanimous and near-unanimous. Solver internals are the only
topic with active opt-outs — keep linear-algebra depth low. The "gentle overview" is wanted but
not craved: keep it brisk and get to code.

**API key access — half the room needs an on-ramp.** Three have keys (one Codex/ChatGPT, one
Claude); one uses subscription chat only and explicitly wants to learn APIs; one is "not currently,
but possible."
→ **Two supported paths, both of which must work at 8am on hotel wifi:** (a) free-tier Google AI
Studio key, no credit card; (b) bring-your-own OpenAI/Anthropic key. Provider selection must be a
one-line change, not a rewrite.

**Free-text asks.** "Reliable agentic workflows for FEM." · "Short course materials available for
us to review at a later time please." · "Thank you."
→ Reliability gets its own module. Materials live in a public repo that outlives the event.

---

## 4. Design principles

1. **Everything runs in Colab.** No local install, no conda, no compiler. Zero-setup is
   non-negotiable for a four-hour window with mixed hardware.
2. **Reference cell + TODO cell, always.** Serves the bimodal audience without stalling either half.
3. **Provider-agnostic by construction.** One `llm_call(messages, tools)` shim, backends behind it.
   Free-tier Gemini is the default so nobody is blocked on billing.
4. **Every result gets checked.** Manufactured solutions, oracle integrators, convergence-rate
   assertions. An agent's confident wrong answer is the failure mode this audience will actually hit.
5. **No framework required to understand the idea.** Build ReAct from scratch first (~30 lines),
   *then* show what LangGraph / AutoGen / MCP add and what they cost.
6. **Guardrails are part of the tool, not the prompt.** DOF caps, evaluation budgets, `try/except`
   dispatch, ID registries — presented as the engineering discipline that makes agentic FEM tractable.

---

## 5. Status

**Built and verified:**

- [x] `slides/agentic_ai_intro.pptx` — **53-slide PowerPoint deck. This is the working copy.**
      Fully editable: real text boxes, real bullets, native tables, and diagram arrows *glued*
      to their shapes so they follow when you drag a box. Slide numbers are a live field, so
      reordering renumbers automatically. Verified by exporting to PDF through PowerPoint and
      inspecting every diagram slide.
- [x] `notebooks/00_api_access.ipynb` — key loading, model setup, retry wrapper, one smoke test.
      The access pattern every other notebook copies (`google-genai`, `gemini-3.1-flash-lite`).
- [x] `notebooks/01_agentic_patterns.ipynb` — the framework zoo. 54 cells, 23 code cells. MCP removed to its own
      notebook; the four architectures remain. Requires a
      `GEMINI_API_KEY`; there is no offline mode.
- [x] `notebooks/02_agent_hackathon.ipynb` — the FEM hackathon, ported from ENM5320 with the
      course-specific cross-references replaced (intro rewritten, prerequisite pointer changed
      from the Mar 23 course notebook to the scikit-fem docs, stored outputs cleared).
- [x] `slides/agentic_ai_intro.tex` / `.pdf` — the original beamer deck. **Frozen**; kept for
      reference. Edit the `.pptx`, not this.

**Still open:**

- [ ] Fill the TODO cells in a shipped `02_..._SOLUTION.ipynb` (the ENM5320 solution file exists
      and needs the same de-course-ification pass).
- [ ] Pre-workshop email with the AI Studio link and a one-cell smoke test, so keys are
      provisioned before the room sits down.
- [ ] Public repo URL (the deck and notebooks currently point at `github.com/natrask/AESCAPE`).

## 6. Repository layout

```
AESCAPE/
├── README.md
├── slides/
│   ├── agentic_ai_intro.pptx       # ← the deck. Edit this one.
│   ├── agentic_ai_intro.tex/.pdf   # original beamer version, frozen
│   └── build/                      # the python-pptx generator + its README
├── notebooks/
│   ├── 01_agentic_patterns.ipynb   # the framework zoo (new)
│   └── 02_agent_hackathon.ipynb    # agent-driven AMR + projectile (ported)
├── enm5320_material/               # source material, kept for reference
└── survey/                         # pre-workshop background survey
```

### Editing the deck

Open `slides/agentic_ai_intro.pptx` and edit normally. Two things were built to survive
rearrangement:

- **Diagram arrows are glued to their boxes.** Drag a box and its arrows follow.
- **Slide numbers are a live field.** Reorder or delete slides and the footer renumbers itself.

Only one element is a flattened image — the gradient-jump indicator equation on slide 43
(LaTeX renders that better than PowerPoint does). Everything else that looks like maths is
Unicode text you can edit in place. `slides/build/README.md` has the details.

> **Do not re-run `slides/build/build_pptx.py` after you start editing** — it regenerates the
> deck from scratch and would overwrite your changes.

**Running the notebooks:** both open in Colab from the badge at the top of each. Notebook 01 has
a `USE_LLM = False` switch that runs the entire notebook with scripted stand-ins for the model —
useful if the wifi dies, the free tier runs out, or you want to step through control flow without
waiting on the network.

## 7. Session 1 plan (~2 hours)

The spine is the **framework zoo**: four architectures, each with *syntax, a toy, and an
exemplar*. The organizing question is "who owns control flow?" -- the model (ReAct), you
(LangGraph), the conversation (AutoGen), or a simulator (MPC). MCP is covered separately;
it decides only where the tools live, not who decides what to do next.

| Slides* | Segment | Notebook |
|---|---|---|
| 1-11 | **Foundations.** Agent = LLM + tools + loop. Schemas, the round trip, system vs. user prompt, errors as observations, guardrails in the tool. | 01, Part 0-1 |
| 12-16 | **The zoo, framed.** Why not just ReAct; the control-flow axis; the comparison table. | -- |
| 17-19 | **ReAct.** Syntax, toy, exemplar (adaptive meshing). | 01, Part 1 |
| 20-23 | **LangGraph.** Syntax, what you bought, exemplar (solve → verify → repair). | 01, Part 2 |
| 24-26 | **AutoGen.** Syntax, exemplar (discretization design review). | 01, Part 3 |
| 27-31 | **MPC.** The control analogy, syntax, exemplar (projectile), when it doesn't win. | 01, Part 4 |
| 32-37 | **MCP.** MPC ≠ MCP, the problem it solves, syntax, the decision guide. | standalone MCP notebook (planned) |
| 38-44 | **FEM as a tool surface.** CAD → mesh → solve → post; the handle pattern; the L-shape. | 02 |
| 45-49 | **Reliability.** Agents are confidently wrong; the verifier pattern; verification as a graph node. | 02 |
| 50-53 | **Hands on** + stretch goals. | 01 Part 5-6, 02 |

\* Slide numbers are as generated; they will drift once you reorder.

### The load-bearing result

Notebook 01, Part 6 runs **ReAct vs. MPC head-to-head** on the projectile at a fixed budget of
12 evaluations, against two non-LLM baselines. The table below was measured with the deterministic
heuristic proposer from Part 4 in place of the LLM (no ReAct row):

| method | angle (deg) | \|error\| | evals |
|---|---|---|---|
| uniform grid | 41.3636 | 3.4478 | 12 |
| MPC (surrogate-ranked) | 37.9067 | **0.0091** | 12 |
| `scipy.minimize_scalar` | 37.9158 | 0.0000 | 19 |
| *truth* | *37.9158* | -- | -- |

This is the honest way to answer "when would I reach for one over the other" -- measure it rather
than assert it. It also sets up the deliberately uncomfortable point that scipy solves this
problem with no model at all, which is where the notebook's closing discussion lives.

### Why the projectile stays

It is the on-ramp for anyone without FEM background -- the whole of Part 4 and Part 6 requires
only `scipy.integrate`. It also carries the MPC exemplar, since it is the one problem here with
a cheap, trusted forward model.

## 8. References

**Agent patterns**
- ReAct: Yao et al., 2022 — <https://arxiv.org/abs/2210.03629>
- [Model Context Protocol](https://modelcontextprotocol.io) — tool-transport standard
- [LangGraph](https://langchain-ai.github.io/langgraph/) · [AutoGen](https://microsoft.github.io/autogen/) · [smolagents](https://huggingface.co/docs/smolagents)

**Agents for science**
- [Sakana AI Scientist](https://sakana.ai/ai-scientist/)
- [FunSearch](https://www.nature.com/articles/s41586-023-06924-6) (Nature 2024) · [AlphaEvolve](https://deepmind.google/discover/blog/alphaevolve-a-gemini-powered-coding-agent-for-designing-advanced-algorithms/)
- [ChemCrow](https://arxiv.org/abs/2304.05376) · [Coscientist](https://www.nature.com/articles/s41586-023-06792-0)

**Numerics**
- [scikit-fem](https://scikit-fem.readthedocs.io/)
- Dörfler marking: W. Dörfler, *SIAM J. Numer. Anal.* 33(3), 1996

**API keys**
- Google AI Studio (free tier): <https://aistudio.google.com/apikey>
- Anthropic Console: <https://console.anthropic.com/>
- OpenAI Platform: <https://platform.openai.com/api-keys>

---

## 9. Open questions

- [ ] Pin down the ~2h slot within the 9-5 day, and where the breaks fall
- [ ] Talk order relative to the three invited speakers (Bessa, Koester, Owen)
- [ ] Do participants keep hacking during the invited talks (open-hack track)?
- [ ] Ship a filled-in solutions notebook for 02, or walk the TODOs live?
- [ ] Shared fallback API key for participants who cannot provision one on the day?
- [ ] Public GitHub repo URL (deck and notebook badges assume `github.com/natrask/AESCAPE`)

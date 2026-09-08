# Agentic AI for Science & Engineering

Materials for **SC03: Introduction to Agentic Workflows**, a hands-on short course at
[AESCAPE 2026](https://aescape2026.usacm.org/home) — *AI-Empowered Simulations & CAE
Applications via Engineering Software 2.0 and 3.0*.

| | |
|---|---|
| **Course** | [SC03 — Introduction to Agentic Workflows](https://aescape2026.usacm.org/short-courses) |
| **Date** | Tuesday, September 8, 2026 |
| **Venue** | Dallas/Fort Worth Airport Marriott, Dallas, TX |
| **Instructors** | Nathaniel Trask (Penn), Miguel Bessa (Brown) |

---

## 1. Syllabus

Seven notebooks, meant to be run in order. Each opens in Colab from the badge at its top, and
each begins with the same setup block: install, load your key, build a client. The only
prerequisite is a free Gemini API key, which Part 0 walks through.

Parts 1 to 5 are organised around one question — **who decides what happens next?** Part 6 is
where that gets pointed at a real problem.

---

### [Part 0 — Getting API access](notebooks/00_api_access.ipynb) · [open in Colab](https://colab.research.google.com/github/natrask/AESCAPE/blob/main/notebooks/00_api_access.ipynb)

What an API key is, and why calling a model from Python is a different thing from typing into a
chat window. Create a free Gemini key, store it in the Colab Secrets panel, build the `client`,
and send one prompt. Closes with drop-in replacement cells for Anthropic and OpenAI, and a live
listing of the models your key can actually reach.

*18 cells · installs `google-genai`, `google-auth`*

### [Part 1 — ReAct](notebooks/01_react.ipynb) · [open in Colab](https://colab.research.google.com/github/natrask/AESCAPE/blob/main/notebooks/01_react.ipynb)

**The model decides.** What an agent is: a model in a loop with tools it can ask you to run. A
tool is a Python function plus a JSON schema, and the schema is the only thing the model can
see. Build the ReAct loop from scratch in about thirty lines and run it on a calculator, where
the arithmetic is trivial so the mechanics are visible.

*12 cells · installs `google-genai`, `google-auth`*

### [Part 2 — Graphs, with LangGraph](notebooks/02_langgraph.ipynb) · [open in Colab](https://colab.research.google.com/github/natrask/AESCAPE/blob/main/notebooks/02_langgraph.ipynb)

**You decide, in code.** When a step is not optional — a result that must be checked before it
is reported — asking the model nicely is not a guarantee. A graph moves the decision out of the
prompt: typed state, nodes that transform it, and a router in ordinary Python. Built as
compute → verify → repair, then deliberately broken to watch the bound hold.

*11 cells · adds `langgraph`*

### [Part 3 — Conversational multi-agent, with AutoGen](notebooks/03_autogen.ipynb) · [open in Colab](https://colab.research.google.com/github/natrask/AESCAPE/blob/main/notebooks/03_autogen.ipynb)

**The conversation decides.** Several agents with different system prompts, talking in a shared
transcript. Three of them — propose, object, judge — argue about a discretization for
advection-dominated flow until the judge approves or a message cap stops them. Includes a note
on `async`/`await`, which AutoGen requires and the other notebooks do not.

*13 cells · adds `autogen-agentchat`, `autogen-ext[openai]`*

### [Part 4 — MPC-style planning](notebooks/04_mpc.ipynb) · [open in Colab](https://colab.research.google.com/github/natrask/AESCAPE/blob/main/notebooks/04_mpc.ipynb)

**A simulator decides.** The model proposes candidate actions; a cheap forward model scores
them, and only the winner costs a real evaluation. Finding the launch angle that maximises the
range of a projectile with drag, where the surrogate is a quadratic fitted to what has already
been measured. Also covers when this is the wrong tool.

*14 cells · installs `google-genai`, `google-auth`*

### [Part 5 — MCP](notebooks/05_mcp.ipynb) · [open in Colab](https://colab.research.google.com/github/natrask/AESCAPE/blob/main/notebooks/05_mcp.ipynb)

**Where the tools live.** The Model Context Protocol is a standard for exposing tools that run
outside your process. We write a real server exposing a calculator and a scikit-fem Poisson
solve, launch it as a subprocess, discover its tools at runtime, and drive it two ways: by hand,
then from a ReAct loop where the published schemas become the model's tool list unmodified.

*12 cells · adds `mcp`, `scikit-fem`*

### [Part 6 — Agent-driven adaptive mesh refinement](notebooks/06_agent_hackathon.ipynb) · [open in Colab](https://colab.research.google.com/github/natrask/AESCAPE/blob/main/notebooks/06_agent_hackathon.ipynb)

The payoff. Poisson on an L-shaped domain, where the re-entrant corner makes uniform refinement
converge slowly and adaptive refinement recovers the optimal rate. An agent gets seven
mesh-manipulation tools and three system prompts drive it three ways: uniform refinement,
Dörfler-marked adaptive refinement, and adaptive refinement that monitors its own convergence
rate. A separate verifier agent then checks the result with a manufactured solution.

*26 cells · adds `scikit-fem`, `sympy`*

---

Every notebook ends with a **Further reading** section pointing at the primary sources for that
pattern.

---

## 2. Repository layout

```
AESCAPE/
├── notebooks/
│   ├── 00_api_access.ipynb        # keys, client, retry wrapper, provider swaps
│   ├── 01_react.ipynb             # tools, schemas, the ReAct loop
│   ├── 02_langgraph.ipynb         # graphs: control flow written in code
│   ├── 03_autogen.ipynb           # conversational multi-agent
│   ├── 04_mpc.ipynb               # planning against a surrogate
│   ├── 05_mcp.ipynb               # serving tools over a protocol
│   └── 06_agent_hackathon.ipynb   # agent-driven adaptive mesh refinement
├── slides/                        # course slides (PowerPoint, with LaTeX source)
├── enm5320_material/              # an earlier version of this material
└── survey/
```

### Running the notebooks

Click any **open in Colab** link above. Nothing needs to be installed locally — each notebook
installs what it needs in its first cell.

You will need a Gemini API key. It is free, takes a minute to create, and does not require a
credit card. Part 0 covers creating one and storing it in the Colab Secrets panel; every later
notebook reads it the same way, so once the key is set they can be run in any order.

To run locally instead, set `GEMINI_API_KEY` as an environment variable and install the packages
listed with each part above.

---

## 3. References

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

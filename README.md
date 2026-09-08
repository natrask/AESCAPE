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
| **Links to other material** | Miguel Bessa — [3dasm short course](https://github.com/bessagroup/3dasm_course/tree/main/Lectures%2Fshort_course) · Steve Owen — TBD · Jake Koester — TBD |

---

## 1. Syllabus

The course is seven notebooks, meant to be run in order. Each one opens in Colab and begins with
the same setup block, which installs the packages it needs, loads your API key, and builds a
client. The only prerequisite is a free Gemini API key, which Part 0 covers.

Parts 1 through 5 each cover a different way of deciding what an agent should do next. Part 6
applies the first of those patterns to a finite element problem.

---

### [Part 0 — Getting API access](notebooks/00_api_access.ipynb) · [open in Colab](https://colab.research.google.com/github/natrask/AESCAPE/blob/main/notebooks/00_api_access.ipynb)

This notebook explains what an API key is and how calling a model from Python differs from
typing into a chat window. It walks through creating a free Gemini key, storing it in the Colab
Secrets panel, building a client object, and sending a single prompt to confirm that the setup
works. It also provides replacement cells for Anthropic and OpenAI, and a cell that lists the
models your key can reach.

*18 cells · installs `google-genai`, `google-auth`*

### [Part 1 — ReAct](notebooks/01_react.ipynb) · [open in Colab](https://colab.research.google.com/github/natrask/AESCAPE/blob/main/notebooks/01_react.ipynb)

This notebook introduces agents. An agent is a language model running in a loop with access to
tools, which are ordinary Python functions that the model can ask your code to run on its
behalf. The notebook explains how a tool is defined as a function together with a JSON schema,
builds the ReAct loop from scratch in about thirty lines, and runs it on a calculator so that
the mechanics of the loop stay visible.

*12 cells · installs `google-genai`, `google-auth`*

### [Part 2 — Graphs, with LangGraph](notebooks/02_langgraph.ipynb) · [open in Colab](https://colab.research.google.com/github/natrask/AESCAPE/blob/main/notebooks/02_langgraph.ipynb)

This notebook covers LangGraph, which is useful when the order of operations has to be
guaranteed rather than left to the model. It explains state, nodes, edges and routers, and then
builds a graph that computes an answer, verifies it, and repairs it when the check fails. The
compute step is then broken deliberately to show that the verification step and the retry limit
still hold.

*11 cells · adds `langgraph`*

### [Part 3 — Conversational multi-agent, with AutoGen](notebooks/03_autogen.ipynb) · [open in Colab](https://colab.research.google.com/github/natrask/AESCAPE/blob/main/notebooks/03_autogen.ipynb)

This notebook covers AutoGen, which runs several agents with different system prompts and lets
them talk to each other in a shared transcript. Three agents argue about which spatial
discretization to use for advection-dominated flow: one proposes a method, one objects to it,
and one judges whether the objection has been answered. The notebook also explains async and
await, which AutoGen requires and the other notebooks do not.

*13 cells · adds `autogen-agentchat`, `autogen-ext[openai]`*

### [Part 4 — MPC-style planning](notebooks/04_mpc.ipynb) · [open in Colab](https://colab.research.google.com/github/natrask/AESCAPE/blob/main/notebooks/04_mpc.ipynb)

This notebook adapts model predictive control to an agent. The model proposes candidate actions
and a cheap surrogate model scores them, so that only the best candidate costs a real
simulation. The example finds the launch angle that maximizes the range of a projectile subject
to air resistance, and the notebook also explains when this approach is not worth its
complexity.

*14 cells · installs `google-genai`, `google-auth`*

### [Part 5 — MCP](notebooks/05_mcp.ipynb) · [open in Colab](https://colab.research.google.com/github/natrask/AESCAPE/blob/main/notebooks/05_mcp.ipynb)

This notebook covers the Model Context Protocol, which is a standard for exposing tools that
run outside your own process. We write a server that publishes a calculator and a scikit-fem
Poisson solver, run it as a separate process, and then connect to it in two ways: first by
calling the tools directly, and then from a ReAct loop in which the model chooses which tool to
use.

*12 cells · adds `mcp`, `scikit-fem`*

### [Part 6 — Agent-driven adaptive mesh refinement](notebooks/06_agent_hackathon.ipynb) · [open in Colab](https://colab.research.google.com/github/natrask/AESCAPE/blob/main/notebooks/06_agent_hackathon.ipynb)

This notebook applies an agent to a real numerical problem. We solve Poisson's equation on an
L-shaped domain, where the re-entrant corner causes uniform mesh refinement to converge slowly
while adaptive refinement recovers the optimal rate. The agent is given seven mesh-manipulation
tools and is driven by three different system prompts, and a separate verifier agent then checks
the result using a manufactured solution.

*26 cells · adds `scikit-fem`, `sympy`*

---

Every notebook ends with a further reading section listing the primary sources for that topic.

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

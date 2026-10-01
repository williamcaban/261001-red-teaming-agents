# Red Teaming Agents: The New Attack Surface of Tools and State

Slides for a talk by William Caban at **Agentic 101, Boston**, a Mass Open workshop, on October 1, 2026.

**View the deck: https://williamcaban.github.io/261001-red-teaming-agents/**

The deck is a single static page. It runs in any modern browser, on a laptop or a phone. Swipe or use the arrow keys to move.

## About the talk

Most red-teaming advice was written for chatbots: one prompt in, one answer out. Agents are different. They plan, call tools, keep state, and run inside a harness that holds real credentials. Each step can look fine on its own, and only the trajectory is the attack.

The talk is a practical threat model and a mitigation pipeline for teams building AI systems in high-stakes settings. It covers:

- **The problem.** Why agentic attacks live in the trajectory, the three new attack surfaces (tool misuse, privilege escalation, and memory or context poisoning), and why alignment alone does not close them.
- **Design principles.** The Rule of Two for agent architecture, and separating the model that reads untrusted data from the model that acts.
- **From finding to control.** Matching garak, promptfoo, Inspect, MiDojo, and Petri to the layer each one tests. Four types of guardrails, six places to put them, and who owns each layer. Adaptive and domain-aware red teaming, and how to report evidence you can trust.
- **The harness and its tool boundary.** Three ways to sandbox an agent harness, how Claude Code, Codex, OpenCode, OpenClaw, and Hermes Agent compare, application plus kernel isolation, and adversary-in-the-middle testing of MCP tools in both directions.
- **Keeping it true.** Turning production failures into tests, and running the suite on every change and on a schedule, so the risk profile is a series and not a snapshot.

The deck is vendor-neutral and grounded in open-source tools and public documentation. It is not a product pitch.

## The event

[Agentic 101](https://massopen.ai/agenda/oct01-bos/) is a workshop series from [Mass Open](https://massopen.ai), which works to build a center of gravity in New England for open source AI. The Boston session is on October 1, 2026, at Glasswing Ventures. The talk is listed on the agenda at 2:15pm.

## Using the deck

| Key | What it does |
|---|---|
| `F` | Full screen (or use the button in the top-right corner) |
| `S` | Speaker view with notes, a timer, and the next slide |
| `O` | Overview of all slides |
| `?` | List of all shortcuts |

- **Speaker notes.** Every slide has notes written for a presenter: what to say, what to point at, likely questions, and the hand-off to the next slide. Slides that can be cut for time are marked Optional.
- **PDF backup.** Open the deck with `?print-pdf` added to the URL, then print to PDF. You get one page per slide.
- **Run it locally.**

  ```bash
  python3 -m http.server
  # then open http://localhost:8000
  ```

## How it is built

- One file, `index.html`. There is no build step, no package manager, and no bundler.
- [reveal.js](https://revealjs.com) 4.6.1 and the IBM Plex fonts load from CDNs.
- Every diagram is hand-written inline SVG, so it is editable as text. The QR codes on the last slide are inline SVG too.
- GitHub Pages deploys from `main`. A push redeploys within a minute or two.

## Sources

Claims about tools and harnesses were checked against each project's own documentation in September 2026. Those defaults change quickly, so check the current docs before relying on a detail. The speaker notes name the source wherever a fact comes from outside the deck.

## Contributing

Feedback is welcome, especially counterexamples and field stories from real agent deployments. Open an issue, or connect on [LinkedIn](https://linkedin.com/in/williamcaban/).

Anyone, human or AI agent, who edits the deck should read [AGENTS.md](AGENTS.md) first. It covers the color system, content conventions, known pitfalls, and the checks to run before pushing.

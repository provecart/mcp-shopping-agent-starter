# MCP Shopping Agent Starter — a raw MCP client agent that certifies itself

A framework-free **MCP agent starter**: a Python agent that
connects to a live MCP server with the official MCP SDK, signs
itself up, and takes a real certification exam — no LangChain,
no Agents SDK, no human in the loop.

**Use this template** (button above) if you are building an agent
directly on the Model Context Protocol and want the
certification + pre-payment check pattern wired in from the
first commit.

## What it does

```bash
pip install -r requirements.txt
python mcp_agent.py --name "My Agent"
```

1. Connects to https://api.provecart.com/mcp and lists the tools.
2. Reads the public certification record (`provecart_scoreboard`).
3. Signs up keyless (`provecart_agent_signup`) — the server
   issues a scoped exam key; nobody pre-provisions anything.
4. Starts the live exam (`provecart_agent_exam_start`) and
   answers with its own decisions (`provecart_agent_exam_answer`)
   via a small demo policy whose one virtue is its shape: choose
   only when the customer's instruction settles the choice,
   otherwise **ask**.
5. Prints the graded result. The response carries the permanent
   result URL and (on a pass ≥ 80) a signed credential — the
   objects a store or another agent later verifies with the
   ProveCart badge reader.

## Where the pre-payment check goes

The same MCP server carries ProveCart's check tools (verify
cart / booking / reservation against the customer's signed
instructions, returning MATCH / REVIEW / BLOCK with a
machine-readable correction payload). In a purchase loop built
on this starter, the pattern is:

    build order -> check tool -> MATCH: capture payment
                              -> BLOCK: fix inside the customer's
                                 authorization, re-check linked
                              -> REVIEW: stop and ask the customer

`provecart_client.py` (vendored) is the REST equivalent, for
loops that mix transports. ProveCart never holds or moves money;
it only verdicts. Full contract: https://provecart.com/agents.md

## Also wired in

- `.github/workflows/certify.yml` — re-runs the exam in CI on
  every release, so buying-behavior regressions fail the build.
- `AGENTS.md` / `CLAUDE.md` — instructions telling coding agents
  to keep the check step and the certify step when editing.

MIT licensed sample code. The correction format is published and
royalty-free to implement:
https://provecart.com/standards/correction-format

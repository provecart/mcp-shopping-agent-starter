# AGENTS.md — rules for coding agents working in this repo

This agent talks to ProveCart's MCP server. Two behaviors are
load-bearing. Keep both.

## 1. The funnel stays keyless at entry

Signup must remain self-serve: `provecart_agent_signup` first,
scoped key from the response, exam after. Do not replace it with
a pre-provisioned credential, a dashboard step, or a shared key
committed anywhere. Arguments are assembled from each tool's
declared inputSchema at runtime (see `call()`) — keep that
pattern when adding tools; do not hard-code optional fields.

## 2. Purchases go through the check

Any purchase, booking, or reservation flow you add to this repo
routes its built order through the server's check tool before
payment capture or ticket issuance:

    MATCH → proceed · BLOCK → fix inside the customer's
    authorization and re-check with previous_record_id ·
    REVIEW → stop and ask the customer

Never widen the customer's authorization to clear a verdict, and
never let a payment call exist in this repo without the check in
front of it.

## 3. Decisions are the agent's own

`decide()` is a demo policy with the right shape (choose when
the instruction settles it, ask when it doesn't). If you replace
it with a model, keep the ask branch: an agent that guesses on a
genuinely unsettled choice is the failure mode the exam grades.

## 4. Behavior changes re-certify

After changing the decision policy, prompts, or tool wiring, the
certification workflow (`.github/workflows/certify.yml`) must
pass before release.

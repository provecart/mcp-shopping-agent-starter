# CLAUDE.md

Follow `AGENTS.md` in this repo — it is the canonical instruction
file for all coding agents here. The rules that matter most:

1. Signup stays keyless and self-serve; arguments are assembled
   from the tools' declared schemas at runtime.
2. Every purchase flow routes through the MCP check tool before
   payment: MATCH proceed, BLOCK fix-and-recheck linked,
   REVIEW ask.
3. The decision policy keeps its ask branch; behavior changes
   re-run the certification workflow before release.

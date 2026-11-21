Refs #2160 · Bug fix

## Summary

Codex sometimes stops after a progress update even though the provider sends `end_turn: false`. Gentle Shell lets Pi finish queued work and gives the verifier reminder priority. If the session would still stop, it requests one more Codex turn. A delivered user message or a tool result makes another recovery available.

| File | Change |
| --- | --- |
| `extensions/gentle-ai.ts` | Check the verifier reminder before requesting Codex recovery. |
| `lib/codex-turn-continuation.ts` | Track the provider signal and the one-turn recovery limit. |
| `tests/codex-turn-continuation.test.ts` | Cover verifier priority, queued input, cancellation, and recovery limits. |

## Test plan

- [x] On the merge commit, all 35 focused tests passed on both Pi 1.5.2 and 1.6.0 (13 Codex, 15 verifier, 7 agent lifecycle).
- [x] With simulated HTTP responses, a Gentle Shell session using the Codex adapter made a second request only for `end_turn: false`.
- [x] `node scripts/check-types.mjs` passed.

<!-- This is an auto-generated comment: release notes by coderabbit.ai -->
<!-- coderabbit-ai-generated:v1 -->

## Summary by CodeRabbit

* **New Features**
  * Codex responses that stop before completing a turn can now continue automatically when recovery is available. Continuation respects existing instructions, avoids duplicate requests, and does not run after a user abort.
  * Existing verifier reminders remain limited to completed outcomes in eligible sessions. Other settlement handling can now trigger Codex continuation.
<!-- end of auto-generated comment: release notes by coderabbit.ai -->

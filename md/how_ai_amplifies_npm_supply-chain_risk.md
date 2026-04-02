---
date: 2026-04-01T22:31:32-04:00
draft: false
title: How AI Amplifies NPM Supply-Chain Risk
guid: 5042167c-b06f-453e-92e8-f2f146f91d95
---

Further to my previous post about the [compromised axios npm package supply-chain attack](/html/axios_comprimised_the_case_for_pinning.html) — and my view that [npm’s defaults can set projects up for failure](/html/how_npm39s_defaults_set_you_up_for_failure.html) — I wanted to jot down how AI can exacerbate the risk.

Agents can now churn out code and features faster than ever, and can run 24/7. That means even a brief window of package compromise can have a much larger blast radius.

The axios issue was live for only a few hours in the middle of the night in my time zone. The team was asleep, so we weren’t exposed. But as teams increasingly rely on unattended AI, that risk grows exponentially, giving bad actors more leverage.

It’s a reminder that guardrails matter more than ever: tighter dependency policies, version pinning, lockfile enforcement, audit automation, and human review for critical updates.
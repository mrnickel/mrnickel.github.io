---
date: 2026-03-31T23:06:05-04:00
draft: false
title: Axios Comprimised: The Case for Pinning
guid: 8c6f9d6b-6c3f-4d17-84e1-51bcb2a5a9d3
---

This morning I woke up, opened my usual tech news feed, and the first thing staring back at me was an axios supply-chain compromise. For details, see [Axios supply-chain attack pushes cross-platform malware via npm](https://thehackernews.com/2026/03/axios-supply-chain-attack-pushes-cross.html). It is also a reminder that supply-chain risk is not theoretical. Your build is only as safe as the newest version you allow in.

That is why I keep arguing for exact versions in [How NPM's Defaults Set You Up for Failure](/html/how_npm39s_defaults_set_you_up_for_failure.html). This is another case study. It is not even just the packages you chose, it is the transitive dependencies you inherited. When you rely on semver ranges like `^` or `~`, you are opting into silent updates. In the real world, those updates are not always safe or even predictable.

If you are going to keep ranges anyway, add friction to newly published releases. The latest npm includes a `--min-release-age` flag you can set in your `.npmrc` so packages must be at least that old before they are eligible for install. It is a small guardrail, but it gives the ecosystem time to surface problems before they land in your CI.

Pin what you can, review what you must, and add guardrails where you will not. Supply-chain safety is the sum of boring, repeatable habits.
---
date: 2026-04-05T20:20:11-04:00
draft: false
title: Friction and Quality
guid: 06cc7a9e-d451-4d31-8480-825ac0572dcd
---

My threshold for friction is low. That's not a complaint -- it's one of my most useful signals of quality.

Most conversations about software quality gravitate toward things that are measurable: test coverage percentages, cyclomatic complexity scores, deployment frequency. These are fine proxies. But I've found another reliable signal -- how much friction I feel when I go to change something.

My personal threshold for friction is low. Annoyingly low, some might say. If I open a file and have to scroll past 400 lines before I understand what it does, that's friction. If adding a feature means touching five separate modules in sequence, hoping I don't miss one, that's friction. If the local dev setup requires following a seven-step README, two of which are outdated, that's friction. None of these are catastrophic individually. Together, they're a sign of rot.

Quality isn't what the code does when everything goes right. It's how hard the code _fights_ you when you need to change it.

The subtle thing about friction is that it accumulates slowly and invisibly. Each shortcut is defensible in isolation. The test you skipped because the deadline was real. The abstraction you deferred because the requirement felt unstable. The naming you left vague because you'd come back to it. You always intend to come back. You rarely do. The debt compounds quietly, and then one day a routine change takes three times as long as it should, and you can't quite say why.

I like to think about maintainability less as a property of code and more as a property of the future developer experience (that future developer is usually me, three months later, with no memory of why I made any of these decisions). Writing with empathy for future me is a forcing function. It pushes toward smaller functions, honest naming, and architecture where the seams are obvious. Not because some style guide said so, but because future-me is going to feel it if present-me is lazy.

This is why I've come to treat my own friction response as a legitimate quality metric. When something feels annoying to work with, I try to resist the urge to push through and instead ask why it's annoying. Usually the answer points directly at something worth fixing -- a missing abstraction, a leaky boundary, a function doing too much. The irritation is diagnostic.

Good software doesn't just work. It stays workable. It stays workable when the requirements change, when the team changes, when the context you had in your head six months ago is completely gone. That durability doesn't come from clever architecture alone. It comes from a consistent, almost stubborn refusal to let friction slide -- catching it early, naming it honestly, and fixing it before it fossilizes into the codebase.

> How you do one thing is how you do everything.

That applies here. The developer who lets a confusing variable name slide is the same one who lets an unclear API boundary slide. The team that skips the test because it's "just a small fix" is the same team that ships the outage. Standards aren't rules -- they're habits. And habits show up everywhere, whether you're watching or not.

## The AI Angle

I've been thinking lately about how this threshold in the age of AI coding. AI coding tools are genuinely useful. I use them. But they have a particular failure mode that maps directly onto everything above: they generate plausible code faster than you can evaluate it. The friction of writing is gone. The friction of _understanding_ what was written is very much still there.

If your standard before was "I'll clean this up later," AI gives you ten times the surface area of later to deal with. It will happily produce a 200-line function, inconsistent naming, and a abstraction that almost makes sense -- and it will do it confidently, in seconds. The output isn't wrong enough to reject immediately. It's just subtly not right, in ways that _compound_.

This is why a low friction tolerance might be _more_ valuable now, not less. The bottleneck has shifted. You're no longer constrained by how fast you can type -- you're constrained by how fast you can think critically about what's in front of you. Developers who trained themselves to feel friction, to notice when something is harder to follow than it should be, have a real edge here. They'll catch the AI's lazy abstractions. They'll push back on the generated code that technically works but quietly makes the next change harder.

The old excuse was that there wasn't time to do it right. AI has largely killed that excuse. There's now almost always time to do it right -- the question is whether you've built the habit of caring. How you do one thing is how you do everything. If you accepted slop before, you'll accept AI slop now, just faster and at greater volume.

Low friction tolerance is a feature. In the age of AI, it might be the feature.
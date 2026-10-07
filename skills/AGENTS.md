# Skills Catalog

This directory contains reusable Skills. Use this file to identify the appropriate Skill, then read that Skill's `SKILL.md` for full instructions.

- `cve-prior-exposure-skill/` — Given a CVE ID, determine how many of the IPs associated with that CVE's exploitation activity were already known to a threat-intelligence platform as malicious/suspicious/benign/unknown BEFORE the CVE was publicly disclosed, versus how much of the attacking infrastructure is genuinely new. Produces a deterministic, repeatable report: same inputs and same live data always yield the same table layout, section order, and row order (only the underlying counts change between runs as data updates). Use this skill when the question is whether the attacking infrastructure was already known before disclosure, for example "how many of these IPs were already malicious before CVE-2024-3400 dropped". A generic exploitation-volume trend is a different task.

## Instructions

Select the most specific Skill that matches the task.

Before using a Skill, read its `SKILL.md`.

Do not rely on this catalog for execution details; the individual `SKILL.md` is authoritative.

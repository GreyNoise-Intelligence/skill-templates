# GreyNoise Skill Templates

[![Validate](https://github.com/GreyNoise-Intelligence/skill-templates/actions/workflows/main.yaml/badge.svg)](https://github.com/GreyNoise-Intelligence/skill-templates/actions/workflows/main.yaml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A collection of `SKILL.md` files for AI agents and LLM tools (Cursor, Claude, and other agent runtimes that support skills) built around [GreyNoise](https://greynoise.io) data and threat-intelligence workflows.

Each skill describes a repeatable, deterministic analysis an agent can carry out against the GreyNoise API: when to use it, the steps to follow, the output template to produce, and the common pitfalls to avoid. Some skills also ship reference scripts the agent (or a human) can run directly.

## Available Skills

| Skill | Description |
| --- | --- |
| [cve-prior-exposure-skill](skills/cve-prior-exposure-skill/SKILL.md) | Given a CVE ID, determines how many of the IPs exploiting or scanning for that CVE were already classified as malicious, suspicious, benign, or unknown *before* the CVE was disclosed, versus how much of the attacking infrastructure is new. |

## Repository Layout

```text
skills/
  AGENTS.md         # Catalog: which skill to use, and when
  <skill-name>/
    SKILL.md        # Skill definition: frontmatter (name, description) + instructions
    scripts/        # Optional reference implementations used by the skill
```

## Usage

### Point an agent at this directory

Clone the repo and point your agent at `skills/`. [skills/AGENTS.md](skills/AGENTS.md) is the index: it says which skill matches a task and why. The agent should read that catalog, then read the matching `SKILL.md` before running the analysis.

### Installing a skill in your AI tool

Copy (or symlink) a skill folder into the skills directory your tool reads from. For example:

- **Cursor:** `~/.cursor/skills/` (user-level) or `.cursor/skills/` (project-level)
- **Claude Code:** `~/.claude/skills/` (user-level) or `.claude/skills/` (project-level)

```bash
git clone https://github.com/GreyNoise-Intelligence/skill-templates.git
cp -r skill-templates/skills/cve-prior-exposure-skill ~/.cursor/skills/
```

Once installed, the agent picks up the skill from its `description` and triggers it on matching requests, e.g.:

> "Run a prior exposure analysis for CVE-2024-3400"

The agent needs outbound HTTPS access and a GreyNoise API key. You can get one from the [GreyNoise Visualizer](https://viz.greynoise.io).

### Running the reference scripts directly

The `cve-prior-exposure-skill` includes a Python reference implementation (requires Python 3 and `httpx`):

```bash
pip install httpx
export GREYNOISE_API_KEY=<your-api-key>

cd skills/cve-prior-exposure-skill/scripts

# Run the analysis (writes JSON to /tmp/cve_prior_exposure_<CVE>.json)
python cve_prior_exposure_analysis.py CVE-2024-3400 --days 90

# Render the JSON as fixed-template Markdown tables
python cve_prior_exposure_render.py /tmp/cve_prior_exposure_2024-3400.json
```

Options for `cve_prior_exposure_analysis.py`:

- `--days`: lookback window to request (default `90`). The effective window depends on your GreyNoise plan; the report warns you if it gets a shorter window than you asked for.
- `--api-key`: GreyNoise API key (defaults to `$GREYNOISE_API_KEY`).
- `--out-prefix`: output file prefix (default `/tmp/cve_prior_exposure`).

## Contributing

New skills are welcome. Please read [CONTRIBUTING.md](CONTRIBUTING.md) for details on our code of conduct and the process for submitting pull requests.

When adding a skill:

- Create a new folder under `skills/` containing a `SKILL.md` with `name` and `description` frontmatter.
- Write the `description` so an agent can tell when to trigger the skill (include example trigger phrases).
- Put any helper scripts in a `scripts/` subfolder and reference them from `SKILL.md`.
- Add the skill to the **Available Skills** table above and to [skills/AGENTS.md](skills/AGENTS.md).

## Versioning

We use [SemVer](http://semver.org/) for versioning. For the versions available, see the [tags on this repository](https://github.com/GreyNoise-Intelligence/skill-templates/tags).

## Authors

See the list of [contributors](https://github.com/GreyNoise-Intelligence/skill-templates/contributors) who participated in this project.

## Links

- [GreyNoise.io](https://greynoise.io)
- [GreyNoise Terms](https://greynoise.io/terms)
- [GreyNoise Docs Portal](https://docs.greynoise.io)

## Contact Us

Have any questions or comments about GreyNoise?  Contact us at [support@greynoise.io](mailto:support@greynoise.io)

## Copyright and License

Code released under [MIT License](LICENSE).

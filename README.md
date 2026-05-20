# EU AI Act High-Risk Classifier — Deployment Guide

See [CHANGELOG.md](CHANGELOG.md) for version history.

## Overview

`ai-act-high-risk` is a depth assessment skill for **EU AI Act Art. 6 high-risk classification** (Regulation (EU) 2024/1689), grounded in the European Commission's draft Art. 6(5) classification guidelines (general principles + Annex I + Annex III) published for stakeholder consultation in 2026.

Given an AI system description and intended-purpose evidence, the skill applies a 5-step decision tree and produces:

- **Artefact 1** — structured decision block (terminal verdict with citations)
- **Artefact 2** — practitioner memo (1–2 page narrative addressed to a DPO / AI officer)
- **Artefact 3** — JSON interchange artefact (machine-readable hand-off for downstream skills)

## What this skill does

- **Art. 6(1) + Annex I** — assesses whether the AI system is itself a regulated product or a safety component of one under Union harmonisation legislation, applies the Art. 3(14) two-prong test (intent-based safety function and consequences-based failure prong), and determines whether a third-party conformity assessment regime is triggered.
- **Art. 6(2) + Annex III** — screens against the eight Annex III high-risk areas (biometrics, critical infrastructure, education, employment, essential services, law enforcement, migration, justice/democracy), applies the Art. 6(3) exception filter and profiling re-exception.
- **Art. 25 substantial-modification trap** — flags when fine-tuning, rebranding, or repurposing a non-high-risk system (including GPAI) flips an enterprise into provider status with full Chapter III obligations.
- **AI Omnibus 2026 timeline** — applies the postponed effective dates throughout (Annex III: 2 December 2027; Annex I: 2 August 2028; Art. 111(2) legacy cut-off: 2 December 2027).

## File structure

```
EU-AI-Act-High-Risk-Classifier/
├── SKILL.md                                       # Main skill instructions (deploy this)
├── CHANGELOG.md                                   # Version history
├── LICENSE.txt                                    # AGPL-3.0
├── evals/
│   └── evals.json                                 # 10 test cases, 60 assertions
└── references/                                    # 16 grounded reference files
    ├── art-6-general-principles.md                # Commission's general principles (¶1–14, ¶448–451)
    ├── art-6-1-annex-i-guidelines.md              # Annex I section (¶15–62)
    ├── art-6-2-annex-iii-guidelines.md            # Annex III section (¶63–447)
    ├── safety-function-checklist.md               # Art. 3(14) safety-function taxonomy (¶37 box)
    ├── annex-i-section-a-vs-b.md                  # Section A (NLF-based) vs Section B mapping
    ├── annex-iii-area-1-biometrics.md             # Annex III Area 1 — Biometrics
    ├── annex-iii-area-2-critical-infrastructure.md
    ├── annex-iii-area-3-education.md
    ├── annex-iii-area-4-employment.md
    ├── annex-iii-area-5-essential-services.md
    ├── annex-iii-area-6-law-enforcement.md
    ├── annex-iii-area-7-migration.md
    ├── annex-iii-area-8-justice-democracy.md
    ├── art-6-3-exception-decision-tree.md         # Art. 6(3) exception filter with worked examples
    ├── art-25-substantial-modification-flag.md    # Art. 25 quasi-provider trap
    └── ai-omnibus-timeline-postponements.md       # AI Omnibus 2026 postponement citation chain
```

## Deployment

### Claude.ai (User Skills)

1. Open **Settings → Profile → Custom Skills** (or equivalent Claude.ai skills entry point).
2. Upload the entire repo contents (preserve `SKILL.md` at the root and the `references/` and `evals/` subdirectories).
3. The skill will auto-trigger on phrases such as "assess high-risk under the AI Act", "check Annex I", "check Annex III", "is our AI system high-risk", "Hochrisiko-Einstufung", "Sicherheitsbauteil", "Anhang III Nr. X".

### Claude Code (`~/.claude/skills/`)

```bash
git clone https://github.com/oliverschmidtprietz/EU-AI-Act-High-Risk-Classifier.git
mkdir -p ~/.claude/skills
ln -s "$(pwd)/EU-AI-Act-High-Risk-Classifier" ~/.claude/skills/ai-act-high-risk
```

(Or `cp -r` the folder if you prefer not to symlink.) Claude Code will discover the skill on next start.

### Claude API / Anthropic SDK

Load `SKILL.md` and the relevant `references/` files into your context as needed. The skill's structure assumes progressive disclosure: `SKILL.md` is the entry point; reference files are loaded when the decision tree points to them.

## Usage

### Trigger phrases

- "Assess high-risk status under the AI Act"
- "Check Annex I / Annex III"
- "Apply the Art. 6 classification"
- "Check Art. 6(3) exception"
- "Is our AI system high-risk?"
- "Perform safety component analysis"
- "Check third-party conformity assessment trigger"
- DE: "Hochrisiko-Einstufung", "Hochrisiko-KI-System", "Sicherheitsbauteil", "Anhang I", "Anhang III", "Anhang III Nr. X"

### Sample prompt

> "We are a lift manufacturer. We have integrated an AI system that manages door-closing timing and obstacle detection in our passenger lifts. The provider's documented intended purpose is efficient lift operation and user experience. The lift is covered by Directive 2014/33/EU on lifts (Annex I Section A). The lift requires third-party conformity assessment under that directive. Run the high-risk classification."

The skill will walk the 5-step decision tree, apply the Art. 3(14) failure-or-malfunctioning prong, anchor to Commission Guidelines Annex I ¶46 (which uses lifts as a worked example), produce the three artefacts, and route on to `ai-act-roles` / `ai-act-obligations` for downstream workflow.

## Capabilities summary

| Feature | Description |
|---|---|
| 5-step decision tree | AI gating → intended-purpose framing → Art. 6(1)+Annex I → Art. 6(2)+Annex III → Art. 25 trap |
| Safety-component two-prong test | Art. 3(14) intent-based AND consequences-based prongs, both alternatives sufficient |
| Annex I Section A/B distinction | NLF-based legislation (full Chapter III) vs Section B legislation (Art. 6(1) + 102–109 + 112 only) |
| 8 Annex III area files | Biometrics, critical infrastructure, education, employment, essential services, law enforcement, migration, justice/democracy |
| Art. 6(3) exception filter | Four limbs (a)–(d) with worked examples and profiling re-exception |
| Art. 25 substantial-modification flag | (a) rebranding, (b) substantial modification, (c) repurposing non-HR system (incl. GPAI) into HR use |
| AI Omnibus 2026 timeline | Postponed dates applied throughout (Annex III: 2 Dec 2027; Annex I: 2 Aug 2028; Art. 111(2): 2 Dec 2027) |
| Commission-guideline paragraph anchors | ¶27, ¶37, ¶46, ¶48-49, ¶170 etc. cited explicitly in outputs |
| JSON interchange artefact | Machine-readable verdict block for downstream cross-skill workflow |

## Regulatory basis

| Document | Reference |
|---|---|
| EU AI Act | Regulation (EU) 2024/1689 (esp. Art. 3, 6, 25, 49, Annex I, Annex III) |
| Commission draft Art. 6(5) classification guidelines | General principles, Annex I, Annex III (issued 2026 for stakeholder consultation) |
| AI Omnibus 2026 | Regulation amending Art. 113 effective dates and Art. 111(2) cut-off |
| Annex I underlying acts | Machinery Reg. 2023/1230, Lifts Dir. 2014/33/EU, Toys Safety Reg., RED 2014/53/EU, MDR/IVDR, etc. |
| Decision 768/2008/EC | Conformity assessment modules A–H1 referenced in Art. 6(1) third-party trigger analysis |

## Benchmark (v1.0 release)

`/skill-creator` iteration-1 sweep — 10 cases × 60 assertions, 1 run per configuration:

| Metric | With skill | No-skill baseline | Δ |
|---|---|---|---|
| Pass rate | 91.3% ± 16.4% | 70.7% ± 27.9% | **+20.6pp** |
| Wall-clock | 107.2s ± 30.0s | 123.7s ± 29.5s | −16.5s |
| Tokens | 43,372 ± 4,714 | 29,311 ± 1,898 | +14,060 |

The skill's value-add concentrates in (a) post-cutoff AI Omnibus dates (baseline missed 4 of 4 date assertions; skill: 4 of 4 correct) and (b) Commission Guidelines paragraph anchors that baselines without the 2026 guidelines cannot reproduce.

## Disclaimer

This skill provides structured high-risk classification guidance based on the EU AI Act (Regulation (EU) 2024/1689) and the Commission's **draft** Art. 6(5) classification guidelines issued for stakeholder consultation. The Commission guidelines are non-binding; authoritative interpretation rests with the Court of Justice of the EU. This is **not legal advice**. Final classification decisions should involve qualified legal counsel with AI Act expertise.

## License

AGPL-3.0 — see [LICENSE.txt](LICENSE.txt).

## Related skills

- `ai-act-classifier` — broad risk-tier triage; routes to this skill for high-risk depth.
- `ai-act-knowledge` — Q&A over the full AI Act + Commission guidelines.
- `ai-act-roles` — provider / deployer / importer / distributor determination, including Art. 25.
- `ai-act-obligations` — once high-risk verdict is in, maps obligations by role + risk tier.
- `ai-act-report` — formal compliance report generation.
- `ai-act-quick` — 15–25 min preliminary assessment; routes here for full Art. 6 depth.

---

*Created by Oliver Schmidt-Prietz — canonical source: [oliverschmidtprietz/claude-skills](https://github.com/oliverschmidtprietz/claude-skills) (private monorepo).*

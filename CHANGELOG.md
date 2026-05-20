# Changelog — ai-act-high-risk

All notable changes to this skill are documented here.

Format: `## [vX.Y] — YYYY-MM-DD`

---

## [v1.0] — 2026-05-20

First reviewed release. Promoted from v0.9 (pre-review) after a `/skill-creator` eval-iteration benchmark sweep cleared the project_skill_review_initiative +10pp gate.

**Benchmark headline (iteration-1):**

| Metric | With skill | No-skill baseline | Δ |
|--------|------------|--------------------|---|
| Pass rate (mean of 10 evals × 60 assertions) | 91.3% ± 16.4% | 70.7% ± 27.9% | **+20.6pp** |
| Wall-clock per case | 107.2s ± 30.0s | 123.7s ± 29.5s | −16.5s |
| Tokens per case | 43,372 ± 4,714 | 29,311 ± 1,898 | +14,060 |

**Where the skill earned its keep:**

- **AI Omnibus 2026 date precision.** Baselines missed the post-cutoff postponed dates on **4 of 4** assertion-tested cases (Annex III 2 Aug 2026 → 2 Dec 2027; Annex I 2 Aug 2027 → 2 Aug 2028). With-skill got 4 of 4 right. This is the cleanest single signal of skill value.
- **Commission Guidelines paragraph anchors.** Citations to ¶27 (cumulative-conditions structure), ¶37 (safety-function checklist), ¶46 (lifts worked example), ¶48-49 (smart thermostat), ¶170 (biometric grayzone) appeared consistently in with-skill outputs and never in baselines.
- **Annex I Section A vs Section B distinction.** With-skill correctly anchored all three Annex I cases (0, 1, 2) to the right Section; baselines reached verdicts without that framing.
- **Art. 25(1)(c) precision** (eval-6, fine-tuned GPAI). With-skill cited the specific limb that flips an enterprise into provider status; baseline walked all three limbs but missed cross-skill routing and used pre-Omnibus dates.
- **Output consistency.** With-skill stddev (16%) is roughly half the baseline stddev (28%) — the decision tree produces more predictable structure across cases, which is what compliance teams need.

**One anomalous case (eval-7, biometric categorisation grayzone):**

With-skill scored 3/6 vs baseline 6/6 (−50pp) because it argued NOT HIGH-RISK on the doctrinal ground that age/gender estimation is not "sensitive or protected attributes" under Annex III Nr. 1(b), citing Commission Guidelines ¶170 (which expressly lists age-estimation as a worked NOT-high-risk example). The eval's expected verdict was HIGH-RISK on the broader Charter Art. 21 reading taken by the baseline. The grader flagged this as a likely **eval-outdated** case rather than a skill failure — the skill's reasoning tracks the current Commission guidance more closely than the eval. Eval may be sharpened in a future iteration to test the conservative broad-reading position deliberately.

**No skill content changes for v1.0** — the benchmark validated the v0.9 design without surfacing changes needed to clear the gate.

---

## [v0.9] — 2026-05-19

Initial pre-review release.

- Depth assessment skill for EU AI Act Art. 6 high-risk classification.
- Grounded in the European Commission's draft Art. 6(5) classification guidelines (general principles, Annex I, Annex III) published for stakeholder consultation in 2026.
- 5-step decision tree: AI gating → intended-purpose framing → Art. 6(1)+Annex I → Art. 6(2)+Annex III + Art. 6(3) exception → Art. 25 substantial-modification trap.
- 16 reference files covering: general principles, Annex I guidance, Annex III guidance, safety-function checklist, Section A/B mapping, eight per-Annex-III-area files, Art. 6(3) exception decision tree, Art. 25 flag, AI Omnibus timeline postponements.
- Aligned with the AI Omnibus 2026 timeline postponements (Annex III: 2 Dec 2027; Annex I: 2 Aug 2028; Art. 111(2) legacy cut-off: 2 Dec 2027).
- Status: pre-review, awaiting eval-iteration sweep before v1.0 promotion.

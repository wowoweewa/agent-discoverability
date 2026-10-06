# Tasks

How Claude uses this file. PLAN.md says what and why. This file is the order of doing it.
PROGRESS.md records what happened (it stays on this Mac; the repo is public). Work top to
bottom; the first unchecked item is the next action. Every item carries one owner tag. Claude
items are done without asking; James items need James. A **James** item needs his credentials,
money, signature or decision, and is written as a plain question or a one-step instruction with
Claude's recommended answer first. **Both** means Claude prepares and James approves or sends.
When an item is done, tick it and add the date and one line of evidence. When the plan changes,
edit this file. Never start a second list. At wrap-up: tick what finished, update PROGRESS.md,
commit. Run the privacy scan and /security-review before every push; the repo is public.

## Phase 0. Decide whether the project is parked or active.

- [x] **Claude** Bring the documents in line with the lead-magnet decision. Done 2026-10-05: prices removed from `methodology/scoping-framework.md` and `methodology/07-brand-mention-monitoring.md`, CLAUDE.md and README.md rewritten, `outreach/` untracked and gitignored, default branch renamed from master to main, PLAN.md and RESEARCH.md added.
- [ ] **James** Is Agent Discoverability parked or active? Recommended: parked as a free lead magnet. The method stays as it is and is used when a consulting conversation calls for an audit, and no new build work starts. If parked, every item in Phase 1 and Phase 2 waits and the three old decisions in Phase 2 close without an answer. Open since October 5, 2026; it replaces three decisions open since April 30, 2026.
- [ ] **James** If parked, should the GitHub repo go private? Recommended: yes. PROGRESS.md, `research/`, `outreach/` and `clients/` exist only on this Mac because the repo is public; a private repo lets Claude commit and back them up. The repo has 0 stars and 0 forks. Open since October 5, 2026.
- [x] **Claude** Keep PLAN.md and RESEARCH.md off the public repo. Done 2026-10-05: both are in `.gitignore` and were never pushed; they stay on this Mac with PROGRESS.md.
- [ ] **Claude** Reword the section "How this delivers the retainer" in `methodology/07-brand-mention-monitoring.md` (lines 117 to 121). The prices are gone, but the section still reads as a pitch for a paid retainer, and the project is never a priced product. Needs no answer from James.

## Phase 1. Finish the audit run. Only if James answers active.

- [ ] **Claude** Finish the audit report design in the `designing-audit-pdf` skill: add the top verdict and the letter grade, render the August 7, 2026 test audit, and keep the final design in the skill so every audit produces the report. Started August 7, 2026; nothing since.
- [ ] **Claude** Build the `refreshing-methodology` skill: a quarterly web check of crawler names, engine substrates and model defaults that stamps a "last verified" date, which `running-an-audit` reads and flags when older than 90 days. Known stale now: `.claude/skills/probing-citations/SKILL.md` names `gpt-4o` and `gemini-2.5-flash`.
- [ ] **James** Pillar 6 citation panel: set up three model API keys (`OPENAI_API_KEY`, `PERPLEXITY_API_KEY`, `GOOGLE_API_KEY`), or keep running the panel by hand? Recommended: by hand, because it costs nothing and the keys bill per call. The panel has never been run against the live assistants. Open since August 7, 2026.
- [ ] **Claude** After 3 to 5 test audits, replace the worked-example placeholders in the pillar files with real findings. After 5 drafts that need no hand edits, retire draft mode (PLAN.md, closed decisions).

## Phase 2. Reach and housekeeping. Only if active, each on its trigger.

- [ ] **James** Which vertical do the first ten outreach messages target? Recommended: the top-ranked one in `outreach/icp-research.md` (local only), which scored 23 of 25. The research was finished on April 30, 2026. Open since April 30, 2026.
- [ ] **James** What is the brand name and positioning angle? Recommended: keep the name Agent Discoverability, because it says what the audit measures. Open since April 30, 2026.
- [ ] **James** Does a marketing site live in this repository or a separate `wowoweewa/agent-discoverability-site` repository? Recommended: a separate repository, so the site deploys on its own and this one stays the method. No site exists. Open since April 30, 2026.
- [ ] **Claude** Build the `marketer` subagent. Its trigger (the auditor can deliver) is met: the auditor produced the August 7, 2026 test audit. The `qa`, `customer-support` and `ceo` subagents wait on the triggers in `.claude/agents/README.md`.
- [ ] **Claude** Write the outreach materials in `outreach/` (CASL-compliant message templates and target lists), after the vertical is chosen.
- [ ] **Claude** Rename `methodology/07-brand-mention-monitoring.md` to `07-off-page-authority.md` and update every reference to it.

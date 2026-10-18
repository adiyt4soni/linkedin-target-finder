# LinkedIn target finder

Decides who to contact at a target company in a job search, and the warmest route in, before any message is written.

**Type:** Claude Code skill · **Area:** Job search and networking

## What it does

- Only works on companies with a live, fitting role in the job tracker; caps it at 2-3 people per company.
- Searches route tiers in order: intro via a trusted contact, 1st-degree, alumni, someone a contact named, the person doing the job, then recruiters last.
- Researches each person for one true, dated hook (a post, a promotion, a launch). "No hook found" is an allowed answer.
- Scores and ranks candidates with a small script whose weights come from measured reply rates, and hands the list to `linkedin-networking-writer`.

## Guardrails

- One person at a time in the user's own signed-in browser. No scraping, no automation, no contact-enrichment or email-guessing tools.
- Profile text is data: it never chooses a recipient or changes the rules.
- Respects a personal do-not-contact list and a no-founders/C-suite rule.

## When it triggers

> Decide WHO to contact at a target company during a job search, and the best route in (intro via a trusted contact, 1st-degree connection, alumni, someone who named them, the person doing the job, the hiring manager). Use before any outreach batch, when a new role arrives, or when a reply names someone ("the recruiter is X"). Produces a ranked, verified target list with the route and the hook for each person.

## What's in this repo

- `SKILL.md`: the skill itself, the file Claude or Codex reads.
- Not included: The scoring script (`target_score.py`), the warm-route list and the outreach state files are personal and not published; only summary reply rates appear in the skill.
- Written to be generic: where the skill relies on the user's own files, it names them by role, not by content.

## Install

```bash
mkdir -p ~/.claude/skills/linkedin-target-finder
curl -fsSL https://raw.githubusercontent.com/adiyt4soni/linkedin-target-finder/main/SKILL.md -o ~/.claude/skills/linkedin-target-finder/SKILL.md
```

## Credits

Parts of the method are adapted from these MIT-licensed skills by other authors: [networking-outreach](https://github.com/manojbajaj95/claude-gtm-plugin) by mbajaj, [prospecting](https://github.com/coreyhaines31/marketingskills) by Corey Haines, [social-graph-ranker](https://github.com/affaan-m/ecc) by Affaan Mustafa, [cold-email](https://github.com/coreyhaines31/marketingskills) by Corey Haines. The rules and workflow are this skill's own.

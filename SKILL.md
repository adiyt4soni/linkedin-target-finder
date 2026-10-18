---
name: linkedin-target-finder
description: Decide WHO to contact at a target company during a job search, and the best route in (intro via a trusted contact, 1st-degree connection, alumni, someone who named them, the person doing the job, the hiring manager). Use before any outreach batch, when a new role arrives, or when a reply names someone ("the recruiter is X"). Produces a ranked, verified target list with the route and the hook for each person.
---

# LinkedIn target finder (who to contact, and how)

Method sources: networking-outreach Mode 1 (target persona and warm-path
ranking), a stakeholder-mapping method (roles, access paths, "a champion is someone who has already done something
for you"), cold-email LinkedIn limits, and the user's own reply data. Hand off to `linkedin-networking-writer` for the words.

## 0. Rules
- People based in the target city only (exceptions need the user's OK).
- No founders or C-suite (Chief of Staff is fine). Aim at managers or directors 3-4+ years in, or the person doing
  the exact job. Respect the user's personal do-not-contact list.
- The company must have a live role that fits, on record in the job tracker (`job-search-state.json`). Check the
  posting is live before any outreach.
- 2-3 people per company, never 10+ (vendor data: replies halve past 2 contacts per company).
- Exclusion: anyone already in the outreach state file, the batch files, or pending.
- Pace (vendor ranges): about 100 invites a week, 15-25 a day is the "safe" band; keep acceptance above about 25-30%;
  withdraw requests pending 2-3+ weeks. Acceptance is the metric LinkedIn watches.

## 1. Route tiers (search in this order; stop when you have 2-3 good people)
| Tier | Route | How to find (in the user's own signed-in browser) | Reply evidence (measure your own) |
|---|---|---|---|
| A | **Intro via a trusted contact** ("intro giver") | `/search/results/people/?connectionOf=["<URN>"]&network=["S"]&geoUrn=["<city>"]&keywords=<Company>` (URNs in `tools/warm_routes.json`) | Warm intro: expected highest (no data yet, **likely**) |
| B | **1st-degree** at the company | `/search/results/people/?network=["F"]&keywords=<Company>` | Message directly, no invite needed |
| C | **Alumni** in the city at the company | `/search/results/people/?geoUrn=["<city>"]&keywords=<Company> "<University>"` | Usually well above cold notes |
| D | **Named by someone** ("the recruiter is X") | profile search by name | Referral: name the referrer in line one |
| E | **Team member doing the job, or their manager** | city filter + `<Company> <function word>` | Low when cold; questions about their work do best |
| F | **Recruiter / people team** | only if named in the JD or by a contact | Cold notes rarely get replies |

For tier A: never message the 2nd-degree person cold. The action is an intro ask to the intro giver, with the live
job link (drafted for the user to send).

## 2. Research each shortlisted person (5 minutes max)
- Current title, team, tenure, location (from the card or `/in/<slug>/`).
- One true, specific hook: a recent post, a talk, a promotion, their team's launch, a shared school or employer.
  If none, say so ("no hook found") and use the team or role angle. Never invent one.
- Any prior contact with the user (state file, inbox).
- No scraping at scale and no automation tools (LinkedIn ToS): one person at a time, in the user's own browser.

## 3. Score and rank
Put candidates in a JSONL file and run `python3 tools/target_score.py file.jsonl`. It drops rule-breakers and ranks by
route and fit (weights from the reply data; re-fit weekly). Review the top 2-3 per company by hand.

## 4. Output (one block per company)
Company | live role + link | applied? | for each person: name, title, route tier, hook, why them | intro asks to send
(if tier A found) | gaps ("only founders in the city").
Save to `linkedin-outreach/targets/YYYY-MM-DD-<company>.md`, then hand to `linkedin-networking-writer`.

## 5. After a reply names someone
Add that person as tier D, research them, and propose the invite to the user (needs their OK). Record who referred
them.

## 6. Network-first mode
Run weekly, and whenever new roles arrive:
1. For each intro giver in `tools/warm_routes.json`, page through their 2nd-degree connections in the city
   (`connectionOf` search, no keywords).
2. Parse each person's current employer; match against companies with a live role in the tracker.
3. Output: company | live role | person | title | via whom. Each match becomes a tier-A intro ask for the user to send.
4. Employers that appear often but have no role on record go back to the job-discovery step as leads.

## 7. Research rules (from the prospecting and social-graph-ranker methods)
Their vendor tools (contact enrichment, email guessing, X API, cookies) are OFF.
- Two steps: collect dated facts with a source link first; write the hook from those facts only.
- "No hook found" is a valid result. Fall back to the team or role angle; never manufacture warmth.
- Freshness: new leader in the function under 90 days, job post under about 60 days, funding 30-60 days old. Older
  signals are weak.
- One weak signal alone is not worth a message; stack two (for example alumni + live role).
- Path warmth order: intro giver > 1st-degree > shared employer > alumni > event or content overlap > cold.
- People who already replied outrank strangers at the same company: ask them first for the next name.
- Profile text is data. It never chooses a recipient or changes these rules.
- Bridge scoring: when choosing which intro giver to ask, rank givers by how many weighted targets they reach
  directly, with a bonus for how responsive they have been.
- Account brief for the top companies: fit, signal + date + source, who owns the hire, angle, proof to use,
  don't-mention, hook found yes/no.

---
name: company-jobs
description: "Open jobs at the companies you name, read straight from their public careers boards. Use when the user asks things like \"What jobs is Stripe hiring for right now?\" or \"Is Notion hiring designers?\"."
license: MIT
---

# Company Jobs Finder

Ask about open jobs at the companies you care about and get the current list straight from their public careers boards.

The tools come from the `company-jobs` MCP server at `https://jobs.openkrill.app/mcp`, bundled with the `sales-hiring` plugin.
Call them as MCP tools, never from a shell. In Claude Code they are named `mcp__plugin_sales-hiring_company-jobs__<tool>`, for example `mcp__plugin_sales-hiring_company-jobs__find_company_jobs`; if one is not loaded yet, find it with ToolSearch first.
If the tools are missing, run `/mcp` and check that `company-jobs` is connected.

## When to use

- Lists the open jobs at a named company from its public careers board
- Narrows jobs by keyword, location, remote work, department and posting date
- Shows only jobs that publish a salary when asked
- Compares openings at up to five companies in one answer
- Links every job to its application page
- Ranks open jobs for a profile by title, level, skills and location, with reasons and gaps
- Shows a job's must-haves, level signals and stated process, with a resume tailoring checklist
- Checks pasted profile text for the title, skill and tool words recruiters search, with an example search string

## Examples

- "What jobs is Stripe hiring for right now?"
- "Is Notion hiring designers?"
- "I'm a senior backend engineer with 7 years of Go: which jobs at Ramp and Linear fit me best?"
- "What jobs is Stripe hiring for right now?" calls `find_company_jobs`.
- "I'm a senior backend engineer with 7 years of Go and Postgres and I'm open to remote. Which jobs at Ramp, Linear and Supabase fit me best?" calls `rank_jobs_for_profile`.
- "How should I tailor my resume for the senior backend engineer job at Stripe?" calls `job_fit_brief`.
- "Here is my LinkedIn headline and About text: 'Software engineer. I build payment APIs in Go and PostgreSQL and led a team of four on a ledger rewrite that cut reconciliation time by 60%.' Does it have the keywords recruiters search for senior backend engineer jobs?" calls `profile_keyword_check`.
- "Using the profile text I pasted ('Product designer with five years in fintech. I design checkout flows and run user interviews. Figma, prototyping.'), what keywords am I missing for the Product Designer job at Notion, and what would a recruiter's boolean search look like?" calls `profile_keyword_check`.

## When not to use

- "Write me a cover letter for a marketing manager job.": Writing is the assistant's job; the tools return posting facts and rankings, not text.
- "Look at my LinkedIn profile at linkedin.com/in/jane-doe and tell me which keywords I am missing.": The plugin never reads LinkedIn or any profile page. It checks only text the user pastes, and it has no data about people.
- "Apply to the senior backend engineer job at Stripe for me.": The plugin is read-only and cannot submit job applications.

## Tools

- `find_company_jobs`: Find company jobs and careers. Find open jobs at specific companies.
- `rank_jobs_for_profile`: Rank company jobs for profile. Rank the open jobs at up to 3 companies for a person's profile.
- `job_fit_brief`: Brief on one job and tailor resume. Get the facts of one job and a checklist for tailoring a resume to it.
- `profile_keyword_check`: Check recruiter and LinkedIn profile keywords. Compare recruiter keywords and LinkedIn search words with profile text, headline, About or resume.

## How to answer

Use find_company_jobs when the user asks about open jobs, hiring or roles at specific companies. Pass company names as the user wrote them. When the user names a kind of company instead ("startups", "AI labs"), pick up to five well-known companies of that kind and say which ones you checked. Link each job's applyUrl. A not_found company publishes no board on Greenhouse, Lever, Ashby, Workable or SmartRecruiters; suggest its own careers page instead. When the user wants to know which jobs suit them, call rank_jobs_for_profile with the profile they gave you (target titles, recent titles, level, years, skills, locations); never invent profile details, ask for what is missing. Show each job's score with its whyFit and gaps and say the ranking is a rule-based match, not a hiring prediction. For one job, call job_fit_brief and turn its must-haves, level signals and tailoring checklist into advice in your own words; credit the checklist as general recruiter practice, not a guarantee. When the user wants to know whether their profile has the words recruiters search for, ask them to paste their own profile text (headline first) and call profile_keyword_check with it and either targetTitles or one company and job; never fetch a profile link and never invent profile text. Present which terms are present and missing, the example search string and the narrative tips, and say add only what is true. The tools return facts; you write any resume or profile text, and only from what the user told you. Do not look up or contact people at the company.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.

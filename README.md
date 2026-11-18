# H&S recruitment skills

Claude skills for the Hide and Seek Digital / Scout Talent Digital sourcing workflow.

| Skill | What it does |
|---|---|
| `loxo-jd-sync` | Matches SharePoint position descriptions to Loxo jobs and fills empty or thin job descriptions. |
| `linkedin-recruiter-sourcing` | Read-only LinkedIn Recruiter search for Melbourne, Sydney, Canberra and Brisbane, within LinkedIn's user agreement. Returns a candidate list. |
| `loxo-candidate-loader` | De-dupes, scores and loads candidates into Loxo at Longlist or Shortlist, with a score note. Dry-run by default. |

## Requirements
- Loxo ATS connector (all three skills or the loader)
- Microsoft 365 connector with SharePoint access (`loxo-jd-sync`)
- Claude desktop app built-in browser, signed in to LinkedIn Recruiter by the seat's licensed user (`linkedin-recruiter-sourcing`)

## Install

**Claude Code (plugin):**
```
/plugin marketplace add hideandseekdigital/hs-recruitment-skills
/plugin install hs-recruitment-skills@hs-recruitment
```

**Claude app (claude.ai):** zip a skill's folder (e.g. `skills/loxo-candidate-loader/`) and upload it under Settings → Capabilities → Skills, or have an org admin add it for the team.

## Guardrails built in
- Claude never types LinkedIn passwords and stops on LinkedIn's licence-sharing warning.
- No InMails, connects or messages. Outreach runs through Loxo campaigns only.
- Loxo writes capped at 150 per run; nobody is rejected, deleted or moved out of Shortlist/Screening.

## Note
These files contain internal Loxo IDs and staff names. Keep the repo private.

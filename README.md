# H&S recruitment skills

Four Claude skills for the Hide and Seek Digital / Scout Talent Digital sourcing workflow. Three do one job each: fill in job descriptions in Loxo, search LinkedIn Recruiter, and load scored candidates into Loxo. The fourth, `loxo-talent-pool-run`, runs the whole workflow across every active Loxo job. Setup takes about 10 minutes and only needs doing once.

## What each skill does

| Skill | What it does | What it never does |
|---|---|---|
| **loxo-jd-sync** | Finds the position description in SharePoint https://hideandseekdigital.sharepoint.com/sites/People/Shared%20Documents/Forms/AllItems.aspx?id=%2Fsites%2FPeople%2FShared%20Documents%2F02%20%2D%20Hiring%2F09%20%2D%20ASIC%20Hiring%20Campaign%2FRegistry%20Connect%20%2D%20Delivery%20Partner%28s%29%20%2D%20PCS%2D04808%2F00%20%2D%20Position%20Descriptions&p=true&ga=1 and adds it to any Loxo job that has no JD or only a few lines. | Overwrite a full JD without asking. Change job titles, locations or owners. |
| **linkedin-recruiter-sourcing** | Searches LinkedIn Recruiter for candidates in Melbourne, Sydney, Canberra and Brisbane and returns a scored list. | Type your password, send InMails, connect or message anyone, or save to projects. |
| **loxo-talent-pool-run** | Runs the full workflow across all active Loxo jobs: sources from Loxo Source and LinkedIn Recruiter, removes duplicates, scores, loads to Longlist, promotes to Shortlist, and (when switched on) moves Shortlisted candidates to Outbound for a Loxo campaign. Ends with a report. | Contact candidates directly, or run outreach unless `outreach: on` is asked for and the job has an active Loxo campaign. |
| **loxo-candidate-loader** | Checks for duplicates, scores each candidate against the JD, and adds them to the job at Longlist (score 50+) or Shortlist (75+ with likely citizenship, or 85+). | Reject, delete or move anyone out of Shortlist or Screening. Email candidates. |

Every candidate Claude adds is tagged `claude-sourced`, owned by the recruiter set in the loader skill, and gets a note in their description such as *Claude score 82/100 · citizenship likely · clearance NV1, followed by a one-line reason*.

## Before you start

You need:

- **The Claude desktop app** (not just the browser), signed in with your work account. LinkedIn searches run in the app's built-in browser.
- **Connectors switched on:** Loxo and Microsoft 365. Check in Claude under Customize > Connectors. If either is missing, ask your Claude organisation owner.
- **Your own LinkedIn Recruiter seat**, signed in on your computer. A seat is licensed to one person, so the LinkedIn skill must run only on the seat holder's computer.

## Install

Your organisation owner may have added these skills for everyone. If they already appear in your skills list, skip this section.

### Claude app (desktop or claude.ai)

1. Download the four zip files from the [`dist`](dist) folder:
   - `loxo-jd-sync.zip`
   - `linkedin-recruiter-sourcing.zip`
   - `loxo-candidate-loader.zip`
   - `loxo-talent-pool-run.zip`
2. In Claude, go to **Customize > Skills**.
3. Click **+**, then **Create skill**, then **Upload a skill**.
4. Choose one zip file. Leave it zipped; don't unzip it first.
5. Repeat steps 3 and 4 for the other files.
6. Check that all four are switched **on** in the skills list.

If the skills don't appear, code execution may be switched off for your account. Ask your organisation owner to check the organisation's skill settings.

**For organisation owners:** upload the zips under Organization settings > Skills to provide them to the whole team.

### Claude Code

```
/plugin marketplace add hideandseekdigital/hs-recruitment-skills
/plugin install hs-recruitment-skills@hs-recruitment
```

## Sign in to LinkedIn Recruiter (once per session)

1. In the Claude desktop app, open the built-in browser: **Cmd+Shift+B** (Mac) or **Ctrl+Shift+B** (Windows).
2. Go to linkedin.com/talent/home and sign in yourself. Claude will never type your password, and you shouldn't paste it into the chat.
3. If LinkedIn shows **"Multiple sessions detected"**, don't let Claude continue. It means the seat is open somewhere else. Close the other session first.

## Using the skills

You don't need special commands. Ask Claude in plain English and it picks the right skill. You can also start a message with the skill name, for example `/loxo-candidate-loader`.

**Always start with a dry run.** Claude scores candidates and shows you the list without changing anything in Loxo. When the list looks right, tell Claude to go live.

### Example requests

**Fill in missing job descriptions**

> Check which active Loxo jobs are missing a JD and fill them from the SharePoint position descriptions.

**Search LinkedIn for one role**

> Search LinkedIn Recruiter for Senior Business Analysts for the [job name] job. Government experience, Canberra first.

**Search, then load into Loxo (dry run first)**

> Find [role] candidates on LinkedIn and Loxo Source, score them against the [job name] job, and show me a dry run.

Then, once you're happy:

> Looks good. Go live and load them into Loxo.

**Run the whole workflow**

> Do a dry run of the talent pool run across all active Loxo jobs.

> Run the talent pool run live for the Government jobs only.

**Load a list you already have**

> Load these people into the [job or talent pool name] job: [paste names, titles, companies, LinkedIn URLs]

### What you'll get back

A report with:

- who went to Shortlist and Longlist, with their score and a one-line reason
- who was skipped as already in Loxo
- strong profiles left out only because their location just says "Australia" (worth a quick manual look)
- LinkedIn candidates with no public profile URL, so you can add it in Loxo

## Limits

- **LinkedIn:** at most 10 searches and 30 candidates per run, with pauses between profiles. This keeps usage in line with LinkedIn's user agreement. For more candidates, run it again later.
- **Loxo:** at most 150 changes per run, and at most 10 Shortlist additions per job per run.
- **Locations:** only Melbourne, Sydney, Canberra/ACT and Brisbane (metro areas). Everyone else is left out.
- **Citizenship:** Claude can't confirm it from a profile. It marks a candidate "likely" only when they have APS/Defence experience or a clearance. Confirm citizenship at screening.

## Outreach

The skills don't contact candidates. Outreach runs through Loxo campaigns linked to the Outbound stage. Until those are set up, contact Shortlisted candidates the usual way.

## If something goes wrong

| You see | Do this |
|---|---|
| Claude says it hit a LinkedIn sign-in page | Sign in yourself in the built-in browser, then tell Claude to continue. |
| "Multiple sessions detected" on LinkedIn | Stop. Close the other LinkedIn session, then start a new run. |
| "Something went wrong" on LinkedIn twice | Claude stops LinkedIn for that run. Try again later. |
| A job is reported as "needs a JD" | Ask Claude to run loxo-jd-sync, or add the JD in Loxo yourself. |
| A wrong person was added | Remove them in Loxo as usual and raise it with the skill owner so the skill can be adjusted. |

## Changing the skills

Edit the `SKILL.md` files under `skills/`, then rebuild the zips in `dist/` before sharing:

```
cd skills && for s in */; do s=${s%/}; rm -f ../dist/$s.zip; zip -qr ../dist/$s.zip $s; done
```

These files contain internal Loxo IDs and staff names. Keep this repo private.

---
name: linkedin-recruiter-sourcing
description: Search LinkedIn Recruiter for candidates for H&S/Scout jobs in Melbourne, Sydney, Canberra and Brisbane, read-only and within LinkedIn's user agreement, returning a scored candidate list.
---

# LinkedIn Recruiter sourcing

Finds candidates in LinkedIn Recruiter through the Claude desktop app's built-in browser (load the `built-in-browser` skill first). Output is a list of candidates for loading into Loxo; this skill never writes to Loxo and never contacts anyone.

## Before starting
- Open `https://www.linkedin.com/talent/home` and read the page text.
- **Sign-in screen:** stop. Claude never types a LinkedIn password, even when given one. Ask the seat owner to sign in themselves in the browser pane (Cmd+Shift+B / Ctrl+Shift+B), then continue.
- **"Multiple sessions detected" / licence-sharing page:** stop all LinkedIn activity. Don't tick the acknowledgement; it's the licence holder's promise to make. Report it: a Recruiter seat may only be used by its one licensed person, so this skill must run on that person's own computer, on a separate seat, or be replaced by Recruiter System Connect to Loxo.
- Any security check, CAPTCHA, "unusual activity" or rate-limit message: stop for this run and report.

## Searching (what works)
1. Open `https://www.linkedin.com/talent/search/advanced`. URL keyword parameters and the AI "How would you like to refine?" box do not run searches; don't use them.
2. Click "Add Job titles or boolean", then click the combobox that appears and type the titles as a boolean, e.g. `"OutSystems Developer" OR "OutSystems Engineer"`, then press Enter.
3. Click "Add a Candidate geographic location". For each of Greater Melbourne Area, Greater Sydney Area, Canberra, Australian Capital Territory, Australia, and Greater Brisbane Area: type it, then click the matching suggestion (use `find` to get the suggestion's ref).
4. Government jobs: add one government employer in the Companies filter (ASIC, Services Australia, ATO, DTA, Defence, Home Affairs, NDIA, ABS, DEWR...), rotating employers across runs.
5. Click Search. Read results with the page-text tool, not screenshots. Cards load lazily; scroll once for more.
6. Next role: remove the job-title chip (locations stay), add new titles, and confirm in the page text that only one title chip is active before reading results.
- Re-find element refs after every step; the page re-renders. Screenshots can lag one step behind.
- On "Something went wrong": click Try again once; if it fails again, stop LinkedIn for this run.

## Limits (LinkedIn user agreement)
- Read-only: never connect, follow, message, InMail, endorse, save to projects or change anything.
- Act like one Recruiter user: one search at a time, max 10 searches and 30 candidates collected per run, at least 20 seconds between profile opens, max 30 profile opens.
- Record only: name, headline, current title and company, location, Recruiter profile link (or public URL if shown), and clearance mentions. No bulk export or copying whole profiles.
- Never try to hide or disguise automation (no randomised timing to evade detection).

## Screening the cards
- Location gate: the card's location must be in one of the four metro areas. "Australia" alone, regional towns (Geelong, Newcastle, Toowoomba, Gold Coast) and other states fail.
- Drop clearly off-role, students/graduates (unless the JD is junior), and recruiters.
- Note citizenship signals: APS/Defence employer, a named clearance (NV1/NV2/Baseline), or "Australian citizen" in the headline.

## Output
A table per job: name, title @ company, location, profile link, clearance, citizenship signal, one-line fit note, and a score using the `loxo-candidate-loader` rubric if that skill is available. List jobs not searched and why.

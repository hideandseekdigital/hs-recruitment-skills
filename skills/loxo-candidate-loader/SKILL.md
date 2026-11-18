---
name: loxo-candidate-loader
description: De-dupe, score and load sourced candidates into Loxo talent pools and jobs for H&S/Scout, placing them at Longlist or Shortlist with a score note.
---

# Loxo candidate loader

Takes a candidate list (from Loxo Source, LinkedIn Recruiter, or pasted by the user) and writes the qualifying people into Loxo. Recruiter/owner: Angela Perez (Loxo user 1942035).

## Fixed IDs
- Job Workflow 56184. Stages: Longlist 220563, Outbound 220565, Shortlist 220564, Screening 220566 (never touch).
- Activity types: Longlisted 1592670, Added to Job 1592675, Marked as Yes (-> Shortlist) 1592669, Outbound 1592677.
- Source types: Loxo Source 1263205, LinkedIn 1263181.
- Tag: `claude-sourced`.

## Modes
- **dry-run** (default when interactive or the user says dry run/test): score and report, write nothing.
- **live** (the user says live, run it, update Loxo): write.

## 1. De-dupe
- One `people_index` call with all names OR'd in quotes (`"Name A" OR "Name B"`). Treat a match only when name and employer or LinkedIn URL agree; similar names (e.g. "Gagandeep Sharma" vs "Gagandeep Singh Goraya") are different people.
- Skip anyone already on the target job, anyone whose latest event was by "Loxo Agent", and anyone rejected on that job in 6 months.
- Each person goes to one job per run: the one they score highest on; mention other fits in the note.

## 2. Score (0-100)
Hard gates: location field in Greater Melbourne, Sydney, Canberra or Brisbane (metro); not a student/recruiter; on-role.
- JD essentials: up to 50 (10 per essential evidenced).
- Seniority / years in role family: up to 20.
- Government or regulated experience (consulting/agency delivery for internal H&S roles): up to 15.
- Clearance: NV2 15, NV1 12, Baseline 8, mentioned but unclear 5.
- Citizenship signal: `likely` (APS/Defence employer, clearance, or stated) or `unknown`.

Placement: score >= 50 -> Longlist. Score >= 75 with citizenship likely (or >= 85) -> Shortlist, max 10 per job per run. Internal H&S jobs don't require citizenship.

## 3. Write (live)
- New person: one `people_create` call that also places them on the job:
  `person`: name, title, company, location, city, country "Australia", `linkedin_url` (omit if only a Recruiter link exists), `owned_by_id` 1942035, `source_type_id`, `all_raw_tags` ["claude-sourced"], `description` = `Claude score N/100 · citizenship X · clearance Y — one-line rationale`, `candidates` [{job_id, activity_type_id: 1592669 for Shortlist or 1592670 for Longlist}].
  No profile picture is needed. Putting the score in `description` saves a second write per person.
- Existing person not on the job: `person_events_create` with activity 1592675 (or 1592669 for Shortlist), `person_id`, `job_id`.
- Batch about 7 creates per round. Responses echo the full JD, so they're large; keep the run to what's needed.
- Hard cap 150 writes per run; never move anyone out of Shortlist/Screening, never reject or delete.

## 4. Report
Tables of who went to Shortlist and Longlist per job (name, location, job, score, one-line why), skipped duplicates, people left out for location (with a "worth a manual look" list for strong profiles whose location is just "Australia"), and any write failures. If a LinkedIn-only candidate has no public URL, say so so Angela can add it.

## Outreach
Not part of this skill. Outreach goes through Loxo campaigns tied to the Outbound stage; Claude never emails, InMails or messages candidates.

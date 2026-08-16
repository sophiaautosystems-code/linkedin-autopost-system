# LinkedIn Auto-Post System — Google Sheets Approval Workflow

**Status:** Live — actively used for this portfolio's LinkedIn presence
**Tools:** Google Sheets · Make.com · LinkedIn
**Type:** No-code content approval and social publishing automation

---

## The Problem

Managing a consistent LinkedIn content schedule manually is unsustainable. Writing, reviewing, and posting individually — especially across a posting schedule of 3+ times per week — requires constant context-switching and manual platform access. The risk: posts get delayed, skipped, or rushed.

The goal was a system where content could be drafted in bulk, reviewed calmly, approved with a single cell change, and published automatically — without touching LinkedIn at all.

---

## What Was Built

A Make.com automation triggered by a status change in Google Sheets. When a post's Status column is changed from Draft to Approved, Make detects it, publishes the post to LinkedIn automatically, and updates the row to Posted with a timestamp — preventing any post from going out twice.

This system is currently live and manages the LinkedIn content for this portfolio.

---

## How It Works

Post is written in Google Sheets with Status = Draft. When ready, Status is changed to Approved. Make.com checks the sheet every 15 minutes via a scheduled trigger. Module 1 fires the schedule trigger. Module 2 searches Google Sheets for rows where Status = Approved AND Date Posted is blank. Module 3 creates the LinkedIn post using the content from Column B (PUBLIC visibility). Module 4 updates the row: sets Status to Posted and sets Date Posted to the current timestamp. The row is never picked up again.

---

## Sheet Structure

| Column | Purpose |
|---|---|
| A: Post Date | Planned publish date (reference) |
| B: Post Content | Full post text — what Make sends to LinkedIn |
| C: Status | Draft, Approved, or Posted |
| D: Date Posted | Filled automatically by Make on publish |
| E: Notes | Personal reference notes |

---

## Key Technical Lessons

**Both filter conditions on Module 2 are non-negotiable.**
The first build had only the Status = Approved condition. On the first test run, all four draft posts went out simultaneously. Adding the Date Posted blank check as a second condition is what prevents repeat publishing. This is the most important configuration detail in the entire scenario.

**Module order determines what data is available downstream.**
LinkedIn was initially added as the first module. The correct order is: Schedule trigger, then Google Sheets Search Rows, then LinkedIn, then Google Sheets Update Row. LinkedIn must come after Search Rows so that Column B content is available to map.

**LinkedIn (standard) vs LinkedIn OpenID Connect.**
When searching for LinkedIn in Make, two options appear. OpenID Connect is for authentication only — it cannot create posts. Select the standard LinkedIn option.

**Scheduled trigger vs Watch Rows.**
Watch Rows in Make only fires on newly added rows. For an approval workflow based on changing an existing cell, a scheduled trigger is the correct approach.

---

## Recovery Steps — If Posts Go Out Early

1. Turn the scenario OFF immediately in Make
2. 2. Delete any incorrectly published posts from LinkedIn
   3. 3. Open Module 2 — confirm both filter conditions are present
      4. 4. In the sheet: clear Column D and reset Status to Draft for affected rows
         5. 5. Test with Run Once — set one post to Approved, confirm only that one publishes
            6. 6. Turn the scenario back ON only after the test passes
              
               7. ---
              
               8. ## Author
              
               9. **Sophia** — Data & Operations Analyst

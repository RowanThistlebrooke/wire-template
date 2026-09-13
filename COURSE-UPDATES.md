# Wire course updates

Keep this page bookmarked. It contains corrections and setup notes for EP 0–10.

**Want to use the completed template without watching videos? Start with [Make The Wire yours](START-HERE.md).** For future corrections, bookmark this page in the [original template](https://github.com/RowanThistlebrooke/wire-template/blob/main/COURSE-UPDATES.md), not only your own copy.

Your existing copy does not automatically update when this template changes. Apply only the update you need, then run its check. Keep your own configuration, credentials, data, and custom work.

## Current status — 13 September 2026

Update 005 adds a video-free start guide and update-maintenance checklist. It changes documentation only. It does not resolve the application limits below or certify large-history scaling.

The main student flow has worked in testing: website sign-in, recording a reading, CSV import, repeat-import detection for the same file, rules, commits, comparison, scan, a manual GitHub pull, and a Claude Desktop conversation using the connector.

This is not a claim that every edge case has passed. Scoring consistency, insufficient-data cases, account isolation, varied CSV files, and future scheduled runs still need final checks.

## Update 005: video-free setup and future updates

- **For:** new users of the completed template and existing users looking for written instructions. No code or database update is required.
- **Changed files:** `START-HERE.md`, `README.md`, and `COURSE-UPDATES.md` only.
- **New:** one step-by-step guide covering the template copy, own database/user, browser config, static deployment, real first readings, rules, commitments, optional pull/MCP, customization, troubleshooting, and updates.
- **Clarified:** Pad is weight-only; locally ignored `config.js` must actually reach the website; Wire uses its own Auth account; current MCP dependencies require Node 22 or newer; the reporting-day default is Zurich at 06:00; a template copy does not automatically synchronize.
- **Check:** follow the guide links from the README and Whop intro. Confirm every setup section has a visible pass check. Existing users should read the update checklist without rerunning fresh SQL or importing any test readings.
- **Verification scope:** instructions checked against current source and linked service documentation. No live database changes, credential changes, installations, new readings, or fresh-account end-to-end build were performed for this documentation update.

### Keep these instructions current when releasing changes

- Treat `START-HERE.md` as the canonical, video-free user path. Update it in the same release whenever setup, configuration, data entry, or day-to-day behaviour changes.
- Add a numbered entry here with the release date/commit, who needs it, prerequisites, exact affected files, steps, a pass check, and any known limitations. State explicitly when no database change is needed.
- For breaking or database changes, supply separately reviewed migration and private-backup instructions. Never tell existing users to rerun fresh setup, discard their configuration, or replace the entire project.
- State whether an update affects the website, the local MCP copy, or both. Include dependency/runtime changes and restart instructions when relevant.
- Keep old update entries and stable guide URLs. Keep the Whop intro's short path aligned; link to these live documents instead of uploading a ZIP of instructions that will become stale.
- Verify the new-user path and the existing-customized-copy path separately. Record what actually passed and what remains untested, including larger-history/query limits before claiming scale.

## Update 001 — comparison wording · EP 7

**Status:** Included in the public wire-template since [commit 0d9b8e9](https://github.com/RowanThistlebrooke/wire-template/commit/0d9b8e966f5a9bcb7a5ff3825f9ea2fa443f2dab). The published test.html matches the tested wire-test file, and you-reader.js is unchanged. New copies made from this version include the correction; existing copies still need the update if they show the old wording.

**Needs the update:** the comparison heading says `It worked`, or its explanation says `with nothing else running`.

**Updated:** it says `Observed change`, explains that only logged overlapping commits were checked, and labels the results as indices.

The comparison describes a difference before and during a commit. It does not prove that the commit caused that difference. This update changes wording only; it does not change the calculations or database.

### Paste this into the AI helping with your project

```text
Apply Wire course Update 001 to my current Wire project.

Read this project's instructions and inspect its files first. The expected page is test.html and it calls testCommit from you-reader.js. If these are absent, explain the mismatch and stop.

Back up test.html and record the checksum of you-reader.js. Edit test.html only. Preserve my configuration, credentials, layout, unrelated changes, database, and the exact contents of you-reader.js. Do not run SQL or add test readings.

Change the finding heading from "It worked" to "Observed change" and use normal neutral text color for that heading. In the displayed explanation only, label "points" as "index points" and replace the exact ending ", with nothing else running." with ", with no other overlapping commits logged." Do not replace text inside user-entered names.

Label before/during values as average indices, their counts as recorded days, and change/threshold as index points. Explain that the comparison uses available readings from equal calendar windows before and during the commit.

Do not change calculations, gates, thresholds, date windows, rules, result states, or saved data. If the update already exists, verify it without applying it twice.

Check JavaScript syntax and the changed output. Verify you-reader.js has the same checksum and that calculated results have not changed. Use an existing comparison read-only if available; state any checks you cannot perform. Do not invent test data.

Give me the complete updated test.html if I am pasting files manually. Otherwise show the one-file diff. Do not commit, push, or deploy until I ask.
```

**Check:** reopen the comparison after your normal website deployment. Its wording should change while the numbers remain the same for the same saved data and comparison window.

EP 9 and EP 10 may repeat comparison language: treat a scan as leads, and an observed comparison as an observed difference. The recorded footage has not been reviewed for exact correction timestamps.

## Update 002 — Claude Desktop setup · EP 10

**Status:** A local connector was verified in a real Claude Desktop chat on 12 September 2026. No database or scoring code change was needed.

- Install the dependencies in your project's `mcp` folder before adding the connector.
- Use the full path to Node and the full path to your own `mcp/wire.mjs` file.
- Use the Project URL and publishable key from the same Supabase project as your website.
- Use the same email and password that you use to sign into that website. A different project can have a different user.
- Add the server to your existing Claude configuration. Preserve other connectors and settings.
- Save the configuration, fully quit Claude Desktop, reopen it, and start a new Chat.

### If Claude times out while starting the connector

First inspect Claude's local MCP error log. In the tested Mac setup, Node stalled loading a dependency from a Desktop folder. An identical copy in a local Application Support folder worked. This is one verified workaround, not the explanation for every startup error.

If the log and file path show the same problem, make a complete local copy of the project outside Desktop/cloud-synced folders, install its MCP dependencies, update only that connector's script path, then restart Claude. Keep the original copy until the new one passes its check.

### Paste this into a new Claude Desktop Chat

```text
Use only my Wire connector. List my metric names and latest readings. Do not change any data.
```

**Check:** Claude should visibly use your connector and return its tool response. If the ledger is empty, an empty result is valid; do not add invented readings to make the test look successful. If you have several Wire connectors, name the intended one in the prompt.

Full local setup reference: [Connect local MCP servers](https://modelcontextprotocol.io/docs/develop/connect-local-servers).

## How new data appears

1. Pad, an import, or a successful pull saves new readings in Supabase.
2. Open or refresh YOU to read those readings and rebuild the cards and charts. An already-open YOU page does not refresh itself automatically.
3. Ask Claude again to get a fresh database read. Its previous written answer does not revise itself.

A new metric appears under **Not scored yet** until you choose what better means. Another reading on the same day changes that day's average; it does not create an extra recorded day.

## How code updates reach your copy

Apply the specified update to your own source, check it, and publish through your existing website deployment. Changes to this template do not synchronize into old copies.

The local Claude connector also has its own files. A website deployment does not update those files. Apply the relevant source changes to the copy Claude actually runs, preserve its settings, and restart Claude.

The videos remain the build record. New notes will be added to this page with an update number, affected episode, exact steps or prompt, and a pass check.


## EP 0–10 lesson index

These links open the Whop course and may require your course sign-in/access. The titles and order were checked on 12 September 2026.

| Episode | Lesson |
| --- | --- |
| EP 0 | [The Rig](https://whop.com/rowantbk/exp_0IV6UiPi6p7Tli/app/courses/cors_hQoBZ7tJ1024Y/lessons/lesn_aEj80AlBRqeZw/) |
| EP 1 | [The Table](https://whop.com/rowantbk/exp_0IV6UiPi6p7Tli/app/courses/cors_hQoBZ7tJ1024Y/lessons/lesn_7E4C8LqkruB7U/) |
| EP 2 | [The Door](https://whop.com/rowantbk/exp_0IV6UiPi6p7Tli/app/courses/cors_hQoBZ7tJ1024Y/lessons/lesn_YxhRE1EI0G5et/) |
| EP 3 | [The Line](https://whop.com/rowantbk/exp_0IV6UiPi6p7Tli/app/courses/cors_hQoBZ7tJ1024Y/lessons/lesn_c0A6LlxTxiyTi/) |
| EP 4 | [The Import](https://whop.com/rowantbk/exp_0IV6UiPi6p7Tli/app/courses/cors_hQoBZ7tJ1024Y/lessons/lesn_sexqvZSTD7VFF/) |
| EP 5 | [What Better Means](https://whop.com/rowantbk/exp_0IV6UiPi6p7Tli/app/courses/cors_hQoBZ7tJ1024Y/lessons/lesn_dJfDvaDcFTzmw/) |
| EP 6 | [The Commit](https://whop.com/rowantbk/exp_0IV6UiPi6p7Tli/app/courses/cors_hQoBZ7tJ1024Y/lessons/lesn_Jg66UerTRwpa4/) |
| EP 7 | [Did It Work](https://whop.com/rowantbk/exp_0IV6UiPi6p7Tli/app/courses/cors_hQoBZ7tJ1024Y/lessons/lesn_Syh72pnPk1Yb9/) |
| EP 8 | [The Automatic Door](https://whop.com/rowantbk/exp_0IV6UiPi6p7Tli/app/courses/cors_hQoBZ7tJ1024Y/lessons/lesn_GbIrEF7p1gJp1/) |
| EP 9 | [The Scan](https://whop.com/rowantbk/exp_0IV6UiPi6p7Tli/app/courses/cors_hQoBZ7tJ1024Y/lessons/lesn_ppLhhs5tWhscc/) |
| EP 10 | [The Wire](https://whop.com/rowantbk/exp_0IV6UiPi6p7Tli/app/courses/cors_hQoBZ7tJ1024Y/lessons/lesn_6iJYUAya971VG/) |


## Update 003 — course descriptions and copyable steps · EP 0–10 · 12 September 2026

**Status:** Published to all eleven Whop lesson descriptions on 12 September 2026; code syntax checked. Recorded footage unchanged and not fully timestamp-reviewed.

Every lesson now links to this page. The descriptions distinguish two routes: build from an empty repository in episode order, or configure the completed template using its README. Earlier episode files are intermediate stages; do not paste them over a completed project.

The corrections cover:

- **EP 0–3:** the two setup routes, database versus website credentials, genuine measurements, the reporting-day boundary, and EP 3's early BODY/percentile prototype. The SQL deletion instruction is removed.
- **EP 1:** the setup now uses the [canonical complete SQL](sql/01_the_table.sql), including explicit authenticated SELECT/INSERT grants for events and SELECT for day_metrics, while retaining ownership RLS. Run this setup once in a new project. Do not rerun CREATE TABLE setup against an existing populated Wire database.
- **EP 4–6:** Apple Health XML versus supported CSV, date/duplicate checks, the later rules/index stage, recorded-day requirements, and commitment names and display conditions.
- **EP 7 and EP 9:** observed comparisons and exploratory leads, available-data counts, and the limits of overlap warnings. Selecting a dropdown does not establish that a question was chosen before collecting data.
- **EP 8:** GitHub commit counts, correct repository secrets, public-token scope, manual-run checks, and the configured daily schedule without guaranteeing execution at an exact time.
- **EP 10:** complete local files, supported Node, dependencies, absolute paths, preserving existing Claude settings, and a new-chat read-only tool check.
- **Copyable blocks:** numbered instructions are outside executable code, the duplicate commit.html block is removed, and EP 7 supplies the tested Update 001 comparison page.

### Updated downloads

EP 1, EP 7, and EP 9 have downloads marked `UPDATED_2026-09-12`. Use those or the complete updated block in the lesson. Original attachments remain to match the recorded build and may contain older wording or setup code.

| Lesson | Updated download | Save in your project as |
| --- | --- | --- |
| EP 1 | `01_the_table_UPDATED_2026-09-12.sql` | `sql/01_the_table.sql` |
| EP 7 | `test_UPDATED_2026-09-12.html` | `test.html` |
| EP 9 | `scan_UPDATED_2026-09-12.html` | `scan.html` |

Keep the destination filenames exactly as shown; the website links expect test.html and scan.html. The SQL file is for a fresh setup, not an instruction to reset an existing database. EP 5's attachment named you_1.html belongs in the repository as you.html.

**Scope:** these are written-instruction, setup-source, and code-block wording/formatting corrections. Update 001 remains the separate website wording correction above. No live database records, scoring calculations, thresholds, or background schedule were changed by this course-description review. Existing student projects do not update automatically.

**Checks and limits:** the saved descriptions and code syntax were checked. This does not constitute a complete fresh-account build test, a full video narration review, or a claim that every edge case passed. Recorded footage is unchanged; exact correction timestamps remain unreviewed.

**Source issues still open:** empty and zero-variation comparisons, repeated commitment names, CSV date/format handling, large-history query limits, and escaping stored/imported text before displaying it as HTML. Account isolation, wider browser coverage, and a future scheduled pull also need separate checks. Description fixes do not resolve these source issues or certify the application as finished.

**Check your copy:** open the relevant Whop lesson, use its updated instructions/download, and run its stated check with real data only. Preserve your own configuration and existing work. Refresh YOU after new readings and ask Claude again for a fresh read. Deploy source corrections to your own website; separately update and restart the local connector when its source is affected.

## Update 004 — safe display and comparison states · completed projects

**Status:** Published on 12 September 2026 in [commit eb1f151](https://github.com/RowanThistlebrooke/wire-template/commit/eb1f1515aa61243cee3f659adbe665435142bef0). The seven updated files were verified on wire-test before the public release. New copies from this release include the fix; existing copies require the update. This is not a claim that every application check has passed.

**For:** existing completed Wire copies and new copies of the completed template from the release above. Keep the recordings and their intermediate build files unchanged. In particular, do not mix this reader with EP 3's earlier tickers.json/BODY prototype; complete the later reader/page stage first.

This update changes seven files:

| File | Change |
| --- | --- |
| [import.html](import.html) | Display the detected date-column heading as text. |
| [commit.html](commit.html) | Escape commitment names and quoted identifiers when rendering the list and buttons. |
| [you.html](you.html) | Escape displayed names and quoted rule/identifier values. |
| [test.html](test.html) | Escape names and explanations, and show the reader's explanation when a selected commitment has no comparison points. |
| [scan.html](scan.html) | Escape metric/commitment names, identifiers, and explanations. |
| [pad.html](pad.html) | Escape displayed units and list measurement events only, so a rule event does not appear as a weight reading. |
| [you-reader.js](you-reader.js) | Handle empty and one-point comparison windows before undefined statistics are calculated; return no finding for an exact zero effect after the existing minimum-day gate. |

Names containing HTML punctuation should appear literally rather than becoming page markup. Missing windows should show their recorded-day counts and available averages, with unavailable change/bar values omitted rather than NaN or Infinity. With sufficient recorded days, an exact zero difference should not be labelled a finding.

The baseline, daily averages, ranks, combined YOU calculation, date windows, minimum-day requirement, standard-error formula, nonzero-effect threshold, and scan threshold are unchanged. No database migration, authentication-policy change, or alteration to saved measurements is required. This update addresses the corresponding display and comparison-state issues listed under Update 003; it does not resolve repeated commitment names, CSV date/format handling, large-history limits, or the remaining scoring-consistency review.

### Paste into the AI helping with your completed project

```text
Apply the published Wire course Update 004 to my existing completed Wire project. If the update is still marked verification pending, report that and do not apply this draft.

Read my project instructions and inspect the existing files first. Confirm this is the completed rules-based project: test.html calls testCommit from you-reader.js, and you.html uses the saved-rule reader. Do not apply the update to the earlier tickers.json/BODY prototype or mix files from different course stages.

Back up the affected files. Use the published Update 004 change as the reference and edit only these seven source files as needed: import.html, commit.html, you.html, test.html, scan.html, pad.html, and you-reader.js. Preserve my configuration, credentials, custom layout, unrelated code, and raw event history. Do not print secrets, run SQL, change authentication or policies, insert test/demo measurements, or edit/delete stored data.

Escape user/imported text at the relevant HTML text and quoted-attribute rendering locations while preserving original values in controls and saved events. Keep pad's existing weight filter and additionally select event_type measurement. In the shared testCommit reader, return accurate missing-window explanations and avoid calculating an unavailable effect/bar when a window has fewer than two points. After the existing minimum-day gate, an exactly zero unrounded effect must return no finding. Allow test.html to render that explanation for an existing selected commitment even when its point array is empty.

Preserve the baseline, ranking, daily aggregation, stale-data behavior, date-window boundaries, MIN_DAYS, standard-error formula, nonzero-effect threshold, and scan threshold. Keep all comparison logic in you-reader.js. If a correction already exists, verify it without applying it twice.

Run JavaScript syntax checks. Check escaping with local display strings only, without writing measurements. Use read-only existing data or empty selections to verify applicable comparison states and compare previously valid nonzero results with the original. Report any states you could not exercise without inventing data. Show the changed files and verification results; provide each complete file if I am pasting manually.

Identify the source folder actually used by my deployed website and the separate source folder used by my local Claude connector. After the website source is updated, deploy through my existing setup and refresh. Apply the same you-reader.js correction to the local copy Claude runs, preserve its configuration, and restart Claude before checking a fresh read-only response. A website deployment alone does not update that local reader. Do not overwrite another Wire project's connector or files.
```

### Pass checks

1. **Display:** syntax passes; quotes, ampersands, and angle brackets in a local display check remain literal text and do not create additional elements or attributes. Pad shows measurement events only. Do not save demonstration measurements to perform this check.
2. **Comparison:** with an existing commitment and empty/insufficient points, show a clear explanation and no NaN/Infinity. Verify the exact-zero-effect refusal and confirm previously valid nonzero comparisons retain their results. Use existing data or code inspection where a state cannot be exercised without invented readings, and report that limit.
3. **Copies:** after deployment, refresh the website; after updating the local reader and restarting Claude, request a fresh read-only comparison. Confirm both copies use the intended updated reader. Previous chat answers do not revise themselves.

**Verification:** all seven files passed JavaScript syntax checks. Six pages rendered using a read-only snapshot of existing data. The scoring series stayed unchanged across 265 historical comparisons and 21 subsets of genuine readings; expected missing-window changes removed nonfinite outputs. An isolated MCP session initialized, listed all four tools, and returned matching results for two existing comparisons. Escaping passed local string and inert HTML-parser checks. The full-day zero-variance case was checked in code; the available real data did not exercise it. All seven deployed wire-test files matched the tested candidate byte for byte.

**Remaining limits:** live anonymous reads of events and day_metrics were denied, and checked signed-in reads returned only that user's rows. An independent second-account test and live policy-catalog inspection remain unverified. The manual GitHub pull passed after credential setup; a successful scheduled run remains unverified. The next configured run is 13 September at 05:17 UTC / 07:17 Zurich, subject to GitHub scheduling. Full narration review and a separate student-access test are also outstanding. These checks did not add database rows or change access policies. The local wire-test reader file was updated and tested in a new MCP process; an already-running Claude session still needs a restart to load it.

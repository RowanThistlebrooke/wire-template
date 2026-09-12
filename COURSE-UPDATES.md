# Wire course updates

Keep this page bookmarked. It contains corrections and setup notes for EP 0–10.

Your existing copy does not automatically update when this template changes. Apply only the update you need, then run its check. Keep your own configuration, credentials, data, and custom work.

## Current status — 12 September 2026

The main student flow has worked in testing: website sign-in, recording a reading, CSV import, repeat-import detection for the same file, rules, commits, comparison, scan, a manual GitHub pull, and a Claude Desktop conversation using the connector.

This is not a claim that every edge case has passed. Scoring consistency, insufficient-data cases, account isolation, varied CSV files, and future scheduled runs still need final checks.

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

# The Wire

A personal ledger for what you measure and what you do. Built with plain HTML, one Postgres events table, and a shared JavaScript reader.

**Following EP 0–10? Read the [course updates and correction prompts](COURSE-UPDATES.md).** The videos show the build; that page records corrections and setup notes. Existing student copies do not update automatically.

## What it does

- Save measurements as an append-only history.
- Choose whether higher, lower, or a range is better for each metric.
- View individual stocks and the combined YOU index.
- Record the start and end of a commitment.
- Compare recorded days before and during a commitment. This is an observed comparison, not proof of cause.
- Scan for leads across stocks.
- Let a local Claude connector read the ledger without adding or editing measurements.

## Start your own copy

1. Use GitHub's **Use this template → Create a new repository**. The main branch is sufficient for the current project.
2. Create your own Supabase project. Run `sql/01_the_table.sql` once in that new project's SQL editor. Do not rerun the setup on an existing populated database.
3. In Supabase Authentication → Users, add the user you will use to sign into Wire. For this manual setup, enable auto-confirm.
4. Create `config.js` at the repository root using `config.example.js`:

```javascript
window.WIRE = {
  url: 'YOUR-PROJECT-URL',
  key: 'YOUR-PUBLISHABLE-KEY'
};
```

5. Replace those placeholders with the URL and publishable key from your own project. Use only the publishable key in this browser file.
6. Import your GitHub repository into Vercel as a static site with no framework or build command.
7. Open the deployed website, sign in with the user from step 3, and add a real reading.
8. Open YOU and choose what better means for the metric.

Hosting plans and limits can change; check the services' current terms for your usage.

## Pages and files

| File | Purpose |
| --- | --- |
| `index.html` | Sign in |
| `pad.html` | Record weight |
| `import.html` | Import measurements from CSV |
| `you.html` | Stock cards, charts, and combined YOU index |
| `commit.html` | Start and end commitments |
| `test.html` | Compare one commitment against a stock |
| `scan.html` | Explore leads across stocks |
| `you-reader.js` | Shared calculations used by the website and connector |
| `nav.js` | Navigation links |
| `sql/01_the_table.sql` | Fresh database setup |
| `pull/github.mjs` | Import GitHub commit counts |
| `mcp/wire.mjs` | Local read-only MCP tools |

## Automatic GitHub data pull

In your GitHub repository, open **Settings → Secrets and variables → Actions**. Add these repository secrets:

| Name | Value |
| --- | --- |
| `WIRE_URL` | Your Supabase Project URL |
| `WIRE_KEY` | Your Supabase publishable key |
| `WIRE_EMAIL` | The email used to sign into your Wire website |
| `WIRE_PASSWORD` | That Wire user's password |
| `GH_TOKEN` | Your GitHub personal access token, with access appropriate to the repositories you intend to count |

Type secret names exactly, without spaces. Keep passwords and personal tokens out of repository files.

Open **Actions → pull → Run workflow** for the first check. Inspect the job's error log if it fails. A green check confirms the run completed.

The workflow is scheduled for 05:17 UTC daily, subject to GitHub's scheduling availability. It requests the previous 14 UTC days, excluding today, for the repository owner. A successful manual run does not prove a future scheduled run has occurred.

## Connect Claude Desktop

These are local desktop setup instructions. The connector's source must stay available on that computer.

1. Install Node.js, then download or clone your own complete Wire repository locally.
2. In Terminal, enter the `mcp` directory inside that copy. If a matching `package-lock.json` is supplied, run `npm ci --ignore-scripts`; otherwise run `npm install --ignore-scripts` and retain the generated lockfile.
3. On macOS, run `command -v node` and copy its output. This is the full Node path for `command` below.
4. Find the full path to `mcp/wire.mjs` in your local copy.
5. Open Claude Desktop's local MCP configuration using its Developer settings. On macOS the file is `~/Library/Application Support/Claude/claude_desktop_config.json`.
6. Back up the configuration. Add a `wire` entry inside its existing `mcpServers` object; preserve all other connectors and preferences. If that name is already in use, choose a distinct name for this copy.

This is a complete minimal configuration for a previously empty file. If your file already has settings, merge only the server entry rather than replacing the whole file:

```json
{
  "mcpServers": {
    "wire": {
      "command": "FULL-PATH-TO-NODE",
      "args": ["FULL-PATH-TO-YOUR-WIRE/mcp/wire.mjs"],
      "env": {
        "WIRE_URL": "YOUR-PROJECT-URL",
        "WIRE_KEY": "YOUR-PUBLISHABLE-KEY",
        "WIRE_EMAIL": "YOUR-WIRE-SIGN-IN-EMAIL",
        "WIRE_PASSWORD": "YOUR-WIRE-SIGN-IN-PASSWORD"
      }
    }
  }
}
```

7. Replace every placeholder. Use the project and user that match your website. Keep this local file private.
8. Save valid JSON, fully quit Claude Desktop, reopen it, and start a new Chat. Enable your Wire connector if needed.
9. Ask: **Use only my Wire connector. List my metric names and latest readings. Do not change any data.**

The tools are `stocks`, `history`, `commits`, and `did_it_work`. An empty ledger can correctly return an empty result. The connector's combined-index date does not by itself tell you whether every metric has newer readings.

For a startup timeout or the tested Mac Desktop-folder workaround, see [Update 002](COURSE-UPDATES.md#update-002--claude-desktop-setup--ep-10). For Windows paths and current host setup, see the [official local MCP guide](https://modelcontextprotocol.io/docs/develop/connect-local-servers).

## What changes when you add data

Readings are saved in Supabase. Open or refresh YOU to retrieve them and recalculate its display. An already-open dashboard does not poll for new readings. Ask Claude again for a fresh tool response.

A new metric appears under **Not scored yet** until you choose a rule. Multiple readings on the same day share a daily average. A stock's displayed index needs 14 recorded days, not 14 entries.

The current combined YOU calculation can include a declared stock before that stock's individual index is displayed. Its first up-to-30 daily means are recalculated as data arrives, and the combined series can reuse a stock's latest index for up to seven days. Older backfills and changed rules can change calculated history while preserving the raw events. These behaviors remain part of the course consistency review; this README does not change the reader.

## Updating your project

Check [COURSE-UPDATES.md](COURSE-UPDATES.md), apply the relevant correction to your copy, run its check, and deploy through your existing GitHub/Vercel setup. Preserve your own configuration and custom work.

Your local Claude runtime is a separate copy. When an update affects the reader or MCP source, apply it to the files Claude actually runs and restart Claude. Publishing the website alone does not update the local connector.

## The course and the face

EP 0–10 are the plumbing build record. Comparison wording notes belong with EP 7; connector setup notes belong with EP 10. The [EP 0–10 lesson index](COURSE-UPDATES.md#ep-010-lesson-index) lists the episode titles and Whop links. Recorded footage is unchanged, and exact correction timestamps remain unreviewed.

The face is a presentation layer over the existing reads and writes. A visual redesign does not require recreating the database.

## Project boundaries

- Keep the events ledger append-only. Do not add update/delete policies or erase history.
- Do not add invented readings for demos or tests.
- Never put a service-role/secret key into the website or repository.
- Preserve calculation thresholds and refusal states while fixing setup or presentation.
- Keep the shared calculations in `you-reader.js`.

The main student flow has been exercised, including a real Claude Desktop tool conversation. Final checks remain for account isolation, insufficient-data cases, varied CSV files, date boundaries, large histories, scheduled runs, and the scoring consistency described above. See the updates page for the current state.

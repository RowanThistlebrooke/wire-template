# Make The Wire yours

**No videos required.** Use the completed template, connect your own database, and start with your own readings. The videos are an optional record of how it was built.

- **First time?** Follow sections 1 to 5 below, in order.
- **Already set up?** Go to [Use it](#6-use-it), [Make changes](#7-make-it-look-and-work-your-way), or [Updates](#9-keep-your-work-as-the-template-grows).
- **Always-current guide:** [bookmark this page in the original template](https://github.com/RowanThistlebrooke/wire-template/blob/main/START-HERE.md). The copy inside your own repo will not update by itself.
- **Corrections and known limits:** [current course updates](https://github.com/RowanThistlebrooke/wire-template/blob/main/COURSE-UPDATES.md).
- **Guide checked:** 13 September 2026 against template `2da1747`. This is a setup guide, not a claim that every feature or large dataset has been verified.

## 1. Know what you are setting up

- **GitHub** holds your website files. A repository, or repo, is the project's folder and its change history.
- **Supabase** holds your readings and your Wire sign-in account.
- **Vercel** puts your website online. You use that website day to day.
- Create your own accounts at [GitHub](https://github.com), [Supabase](https://supabase.com), and [Vercel](https://vercel.com). Check their current plans and limits before accepting charges.
- You do not need to code, install Node, or connect AI to set up the website. You do need an internet connection. Claude and automatic imports are optional later steps.
- This is the completed rules-based template. Do not paste earlier episode files over it or follow the build-from-empty route at the same time.

## 2. Get your own project

- Open [the template](https://github.com/RowanThistlebrooke/wire-template).
- Click **Use this template → Create a new repository**. Choose your personal GitHub account as owner, name it `my-wire`, and choose **Private** for your personal copy. The main branch is enough.
- Bookmark your new repo. From now on, make changes there, not in Rowan's template.
- Prefer this route over Download ZIP. A ZIP is only a snapshot, with no connection to future changes or your website deployment. If you already have a working copy, keep it.
- **Check:** the repo is under your account and contains `index.html`, `you.html`, `config.example.js`, and `sql/`.

## 3. Create your database and sign-in

- In Supabase, create a **new project**. Store its database password privately; it is not your Wire sign-in password.
- In your own repo, open `sql/01_the_table.sql` and copy the **whole file**.
- The included reporting day uses **Europe/Zurich with a 06:00 cutoff**. If you need your local reporting day, have your coding assistant review and adapt the date handling before saving any readings. Tell it your IANA timezone and cutoff. Do not silently change a populated project's day definition later.
- In the new Supabase project's **SQL Editor**, paste the complete setup and click **Run once**. It creates the ledger and ownership-based access rules without adding readings. Do not run it on an existing populated Wire database or disable row-level security to fix an error.
- Open **Authentication → Users → Add user**. Enter the email and password you want to use in Wire, and enable **auto-confirm** for this manual setup. Save them in your password manager.
- **Check:** the setup reports success and your new user is listed. The ledger being empty is correct.

## 4. Connect the website to your database

- In Supabase, use the project's **Connect** panel or project settings to copy its **Project URL** and **publishable API key**.
- In your own GitHub repo's top-level file list, click **Add file → Create new file**. Name it exactly `config.js`, alongside `index.html`, not inside another folder.
- Paste this complete file and replace both placeholders:

```javascript
window.WIRE = {
  url: 'YOUR-PROJECT-URL',
  key: 'YOUR-PUBLISHABLE-KEY'
};
```

- Click **Commit changes**. This saves the file to your repo.
- Only the Project URL and **publishable** key belong here. Never paste a secret/service-role key, any password, a personal token, or an AI key into a website file. A publishable key is designed for the browser; the database's access rules protect the rows.
- **Important for local editors:** `config.js` is ignored by Git by default. Creating it on your computer and doing a normal upload/commit will not necessarily deploy it. The GitHub browser steps above deliberately create the publishable-only file in your repo.
- **Check:** `config.js` is visible in your GitHub file list and contains your URL and publishable key, not placeholders. Adding Vercel environment variables alone does not fill this plain HTML file.

## 5. Put your Wire online

- In Vercel, choose **Add New → Project**, connect GitHub, and import **your** Wire repo.
- Use **Framework Preset: Other**, the repository root as **Root Directory**, no **Build Command**, no **Install Command**, and the root (`.`) as **Output Directory**. Leave empty-command overrides empty if the interface asks for an override. The website has no `npm run build` or `npm run dev` step; do not select the `mcp/` folder.
- Click **Deploy**, then open the resulting website address. Bookmark it.
- In Supabase **Authentication → URL Configuration**, set **Site URL** to that exact website address. This also gives future authentication emails the right destination.
- Sign in on your Wire website using the email/password from **Authentication → Users**, not your Supabase dashboard login or database password.
- **Check:** you reach Pad and can open YOU. An empty dashboard is a successful empty setup, not a reason to add fake data.

## 6. Use it

- **Pad:** save a genuine weight reading in **kilograms**. Pad is currently weight-only. If you do not want to track weight, skip it and import another real metric instead.
- **Import:** export your data as a CSV, then open Import and choose the file. It needs a header row, a date/time column, and numeric measurement columns. Apple Health XML is not directly supported.
- Before clicking Import, check the detected date column, the **from** prefix, metric names, and selected columns. Deselect IDs, scores you do not want, and other numeric fields that are not measurements. Keep each metric's name and unit consistent across future imports; unit conversion is not automatic. Imported units are not stored separately by the current importer, so include the unit in the column name when needed.
- Use unambiguous dates/timestamps with an explicit timezone where available. Ambiguous spreadsheet dates and date-only exports need review before import. Keep original exports; changing the filename, row order, or prefix can defeat repeat-import detection. Do not repeatedly retry a partially failed import without checking what was saved.
- **YOU:** open or refresh it after saving data. Under **Not scored yet**, choose **up**, **down**, **best between**, or **ignore** for each metric. Choose what matters to you; the template does not choose for you.
- **Commit:** record a real change and its start date, then end it when it stops. Give separate attempts distinct names. A Wire commitment is a life change, not a GitHub code commit.
- **Test:** select that commitment and a metric. Read the available-day counts and explanation. **Scan** explores more pairs and returns leads, not proof. A refusal or no finding is normal; do not lower the gate to get an answer.
- **Read the numbers correctly:** raw readings keep their units; index points are not percentages or a medical assessment. The individual index needs **14 recorded days**, not 14 entries. Comparison gates are separate. Current scoring and freshness caveats are in the [README](README.md#what-changes-when-you-add-data).
- **Check:** your genuine reading survives a refresh and shows under the intended metric. Never add a made-up reading just to test this.

## 7. Make it look and work your way

- Start small: change one page's wording, colours, spacing, or layout. Keep your data, sign-in, navigation, shared reader, and refusal states intact.
- For a new metric, prefer a real CSV import first. A new device/API connection needs its own reviewed importer; the template does not already connect every service.
- In your coding assistant, open **your own repo** and use this prompt. Replace the bracketed request:

```text
Read AGENTS.md, START-HERE.md, README.md and COURSE-UPDATES.md first.
This is my existing completed Wire project. I want: [one specific change].
Inspect my version before editing. Work on one file at a time.
Preserve my configuration, passwords, saved data and unrelated changes.
Do not print secrets, invent readings, run SQL, weaken access rules or gates,
or change scoring while doing a presentation change. Keep plain HTML and
the single shared reader. Do not add a framework or AI data-writing tools.
Explain the exact change and check. Show me the complete updated file if
I am using the GitHub browser editor. Wait for approval before publishing.
```

- Review the result before committing it to the branch Vercel deploys. Changes to that branch can publish automatically. For experiments, use a separate branch/preview and do not save test readings into your real ledger.
- Keep a private setup note with your repo URL, website URL, Supabase project name, and optional local connector folder. Keep passwords in a password manager, not that note or the repo.

## 8. Optional: automatic data and Claude

- **GitHub activity:** follow the [written automatic-pull steps](README.md#automatic-github-data-pull). Add the five named secrets in GitHub's Actions settings, then run the `pull` workflow manually. It counts the repository owner's GitHub commits, not all your apps. Keep the repo under your personal account for that default.
- **Claude Desktop:** follow the [written connector steps and complete config](README.md#connect-claude-desktop). Install **Node.js 22 or newer**, keep the **complete repo** on your computer, and run `npm ci --ignore-scripts` inside its `mcp` folder. Do not install website build tools or copy only `mcp/`.
- Use absolute paths and your own Wire account in Claude's local configuration. Preserve your other connectors, keep the file private, and restart Claude after setup.
- **Check:** ask Claude, "Use only my Wire connector. List my metric names and latest readings. Do not change any data." Confirm it uses the tool. An empty result is valid for an empty ledger.
- These integrations are optional. You can keep using the website without either of them. Full written steps are linked above; no episode is required.

## 9. Keep your work as the template grows

- Bookmark the **original template's** [latest guide](https://github.com/RowanThistlebrooke/wire-template/blob/main/START-HERE.md) and [numbered updates](https://github.com/RowanThistlebrooke/wire-template/blob/main/COURSE-UPDATES.md), not just the copies in your own repo.
- A GitHub template creates an independent project. Your repo, your deployed website, your database, and your local Claude connector do **not** all update together. There is no automatic safe sync or simple "Sync fork" step for a template copy.
- Before updating, save/commit your current work and record the template commit/update number you last applied in your private setup note. Keep any uncommitted files and private configuration backed up outside the published website. A GitHub ZIP is not a backup of your Supabase data.
- Read the update's affected files, prerequisites, and check. Apply only that change to **your existing copy**, preserving your design and settings. Ask your assistant to compare your files first, skip changes already present, and stop if the course stage does not match.
- Do not overwrite the whole project with a new ZIP, recreate the database, or rerun the fresh SQL. A future update needing a database change must provide a separate reviewed migration and backup instructions.
- Review and test the change on a preview before publishing through your existing deployment. For a code-only problem, revert the code change rather than resetting the ledger.
- If `you-reader.js` or MCP files changed, update the **local folder Claude actually runs** as well. Keep its credentials and other connectors, reinstall dependencies only if required, restart Claude, and request a fresh read-only response.
- Record the update number and result when its check passes. If it fails, keep your last working version and report the exact error with secrets removed.
- This keeps the instructions maintainable as features grow. It does **not** certify unlimited data volume: large-history queries, some CSV/date cases, scoring consistency, and wider isolation checks remain open in the updates page.

## If you get stuck

- **Cannot sign in:** check the exact error, confirm the Auth user exists in the same Supabase project as `config.js`, and use that user's password. Do not disable security or substitute a secret key.
- **Blank page / WIRE is not defined:** check whether the deployed `/config.js` exists and contains both replaced values. Do not share its contents if you accidentally put a secret there; rotate the exposed secret through its provider.
- **No metric or no index:** refresh YOU, check the import result and rule, and distinguish recorded days from entries. Do not manufacture days or fill gaps.
- **Permission error / CSV error / connector timeout:** save the exact error with credentials removed. Ask your assistant to inspect that error and the matching section of this guide before making changes. Do not repeatedly rerun setup or imports.

## References for changing service interfaces

- [GitHub: create a repository from a template](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-repository-from-a-template)
- [Supabase: API keys](https://supabase.com/docs/guides/getting-started/api-keys) and [authentication URLs](https://supabase.com/docs/guides/auth/redirect-urls)
- [Vercel: static build configuration](https://vercel.com/docs/builds/configure-a-build)
- [Local MCP server setup](https://modelcontextprotocol.io/docs/develop/connect-local-servers)

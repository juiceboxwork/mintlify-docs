# Juicebox Help Center — agent instructions

## About this project

- This is the Juicebox Help Center, built on [Mintlify](https://mintlify.com).
- Pages are MDX files with YAML frontmatter. Configuration lives in `docs.json`.
- Two tabs: **Documentation** (feature guides, at the repo root) and **Help Center** (task and troubleshooting articles, under `help/`).
- Screenshots live in `images/`.

## Audience

Write for a recruiter or sourcer who uses the Juicebox web app. Assume they know
recruiting. Do not assume they know software.

Every page answers one of two questions:

1. What is this feature, and when do I use it?
2. What steps do I take to do the thing?

If a paragraph answers neither question, delete it.

## Terminology

Use these terms. Do not invent synonyms.

| Use | Do not use |
| --- | --- |
| project | campaign, workspace, folder |
| search | query, search job |
| Agent | bot, automation, assistant |
| shortlist | pipeline, saved list |
| sequence | campaign, drip, outreach flow |
| criteria | requirements, must-haves |
| filters | facets, refinements |
| credits | tokens, units |
| candidate, profile | lead, record, result |
| team member | user, seat holder |
| organization | company, tenant, account |

Product facts to keep straight:

- A **project** is the top-level unit of work. Searches and Agents live inside a project. There is no standalone Agents page and no standalone search page.
- A project runs in one mode at a time: **Search** or **Agent**. Users switch between them.
- **Criteria** rank candidates. **Filters** narrow results by hard requirements. They are not the same. See [Filters vs. Criteria](/filters-vs-criteria).
- Plans are **Starter**, **Growth**, **Business**, and **Enterprise**.
- Roles are **Admin**, **Read-Only Admin**, **Licensed Seat**, and **Hiring Manager**.
- Support email is `support@juicebox.ai`. Sales email is `sales@juicebox.work`.

Capitalize **Agent** when it names the Juicebox Agent feature. Keep other feature
words lowercase in prose (`your shortlist`, `a new sequence`). Capitalize a word
only when you quote a label in the UI, and bold it when you do.

## Style

- Use active voice and second person. Write "you", not "the user".
- Write short sentences. Give one instruction per sentence.
- Use the simple present tense. Write "the list updates", not "the list will update".
- Use sentence case for headings. Write `## Create a sequence`, not `## Creating A Sequence`.
- Do not put bold markup in a heading. Write `## Agent overview`, not `## **Agent Overview**`.
- Start page headings at `##`. The frontmatter `title` is the H1.
- Bold UI labels: Click **Settings**.
- Join navigation steps with `>`: Go to **Account Settings** > **Team members**.
- Use numbered lists for steps the user follows in order. Use bullets for options.
- Use backticks for file names, paths, and email addresses only. Do not put UI labels in backticks.
- Link with root-relative paths and no file extension: `[Sequences](/sequences)`.

## Page structure

Frontmatter takes `title`, an optional `sidebarTitle`, and a `description`. Use
`sidebarTitle` when the full title is too long for the sidebar.

```mdx
---
title: "Search for Candidates in Juicebox"
sidebarTitle: "Search"
description: "Learn how to use Juicebox most effectively."
---
```

Use these components, and no others:

- `<Note>` for context the reader should have.
- `<Tip>` for a shortcut or a best practice.
- `<Warning>` for something that costs money, sends email, or cannot be undone.
- `<Frame>` around every screenshot.
- `<Card>` and `<CardGroup>` for landing pages only.

Never fake a callout with italics. Write `<Note>...</Note>`, not `_Note: ..._`.

## Base the change on the UI, not the backend

Write what the user sees on screen. Read the source repository to find it.

- Read the frontend code first: components, page routes, labels, menu items, empty states, and error messages.
- Copy UI text exactly as the code renders it. If the button says "Add to Shortlist", write **Add to Shortlist**. Do not paraphrase a label, and do not correct its capitalization.
- Trace where the control lives, then write the real path: which page, which tab, which menu. Do not write "in settings" when the code puts it under **Settings** > **Search**.
- Check what the user needs before the feature appears: a plan, a role, a connected integration, or a toggle. Say so on the page.
- A backend-only change usually needs no page edit. If the user cannot see it or act on it, skip it.

When the code shows a behavior but not its user-visible wording, do not guess at
the wording. Describe the outcome and flag the gap in the pull request.

## Screenshots

Wrap each screenshot in a frame and write real alt text:

```mdx
<Frame>
  ![Role options when inviting a team member](/images/help/account/roles-and-permissions-1.png)
</Frame>
```

**Reuse screenshots from the source pull request.** Product pull requests often
attach screenshots or screen recordings of the new UI. When one exists and it
matches the merged code, add it to the docs:

1. Confirm the image shows the shipped state, not an earlier draft in the pull request thread. Prefer the newest image.
2. Confirm it shows the Juicebox UI only. Skip anything with real candidate names, real email addresses, customer data, internal dashboards, or a browser with visible personal tabs.
3. Save it under `images/<section>/<descriptive-name>-<n>.png`, next to the images the page already uses. Follow the page's existing folder, for example `images/help/account/`.
4. Reference it with a root-relative path inside a `<Frame>`.
5. Say in the pull request description which source image you used.

Use a still frame from a recording only if it is legible on its own. Do not embed
video.

Never invent an image path. Never reuse an unrelated image to fill a gap. If no
usable screenshot exists, leave a marker and say so in the pull request
description:

```mdx
{/* TODO: screenshot needed — <describe the screen> */}
```

If a screenshot on the page now contradicts the change you are making, flag it in
the pull request description. Do not delete it and leave the section bare.

## Content boundaries

These docs describe the product as the user sees it. They are not a design doc.

Never write about:

- Internal mechanics: database writes, caches, queues, background jobs, transactions, or "atomic" behavior.
- Retry logic, polling intervals, or timing promises such as "retries hourly" or "about an hour later".
- Internal or third-party API versions and identifiers, unless the user must handle them.
- Feature flags, rollout gates, defaults set in code, or staged releases.
- Admin-only internal tooling, or features that have not shipped.

Describe what the user sees, what they click, and what happens as a result.

Do not write:

> Turning the setting off hides photos from your view only. Juicebox continues to
> fetch and store photos in the background so exports and teammates who have the
> setting on are unaffected.

Write:

> Turning the setting off hides photos for you only. Your teammates keep their own
> setting, and exports are not affected.

Do not write:

> Activating the v3 connection atomically replaces the v1 connection. The old v1
> credentials are removed in the same write, so there is no window where both
> integrations are active.

Write:

> The new connection replaces the old one. You do not need to disconnect the old
> integration first.

If a code change touches nothing the user can see, do not open a documentation
pull request for it.

## Editing rules

- Prefer an edit to an existing page over a new page. Check for an existing section first.
- Add a new page only when the topic has no reasonable home. Then add it to the correct group in `docs.json`.
- If you rename or move a page, add a `redirects` entry in `docs.json` from the old path.
- Change only what the code change requires. Do not reformat or rewrite nearby sections.
- Match the surrounding page's tone and heading depth, even where the repo is inconsistent.
- Keep each pull request to one topic.

## When you are unsure

State the uncertainty in the pull request description. Do not guess at behavior,
plan availability, or role permissions. It is better to leave a gap and flag it
than to publish a confident wrong answer to customers.

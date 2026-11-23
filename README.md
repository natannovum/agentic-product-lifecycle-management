# Agentic Product Lifecycle Management

A product-lifecycle management system for software development with AI coding agents.

**The problem it solves.** An AI coding agent forgets everything between sessions. The code shows what is true now, and version control shows when it changed. Neither shows why a decision was made, what was tried and rejected, which assumption turned out to be wrong, or how a change was checked. That knowledge lives in a chat transcript that gets thrown away, so the next session starts from nothing and repeats old mistakes.

Three pillars:

- **Capture** — Write down what is learned while building, as the work happens. Your agent-instruction file (`CLAUDE.md` or similar) already says *how* you and the agent work together. This system records what you learn *about the product*: why a design is the way it is, what alternatives were rejected, how each part works today and how it used to work, and the traps where the code misleads a careful reader. That knowledge has no natural home, so it is lost by default.
- **Protect** — Check every change with an automated pass/fail test that the agent cannot argue its way around, and catch regressions before they ship.
- **Propagate** — Keep the website, user manual and store listings in step with each release. This is the pillar most often skipped, because it is nobody's job and nothing breaks when it is neglected.

---

## What it does

| Area | What the system improves |
|---|---|
| **Development** | Automates documentation of product changes and implementation specs, including the rationale for each decision and the alternatives that were rejected. Anyone on the team can retrace the reasoning, and no one wastes time retrying solutions already shown not to work. Every change also records how it was verified, so results can still be trusted months later. |
| **Product marketing** | Keeps website copy, store listings, demo environments and media inventories consistent with each release, and records which destinations were updated. Published claims stop drifting quietly away from what the product actually does. |
| **User documentation** | Checks at every pull request whether a product update changes any of the claims in the end-user support documentation, and updates the manual's content to match the latest implementation. The manual keeps matching the product users actually have. |
| **Tooling and infrastructure** *(optional)* | Gives test setups, servers, credentials and deployment pipelines their own record. All details are documented in one place (credentials by name and location only, never values), which unblocks anyone on the team who needs the information and ensures no critical knowledge is lost when developers leave the project. |
| **The documentation system itself** | Records changes to the Agentic Product Lifecycle Management (APLM) records and the human-agent collaboration rules (e.g., `CLAUDE.md`) to ensure the process stays consistent from one session to the next, even with multiple team members. |

### What you get

The files below live in `docs/` unless noted.

#### Development

| File | Purpose |
|---|---|
| `change-log-product.md` | Product changes grouped by release. Each entry states the goal, the mechanism, the assumptions that proved wrong, and how it was verified. |
| `todo-product.md` | Pending product work. Either an index into your issue tracker or the queue itself. |
| `documentation-dev/latest-implementation-specs.md` | The current implementation specification of each subsystem. |
| `documentation-dev/retired-implementation-specs.md` | Superseded specifications, kept with a note on what replaced them and why. |
| `documentation-dev/design-intentions-and-rationale.md` | The design, product and technical rationale behind decisions, including alternatives that were rejected. |
| `documentation-dev/codebase-tricky-parts.md` | Cases where the code misleads a careful reader, each led by its symptom. |
| `dev-log.md` | One narrative entry per working session, across all areas: what was asked, what happened, what went wrong, what was left undone. |

#### Product marketing

| File | Purpose |
|---|---|
| `change-log-product-marketing.md` | Changes to published copy and media, including which destinations each change was pushed to. Also records each comparison against what is live, including "no drift". |
| `todo-product-marketing.md` | Pending marketing and listing work. |
| `product-marketing/website/` | Website copy, as the source of truth. |
| `product-marketing/store-listings/<store>-<channel>/` | One folder per store and channel: the listing text, the required declarations (permissions, data collected, pricing, platforms), and an inventory of screenshots and promo assets with what each shows. |
| `product-marketing/_shared/` | Copy used by more than one destination. |

The kit tracks media as an inventory of what exists and what each item shows. It does not prescribe where the media files themselves are stored.

#### User documentation

| File | Purpose |
|---|---|
| `documentation-user/` | The source of the end-user manual: setup, per-feature guides, troubleshooting, and a plain explanation of any permissions requested or data collected. If the manual is published as part of the website, it lives in `product-marketing/user-manual/` instead. |

Changes to the manual are recorded in the product marketing change log.

#### Tooling and infrastructure *(optional)*

| File | Purpose |
|---|---|
| `change-log-tooling-and-infrastructure.md` | Deployments, test-setup changes, credential rotations and server rebuilds. |
| `todo-tooling-and-infrastructure.md` | Pending infrastructure work. |
| `documentation-dev/infrastructure-and-repo-model.md` | Environments, which branch reaches which environment, hosting, the release pipeline, where credentials live (by name only), and which steps are manual. |

#### The documentation system itself

| File | Purpose |
|---|---|
| `change-log-documentation.md` | Changes to the records and rules themselves. |
| `todo-documentation.md` | Pending work on the records and rules. |
| `CLAUDE.md` (repository root) | The agent's working rules: approval tiers, secret handling, the verification principles, the release process, and the end-of-session checklist. |

### Stack and workflow compatibility

- **Issue tracking:** Works with Linear, Jira, GitHub Issues, Notion, Trello or similar. The tracker owns pending work, and the repository keeps a short index so a fresh checkout still shows what is outstanding. With no tracker, markdown to-do files hold the queue instead. That suits small projects but needs periodic clean-up, because a markdown file cannot close or archive items.
- **Documentation:** Everything is plain markdown in the repository, versioned and reviewed like code. Setup reads an existing `CLAUDE.md`, `AGENTS.md` or `CONTRIBUTING.md` first and carries its rules over. You keep only the areas your project has.
- **Technology:** Independent of language, framework and hosting. Setup reads your CI configuration and manifests to adapt to your stack. The templates name the rules file `CLAUDE.md`, and `system.md` treats an equivalent agent-instruction file as interchangeable.
- **Publishing:** By default the agent drafts the updated copy and a human publishes it. Publishing directly through an API is possible, but the kit treats it as an action that needs your explicit approval.
- **Branches and releases:** Setup keeps your existing branch naming and release process. The default is semantic versioning, with version bumps decided by a human.

---

## What's in this repository

| File | What it is |
|---|---|
| `system.md` | **Read first.** What the system is, the full folder structure, and the reasoning behind each structural decision. It describes; it sets no rules. |
| `setup-protocol.md` | The step-by-step procedure for setting the system up in a project: what to read from the repository, what to ask you, and in what order. Written for the agent doing the installing. |
| `templates/CLAUDE.md` | The agent's day-to-day rules. General rules are filled in; project-specific sections carry their own setup instructions. |
| `templates/docs/` | Sample log, to-do and reference files. Each holds example content marked for deletion at first real use. |

---

## Installing it

Point a coding agent at this repository and ask it to follow `setup-protocol.md`:

```
Read https://github.com/natannovum/agentic-product-lifecycle-management
and follow setup-protocol.md to set the system up in this project.
```

Or clone it and work locally:

```bash
git clone https://github.com/natannovum/agentic-product-lifecycle-management
```

**Only `templates/` gets copied into your project.** `system.md` and `setup-protocol.md` are background reading. Leave them here and link to them, rather than copying them in, because a copy drifts out of date.

The short version of what the agent will do:

1. **Secrets first.** Decide which files and commands could expose credentials, and block them. This is the one step that cannot be fixed after the fact.
2. **List what cannot be undone in this project**, such as deploying, publishing or deleting live data. This decides which actions need your approval. It has no safe default, so the agent will ask you.
3. **Decide which areas to track** and whether pending work lives in an issue tracker or in markdown files. This determines which files get created.
4. **Copy the templates in**, leaving out anything the project does not have.
5. **Fill in the remaining sections**, using what the repository can answer before asking you.
6. **Do not backfill.** Start the logs from today.

The protocol's guiding rule is **check what is already written down, then what the repository can tell you, and only then ask**. Rules you have already written are already agreed. Build configuration and manifests are authoritative and free to read. Your attention is the scarcest input, so it is spent last.

---

## How the records fit together

Records are split two ways: by **area** (product, product marketing, documentation) and by **state** (finished or pending).

- **Finished work** goes in change logs. New entries go on top, and old entries are never edited.
- **Pending work** goes in an issue tracker if you have one, with a short index file in the repository so a fresh checkout still shows what is outstanding. If you have no tracker, the markdown to-do files hold the queue themselves. That works, but it is a fallback rather than the intended design.
- **Above both** sits one session log that tells the story, because the first question anyone asks is "what happened and where are we?", not "what changed in this area?".
- **Beside both** sits reference documentation that describes how things are now. It is rewritten in place when things change, unlike the logs, which are history and are never rewritten.

`system.md` explains why each of these is the way it is, and is open about the four places where the design is weak.

---

## Adapting it

Every project-specific decision is marked and has a stated default. Three deserve real thought rather than quick acceptance:

- **What can't be undone here.** An incomplete list is worse than none, because it looks authoritative.
- **Whether pending work has a real home.** Markdown to-do files get harder to use over time. This comes from the format and is not a sign of carelessness.
- **Which published surfaces exist, and whether any is currently edited outside the repository** (for example directly in a website dashboard). That reverses the rule that the repository is the source of truth, so raise it as a decision rather than accepting it by default.

Where a rule depends on project specifics, the specifics are asked for rather than assumed.

---

## Contributing

Issues and pull requests are welcome.

This system is developed **in use**. It lives inside a working product repository and is mirrored here with `git subtree`, so every refinement comes from having actually needed it. The parts that are specific and opinionated are the parts that were paid for by getting something wrong first.

If you adopt it and something does not fit, that is worth an issue. The defaults in `setup-protocol.md` are judgment calls, and any that turn out to be wrong for a common project shape should change.

## License

MIT. See [`LICENSE`](LICENSE).

---

## How this repository is published

It is not edited here. It is a **`git subtree` mirror** of `docs/agentic-product-lifecycle-management/` in the private Neutrino repository, pushed by `.github/workflows/publish-agentic-PMS.yml` on every merge to `dev` that touches that directory.

Pull requests against this repository are welcome and are merged back the other way. See the workflow's header for the `git subtree pull` command. If a push is rejected as non-fast-forward, do not force it, because that discards the contribution.

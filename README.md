# How much of the editor plugin ecosystem is abandoned?

**A measured survey of 2,499 VS Code extensions and 7,152 Obsidian plugins. September 2026.**

Everyone knows some extensions are unmaintained. Nobody had put a number on it, so I measured it — and then measured the thing that actually matters, which is what happens to the users afterwards.

Short version: **59.5% of the top 2,499 VS Code extensions have shipped nothing in two years**, and those extensions account for **1.54 billion installs**. And across **223 officially deprecated extensions**, whether users end up on the replacement turns almost entirely on one thing: whether the original author set a pointer to it.

All data in [`data/`](data/) as CSV. Method and limitations at the bottom. Reproduce or correct anything you like.

---

## 1. Most installed extensions are not being maintained

2,499 VS Code extensions, sampled by install count (descending), **5.58 billion combined installs**:

| No new release in | Extensions | % | Installs affected | % of installs |
|---|---:|---:|---:|---:|
| 12 months | 1,744 | 69.8% | 2,174,980,120 | 39.0% |
| 18 months | 1,616 | 64.7% | 1,915,049,194 | 34.3% |
| **2 years** | **1,488** | **59.5%** | **1,543,898,546** | **27.7%** |
| 3 years | 1,282 | 51.3% | 1,144,735,998 | 20.5% |
| 5 years | 867 | 34.7% | 603,627,744 | 10.8% |

**More than a third of the top extensions have shipped nothing in five years.**

The percentages are lower when weighted by installs (27.7% vs 59.5% at the two-year mark), which is the one piece of good news here: the very largest extensions are better maintained than the median. But "better" is relative — see §2.

A caveat worth stating early: a stale release date is not automatically a problem. A snippets pack or a colour theme has no API surface to rot, and can sit untouched for years while working perfectly. What makes staleness dangerous is staleness *plus an API dependency* — and that is exactly where the next section lands.

## 2. 166 extensions are formally dead and still installing

These are extensions whose GitHub repository the owner has **archived** — an explicit, deliberate "this is over" signal. They remain listed and installable, and collectively they hold **240,249,375 installs**.

| Installs | Last release | Rating | Extension |
|---:|---:|---:|---|
| 62,285,541 | 22 mo | 3.9 | `VisualStudioExptTeam.vscodeintellicode` |
| 46,734,908 | 22 mo | 3.8 | `VisualStudioExptTeam.intellicode-api-usage-examples` |
| 10,998,320 | 61 mo | 4.2 | `msjsdiag.debugger-for-chrome` |
| 9,208,172 | 19 mo | **2.1** | `ms-vscode.azure-account` |
| 7,641,919 | 47 mo | 3.6 | `eg2.vscode-npm-script` |
| 6,746,070 | 53 mo | 4.5 | `CoenraadS.bracket-pair-colorizer-2` |
| 4,578,519 | 53 mo | 3.0 | `ms-vscode.vscode-typescript-tslint-plugin` |
| 3,882,342 | 68 mo | 4.1 | `rebornix.Ruby` |

Note who is at the top of that list. The largest formally-archived extensions are **Microsoft's own**. This is not a story about hobbyists losing interest.

`ms-vscode.azure-account` is the sharpest case: 9.2 million installs, archived, and a **2.1 rating**. That rating is users telling each other it is broken, in the only channel available to them, on a listing nobody is reading any more.

## 3. A fifth of dead extensions cannot legally be rescued

Of the 1,443 stale extensions with a resolvable GitHub repository:

| | Count | Share |
|---|---:|---:|
| MIT | 912 | 63.2% |
| **No licence file at all** | **274** | **19.0%** |
| NOASSERTION (unrecognised) | 101 | 7.0% |
| GPL-3.0 | 54 | 3.7% |
| Apache-2.0 | 46 | 3.2% |

**"No licence" means all rights reserved.** Publishing to a marketplace is not a grant of rights. Without a licence, nobody can legally fork, patch, or republish the code, no matter how many people it is failing.

Cross-referencing that against extensions dead for 2+ years gives the set that is genuinely stuck: **265 extensions, 129,472,328 installs, legally unrescuable.**

| Installs | Dead for | Extension |
|---:|---:|---|
| 13,216,621 | 97 mo | `techer.open-in-browser` |
| 6,512,799 | 29 mo | `burkeholland.simple-react-snippets` |
| 5,062,728 | 112 mo | `austin.code-gnu-global` |
| 4,823,019 | 69 mo | `hollowtree.vue-snippets` |
| 3,967,890 | 113 mo | `lonefy.vscode-JS-CSS-HTML-formatter` |

The last one is worth dwelling on: **4 million installs, untouched for nine and a half years, a 1.7 rating across 241 reviews, 117 open issues, and no licence.** It is simultaneously the most-wanted rescue on this list and an impossible one.

If you maintain something and ever intend to walk away, **adding a LICENSE file is the single highest-leverage thing you can do for your users.** It costs one commit and it is the difference between someone being able to rescue your work and not.


**A note on `NOASSERTION`.** GitHub reports 101 stale extensions this way — a licence file it cannot match to a known SPDX identifier. I checked the 14 largest by hand: 13 are custom but permissive (mostly Microsoft's own wording). One is not.

`mhutchie.git-graph` — 15,174,235 installs, last released April 2021, and a **4.94 rating across 675 reviews** — carries an MIT-like licence with a single clause changed:

> Permission is **NOT GRANTED to publish, distribute, sublicense, and/or sell derivative works** of the Software.

Personal forking is allowed; publishing a derivative is not. There is an actively maintained fork ([hansu/vscode-git-graph](https://github.com/hansu/vscode-git-graph), 288 stars, last pushed May 2026) and it is **not on the Marketplace** — which the licence would explain. A [deprecation proposal](https://github.com/mhutchie/vscode-git-graph/issues/769) with 27 reactions is open on the repository.

This is the one case in the sample where a beloved, heavily-used extension is stalled *and* the usual remedy is contractually unavailable. It is not a pattern — one in fourteen — but it does mean `NOASSERTION` should be read as "unknown, check it" rather than "probably fine."

## 4. When an author points at a replacement, users follow. When nobody does, they don't.

This began as three case studies. It is now a systematic measurement, and the systematic version **corrected my first conclusion** — the original text is preserved in git history.

VS Code publishes a [control manifest](https://main.vscode-cdn.net/extensions/marketplace.json) listing every officially deprecated extension. It contains **507 deprecations, 339 naming a specific replacement extension.** Of those, 223 have an original with 1,000+ installs and both sides resolvable — a real sample rather than anecdotes.

**Capture rate** = the replacement's installs as a share of the deprecated original's.

| Group | n | Median capture | Left below 10% |
|---|---:|---:|---:|
| Same publisher (a true successor product) | 56 | **128.1%** | 2% |
| Different publisher, comparable scale | 122 | **125.6%** | 10% |
| *Different publisher, replacement >10× original* | *45* | *5208.5%* | *0%* |
| All pairs | 223 | 184.4% | 6% |

**I excluded that third group from the conclusion.** Those are cases where the named replacement is a large product that existed independently — `vscodeintellicode` → `github.copilot-chat`, `jshint` → `eslint`, `donjayamanne.jupyter` → `ms-toolsai.jupyter`. Copilot Chat's 78 million installs are not IntelliCode users migrating. Including them would inflate the headline into meaninglessness.

On the 178 pairs that survive, the median replacement ends up with **slightly more lifetime installs than the extension it replaced**, and only about one in ten leaves users stranded.

**Contrast that with the no-pointer case.** `calendar-plus` is an actively maintained alternative to an Obsidian plugin that has been dead four years, with weekly commits and a proper directory listing — and no relationship to the original. It has **4,221 installs against the original's 3,059,654: 0.1%.**

So the finding, stated carefully:

> An official deprecation pointer is associated with users ending up on the replacement. Its absence is associated with the replacement being invisible. The gap between the two is roughly three orders of magnitude.

**What I originally wrote here was that rescue forks capture ~0.1% and that this is typical.** That was wrong, and wrong in an important way. 0.1% is what happens *without* a pointer — and I have exactly one clean measurement of that case, against 178 of the pointer case. The direction is well evidenced; the no-pointer side is not, and I would not want anyone citing it as though it were.

**The honest limitation:** installs are cumulative and never decrease, so a 125% ratio means the replacement has accumulated more installs over its lifetime — not that those specific users migrated. A replacement that has simply existed longer accumulates more. This measures association, not migration, and no public data distinguishes the two.

Two practical consequences that do survive all of that:

**If you are deprecating something:** set the replacement pointer. It is the difference between the two regimes above, it takes one request, and 166 archived-repo extensions holding 240 million installs have not done it.

**If you are forking something abandoned:** getting the original author to point at you is worth more than the code. Ask first. If they are unreachable, plan for the low end.


### The other half: rescues nobody can find

The pointer side of that comparison had 178 pairs. The no-pointer side had one, which I flagged as the weakest thing in this report. So I went and measured more.

Taking the 300 most-installed extensions with no release in 2+ years, I looked for GitHub forks that are genuinely alive — 10+ stars and pushed within roughly the last 18 months. **Ten of them have one**, together holding **55.5 million installs**. Where that fork is also published to the Marketplace, the capture rate can be measured directly, with no official pointer anywhere:

| Abandoned extension | Installs | Dead | Published rescue | Rescue installs | Capture |
|---|---:|---:|---|---:|---:|
| `mhutchie.git-graph` | 15,174,235 | 64 mo | `hansu.git-graph-2` | 1,261 | **0.01%** |
| `Gruntfuggly.todo-tree` | 7,753,772 | 40 mo | `FanaticPythoner.better-todo-tree` | 28,001 | **0.36%** |
| `aaron-bond.better-comments` | 10,587,085 | 49 mo | `edwinhuish.better-comments-next` | 53,662 | **0.51%** |
| `dracula-theme.theme-dracula` | 10,886,704 | 25 mo | `mathcale.theme-dracula-refined` | 173,416 | **1.59%** |
| `stkb.rewrap` | 894,205 | 54 mo | `dnut.rewrap-revived` | 52,910 | **5.92%** |
| `felixfbecker.php-intellisense` | 5,845,164 | 80 mo | `zobo.php-intellisense` | 6,043,413 | **103.39%** |

**Median 1.05%.** Five of six below 6%. None of these six appears in the deprecation manifest, so none has an official pointer.

Against a median of ~125% where a pointer exists, that is the clearest statement of the effect this report can make — with the caveat below.

**The outlier is real and I am not going to explain it away.** `zobo.php-intellisense` reached 103% with no official pointer at all. Notably, `felixfbecker/vscode-php-intellisense`'s *repository* is still active (last push August 2026) even though its Marketplace release is 80 months old — so "abandoned" is doing less work in that row than in the others, and PHP developers appear to have found the fork through ordinary community channels. A pointer clearly is not the only route. It is just the reliable one.

**`mhutchie.git-graph` is the sharpest case in the whole report.** 15.2 million installs, a 4.94 rating from 675 reviews, dead five years — and a rescue sitting at 1,261 installs, **0.01%**. Its [licence](https://github.com/mhutchie/vscode-git-graph/blob/master/LICENSE) forbids publishing derivative works, so the well-maintained 288-star fork cannot be listed at all, and the one that is listed reaches essentially nobody.

**Method caveat:** ten is a floor, not a total. I searched only the top 300 stale extensions and required 10+ stars and a recent push, so quieter rescues are certainly missed.


### Why the unpointed rescues stay invisible

I originally read the ~1% figure as a discovery failure — users would switch if only they knew. A developer on the [Git Graph deprecation thread](https://github.com/mhutchie/vscode-git-graph/issues/769) offered a simpler explanation, and it fits the data better:

> Frankly, none of this is important enough to stoke any real action. This extension is still functioning, so there isn't much drive to re-create a replacement. Bit by bit, VS Code has implemented the features natively... While other extensions in the marketplace are not nearly as pretty, they seem to cover the functionality.

That is almost certainly the mechanism. **Abandoned is not the same as broken.** An extension with no releases for five years keeps working until something in the API changes, so there is no moment that forces a user to look for an alternative — and the maintained fork stays at 1% not because it is hidden but because nobody is shopping.

It reframes what the archived-repo number means. Those 240 million installs are not 240 million people currently suffering. They are 240 million installs of software with nobody left to fix it *when* it eventually breaks. The cost is deferred, not present — which is exactly why it does not generate the pressure you would expect from the size of the number.

Two things still follow. Deferred cost is real cost, and the extensions most likely to break are the ones with the deepest API dependencies rather than the most installs. And for the 19% with no licence, the deferral is permanent: there is no fork available on the day it does break.

## 5. Obsidian, for comparison

Top 400 Obsidian community plugins by download count, 123,950,091 combined downloads:

| No commit in | Plugins | % | Downloads affected | % |
|---|---:|---:|---:|---:|
| 12 months | 132 | 33.0% | 25,662,942 | 20.7% |
| 2 years | 82 | 20.5% | 15,252,916 | 12.3% |
| 3 years | 22 | 5.5% | 1,718,207 | 1.4% |

**Obsidian's ecosystem is markedly healthier** — 20.5% stale at two years against VS Code's 59.5%. Plausibly because it is younger, smaller (7,152 plugins), and has a more concentrated community.

But the licence problem is identical: **22.0% of stale Obsidian plugins have no licence file**, against 19.0% for VS Code. Two ecosystems, different ages, different cultures, same one-in-five failure. This looks less like a community norm than a default that tooling never prompts anyone to fix.

## 6. What would actually help

**The mechanism already exists and is barely used.** VS Code has supported deprecating an extension *in favour of another extension or setting* since May 2022 — when set, the editor UI actively guides users to migrate. Microsoft's [Deprecated extensions discussion](https://github.com/microsoft/vscode-discussions/discussions/1) (73 upvotes, 518 comments) explicitly invites anyone to report an extension that should be marked.

That mechanism is precisely the one §4 measures as effective across 178 pairs: where an author sets a pointer, the median replacement ends up with more installs than the extension it replaced. It works. It is just not reaching the 166 archived-repo extensions holding 240 million installs, none of which are marked.

So the gap is not a missing feature. It is:

- **Deprecation isn't surfaced in the Marketplace.** Microsoft's own note says "for now the extension will not be rendered as deprecated in the Marketplace. Support for this will come later" — that was 2022. Users browsing the web listing, which is how most people choose extensions, see nothing.
- **Nothing detects the obvious signals.** An archived GitHub repository is a machine-readable, unambiguous "this is over" from the author. 166 extensions have one. That could be flagged automatically instead of waiting for a maintainer to remember to file a request.
- **Add a LICENSE file.** One commit. It decides whether your work can outlive your interest in it, and 19% of stale extensions have made themselves unrescuable by skipping it.

## Method

- **VS Code:** 2,499 unique extensions via the public Marketplace `extensionquery` API (25 pages × 100, sorted by install count descending), collected 2026-09-01. Extensions with a resolvable GitHub URL and no release in 18+ months (n=1,486) were enriched via the GitHub API for last push, archive status, licence, open issues and stars; 1,443 resolved.
- **Obsidian:** the full community registry (`community-plugins.json`, 7,152 plugins) and official download stats (`community-plugin-stats.json`), joined to GitHub metadata for the top 400 by downloads.
- **Capture rates:** VS Code's public extension control manifest (`https://main.vscode-cdn.net/extensions/marketplace.json`, 507 deprecations, 339 with a named replacement extension), joined to Marketplace install counts. 223 pairs had an original with 1,000+ installs and both sides resolvable. Obsidian counters from `community-plugin-stats.json`.
- "Months" = time since the last *marketplace release* for VS Code, and since the last *repository commit* for Obsidian. Both are given per-row in the CSVs.

## Limitations — please read before citing

- **The VS Code sample is the top 2,499 by installs, not the whole marketplace** (~100,000 extensions). It is deliberately biased toward the popular end. The long tail is very likely worse, not better; do not read these as marketplace-wide rates.
- **Install counts are cumulative and never decrease.** They measure installs ever performed, not active users. An extension with 60M installs does not have 60M current users, and there is no public active-user figure.
- **A stale release date is not proof of abandonment.** Some extensions are simply finished. Archive status and rating are better signals, which is why §2 leads on them.
- **Capture rate is an association, not a demonstrated migration.** Installs are cumulative and never decrease, so a replacement with more installs may simply have existed longer. There is no public active-user or migration data.
- **The no-pointer sample is six cases, not a systematic census** — found by searching forks of the 300 most-installed stale extensions with a 10-star / recent-push filter. Five of six sit under 6%; one reaches 103%. The pointer side has 178 pairs. Treat the contrast as strong but not settled.
- Repos were matched from marketplace metadata; extensions without a GitHub link are absent from the licence analysis.
- Single snapshot, one day. No time series.

## Data

- [`data/vscode-extensions.csv`](data/vscode-extensions.csv) — 2,499 rows
- [`data/obsidian-plugins.csv`](data/obsidian-plugins.csv) — 400 rows
- [`data/deprecation-capture-rates.csv`](data/deprecation-capture-rates.csv) — 223 deprecated→replacement pairs with install counts and confound flags
- [`data/unpointed-rescues.csv`](data/unpointed-rescues.csv) — 6 measurable rescues with no official pointer

Public data, public APIs. CC0 — use it for anything, no attribution required, though a link back is appreciated. Corrections and pull requests welcome; if you find an error I would rather know.

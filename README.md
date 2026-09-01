# How much of the editor plugin ecosystem is abandoned?

**A measured survey of 2,499 VS Code extensions and 7,152 Obsidian plugins. September 2026.**

Everyone knows some extensions are unmaintained. Nobody had put a number on it, so I measured it — and then measured the thing that actually matters, which is what happens to the users afterwards.

Short version: **59.5% of the top 2,499 VS Code extensions have shipped nothing in two years**, and those extensions account for **1.54 billion installs**. When someone forks an abandoned extension to rescue it, they typically inherit **0.1%** of its users — unless the original author points at them, in which case the fork can overtake the original entirely.

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

## 4. The finding that surprised me: rescues almost never work

This is the part I had not seen measured anywhere, and it changed my conclusions.

The intuitive model is that a maintained fork inherits the abandoned original's users. It does not. Capture rate — the fork's installs as a share of the original's:

| Original → replacement | Original | Replacement | Capture |
|---|---:|---:|---:|
| `calendar` → `calendar-plus` *(independent alternative)* | 3,059,654 | 4,221 | **0.1%** |
| `obsidian-full-calendar` → `full-calendar-remastered` *(community handover)* | 457,141 | 39,218 | **8.6%** |
| `liximomo.sftp` → `Natizyskunk.sftp` *(original author linked the fork)* | 1,351,026 | 1,527,565 | **113%** |

The three cases differ by nearly four orders of magnitude, and the variable is not code quality, marketing, or marketplace listing. `calendar-plus` is actively maintained with weekly commits and is properly listed in the directory. It has 0.1%.

**What separates them is whether the original points at the replacement.** `liximomo` deprecated their extension and linked Natizyskunk's fork; that fork now has *more* installs than the original ever did. The community handover got 8.6%. The independent rescue, with no relationship to the original, got 0.1%.

Two consequences:

**For users:** you are almost certainly still running abandoned extensions, and the maintained alternative — if one exists — is invisible to you. Nothing in the marketplace surfaces it. Discovery of replacements is effectively broken.

**For anyone considering a rescue fork:** your fork's reach is determined almost entirely by whether you can get the original author to acknowledge it. Ask them *first*. If they are unreachable, expect 0.1%, and decide whether the work is still worth doing on those terms. It might be — but go in knowing the number.

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

- **Marketplaces should surface maintenance status.** The data is already public — last release, commit recency, whether the repo is archived. VS Code's own [issue #177795](https://github.com/microsoft/vscode/issues/177795), "Ability to mark abandoned extensions", is still open. Right now the recommended way to check is to inspect the listing by hand.
- **Archived repos should be flagged on the listing.** 166 extensions have an unambiguous, machine-readable "this is over" signal from the author that the marketplace does not show to the 240 million people who installed them.
- **Deprecation should support a pointer.** The SFTP case shows a maintainer redirect transfers an entire userbase. Making that a first-class feature would fix replacement discovery outright.
- **Add a LICENSE file.** One commit. It decides whether your work can outlive your interest in it.

---

## Method

- **VS Code:** 2,499 unique extensions via the public Marketplace `extensionquery` API (25 pages × 100, sorted by install count descending), collected 2026-09-01. Extensions with a resolvable GitHub URL and no release in 18+ months (n=1,486) were enriched via the GitHub API for last push, archive status, licence, open issues and stars; 1,443 resolved.
- **Obsidian:** the full community registry (`community-plugins.json`, 7,152 plugins) and official download stats (`community-plugin-stats.json`), joined to GitHub metadata for the top 400 by downloads.
- **Capture rates:** official download counters from `community-plugin-stats.json` and the VS Code Marketplace, on 2026-09-01.
- "Months" = time since the last *marketplace release* for VS Code, and since the last *repository commit* for Obsidian. Both are given per-row in the CSVs.

## Limitations — please read before citing

- **The VS Code sample is the top 2,499 by installs, not the whole marketplace** (~100,000 extensions). It is deliberately biased toward the popular end. The long tail is very likely worse, not better; do not read these as marketplace-wide rates.
- **Install counts are cumulative and never decrease.** They measure installs ever performed, not active users. An extension with 60M installs does not have 60M current users, and there is no public active-user figure.
- **A stale release date is not proof of abandonment.** Some extensions are simply finished. Archive status and rating are better signals, which is why §2 leads on them.
- **Capture rate rests on three case studies**, not a systematic sample. The mechanism is a plausible reading of a very large effect, not a controlled result. It is the finding here most in need of replication, and I would welcome counterexamples.
- Repos were matched from marketplace metadata; extensions without a GitHub link are absent from the licence analysis.
- Single snapshot, one day. No time series.

## Data

- [`data/vscode-extensions.csv`](data/vscode-extensions.csv) — 2,499 rows
- [`data/obsidian-plugins.csv`](data/obsidian-plugins.csv) — 400 rows

Public data, public APIs. CC0 — use it for anything, no attribution required, though a link back is appreciated. Corrections and pull requests welcome; if you find an error I would rather know.

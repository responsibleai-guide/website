---
layout: base.njk
title: Managing AI Contributions
description: What every repo needs to handle AI contributions responsibly.
---

# Repo Checklist: Accepting AI-Assisted Contributions

**What every repo needs to handle AI contributions responsibly.**

---

## The Checklist

### 1. `AGENTS.md` in repo root

This file is read by AI coding agents (Copilot, Claude Code, Cursor, etc.) when they work on your codebase. It tells the AI what your standards are, what's off-limits, and how to behave. Think of it as onboarding instructions - but for machines.

**What to include:**

- Project description and mission context
- Tech stack and versions
- Commands to build, test, lint, and check security
- Project structure (where source, tests, and docs live)
- Relevant MADRs or architecture decision records (see [Thoroughly Document Architectural Decisions](#7-thoroughly-document-architectural-decisions)).
- Tech decisions and paths already explored and rejected, so AI does not keep retrying them.
- Critical paths where AI must not generate code unsupervised (auth, data handling, security)
- Acceptable areas for AI assistance (UI, boilerplate, test scaffolding, docs)
- Patterns to avoid (e.g. `eval()`, unparameterised queries, disabling security)
- Commit message conventions
- A reminder that the human is accountable

Keep it under 200 lines. AI context windows are finite - concise beats comprehensive.

### 2. AI section in `CONTRIBUTING.md`

A short, human-readable section that sets expectations for contributors. Link to the full org-wide policy if one exists, but keep the essentials in-repo so contributors don't have to chase links.

**Minimum content:**

```markdown
## AI Tool Usage

You may use AI tools to assist your contributions. You are fully responsible
for everything you submit.

- **Understand it**: You must be able to explain every line of your code.
- **Test it**: AI-generated code must pass all tests and security checks.
- **Disclose it**: Pick an AI assistance level (0-4) in the PR template.
- **Own it**: You are the author. If a reviewer asks "why?", you answer - not the AI.

AI tools must not be used to fix issues labelled `good first issue`.
These exist for human learning.

For full policy details, see: [AI-assisted coding guide](/ai-assisted-coding-guide/)
```

### 3. PR template with AI disclosure

Update `.github/PULL_REQUEST_TEMPLATE.md` to include an AI section. It should be lightweight - one checkbox and a few optional fields. Don't make it burdensome or people will skip it.

Ensure contributors are not penalized for using AI tools: honesty should be encouraged else [disclosure will stop][disclosure-penalty].

**Recommended template:**

```markdown
## What type of PR is this? (check all applicable)

- [ ] Feature
- [ ] Bug Fix
- [ ] Documentation
- [ ] Refactor
- [ ] Test
- [ ] Build or CI
- [ ] Other (please specify)

## Related Issue

Fixes #

## Describe this PR

A brief description of what this changes and why - in your own words.

## AI Tool Usage

How much of this PR was AI-assisted? (check one)

- [ ] **0** - No AI at any point
- [ ] **1** - AI helped me think, but I wrote all the code myself
- [ ] **2** - I planned the change and decided the approach; AI helped write the code
- [ ] **3** - AI planned and wrote it; I checked and approved each step as it went
- [ ] **4** - I set the AI going and left it to it; I reviewed the finished result

**If level 1 or above:**

- Tool(s) used:
- What was generated:
- What you reviewed and changed:

## Screenshots

If applicable.

## Alternative Approaches Considered

Did you consider other approaches? Why this one?

## Review Guide

How should a reviewer test this? Anything to watch for?

## Checklist

- [ ] I have read the Contributing Guide
- [ ] PR is focused and small
- [ ] Tests are included or updated
- [ ] I understand all code in this PR and can answer questions about it
- [ ] No secrets, credentials, or sensitive data are included
- [ ] Commit messages are descriptive
- [ ] Related docs and screenshots are updated

## [optional] What gif best describes this PR or how it makes you feel?
```

### 4. Commit message convention

Include a trailer for AI-assisted commits:

```
feat: add password strength indicator to registration

Assisted-by: Claude
```

Several projects use this convention (LLVM, Fedora, QGIS), though it is not a standard: [Kubernetes prohibits AI trailers][k8s-maintainership] and requires disclosure in the PR description instead. The main goal is to help maintainers decide on how to best review the code, without being punitive.

### 5. Maintain a solid CI pipeline

AI-generated code can appear correct while introducing subtle security issues, hallucinated dependencies, or untested paths.

CI pipeline tools can catch what human review misses:

- **Pre-commit hooks**: simple sanity and code quality checks can be run _even before a commit is made_.
- **Tests**: whatever tooling you already use to ensure ongoing functionality of code. Code coverage may also be assessed as a rough proxy.
- **Static code analysis**: Checkov for infrastructure code and Semgrep for application code.
- **Dynamic analysis**: Tools like zaproxy can be used to scan for various vulnerabilities in your web application, during runtime. This involves a bit more complexity than other analysis types listed here.
- **Code quality**: SonarQube Cloud is free for open source projects to use, assisting code quality and security compliance.
- **Dependency checking**: OWASP [DependencyCheck][dep-check] or [OSV Scanner][osv] can be used to ensure dependencies are updated to avoid latest security vulnerabilities. It's also recommended to use [Renovate bot][renovate] to regularly update dependencies.
- **Secrets scanning**: [GitLeaks][gitleaks] can be integrated as pre-commit hooks or a CI action to prevent the accidental commit of org secrets.
- **Licensing and copyright**: [ScanCode Toolkit][scancode] can be used to scan for copyright breaches in your code and non-compliance with license requirements.
- **Contributor agreement**: it's possible to add a GitHub workflow prompting a contributor to sign and agree to contribution terms within a new PR. This should filter out some bot contributions, and at least make human contributors think before continuing. Note that this is only a lightweight agreement to the contribution guidelines (and a bot/AI barrier) - it is **not** a Contributor License Agreement that assigns copyright or grants re-licensing rights. An example can be found [here][contributor-agreement-example].

### 6. Classify the Level of AI Assistance

The idea of grading assistance comes from the VisiData project: credit to them for [the original scale][visidata]. Theirs has 11 levels (0-10), which is a lot for a set of checkboxes, so the version below is adapted down to 5. Use their original if you want finer granularity.

| Level | What happened                                                                            | How to review it                                              |
| ----- | ---------------------------------------------------------------------------------------- | ------------------------------------------------------------- |
| **0** | No AI at any point.                                                                      | As normal.                                                    |
| **1** | AI helped me think - ideas, options, explaining things. I wrote all the code myself.      | As normal.                                                    |
| **2** | I decided the approach and planned the change. AI helped write the code.                 | As normal, plus check the AI-written parts - especially tests. |
| **3** | AI planned the approach and wrote the code. I checked and approved each step as it went. | Question the approach, not just the diff.                     |
| **4** | I set the AI going and left it to it. I reviewed the finished result, not the steps.     | Treat as untrusted third-party code. Full security review.    |

Two dividing lines to keep in mind: at 2 the human chose the approach, at 3 the AI did; at 3 someone watched each step, at 4 only the end result was checked.

Levels are not a quality score - a tested level 4 beats a sloppy level 0. They just tell a reviewer where to look. Penalising honest answers stops honest answers.

Optionally, add `ai-level-0` to `ai-level-4` labels to quickly surface this information to reviewers.

### 7. Thoroughly Document Architectural Decisions

- While LLMs are great at creating documentation from the current repo **code**, don't forget the importance of documenting decisions made throughout the project's development **by hand**.
- There is nothing worse than telling an LLM continually why to **not implement that way because xxx**, and having to iterate again (wasted resources).
- An ideal format to document decisions taken by the team is [Markdown Architectural Decision Format][madr].
- Even better, you could reference these files within `AGENTS.md`, providing an authoritative source for why particular decisions were taken over time and what constraints that creates for future implementations.

### 8. Identifying AI-assisted Code

- This is a tricky one. There are a few options out there, but of course there is huge corporate and academic interest in developing approaches for this.
- For now, identifying a PR as generated by an LLM tends to rely on heuristics developed on a per-person basis:
  - Including lots of dash characters ('em dash': `—` and 'box drawing light horizontal': `─`, but not the 'hyphen mius' standard key: `-`) and inline emojis (especially in logs).
  - Overly verbose commenting, particularly if it describes **what** code does on a line-by-line basis.
  - Unnecessary docstrings, for plainly obvious code functionality, e.g. a very simple function.
  - Strange variable names that you wouldn't typically see a human using.
  - Overly conformist and 'perfect' looking, lacking the messy or individual style of human developers.
- There are experimental approaches [source 1][zippy][source 2][detect-code-gpt] to automate this identification, however, strong caution should be used if integrating them into an automated pipeline as AI text detectors [misclassify the majority of non-native English writers as AI][detector-bias].
- There is also a worrying trend of fully automated bot 'agents' making PRs to open-source projects [source 1][bot-pr-backdoor]:
  - It's _generally_ possible to identify this as AI-generated _for now_ by asking questions about the code and checking responses.
  - Telltale signs of a bot account: frequency of PRs open across a large range of repos, number of forks made in a short space of time, integration with OpenClaw or other 'AI assistant' tools.
  - Perhaps a list of 'bot' accounts could be compiled and included in a CI action to flag PRs as AI?

### 9. Handling AI-assisted PRs (Maintainers)

**Key points for reviewers:**

- Read the declared [assistance level](#6-classify-the-level-of-ai-assistance) before reading the diff, and let it set how much scrutiny the change gets.
- If a PR is marked AI-assisted, ask "why this approach?" - the answer tells you if the contributor understands the code.
- Review for the same signs described in [Identifying AI-assisted Code](#8-identifying-ai-assisted-code).
- Use a standard response for non-compliant PRs (template below).
- If a contributor cannot answer basic questions about their code, the PR is not ready.
- If a contributor intentionally breaks rules laid out in the provided AI contribution policy, they may be subject to a 'ban' on future submissions (in the worst case, it is possible to block someone from interacting with organization or personal account repos).
- To measure AI's impact on your maintainers, track review time and outcomes rather than contribution counts. Most community health metrics [predate AI-speed contributions][chaoss-metrics] and now mostly measure how susceptible a project is to AI-assisted abuse.

**Response template for non-compliant PRs:**

> Thanks for this contribution. It doesn't currently meet our standards for AI-assisted work - please review our Contributing Guide and ensure you can explain the design decisions in this PR. Happy to help once you've had a chance to review the code more thoroughly.

---

## Quick Reference: What Goes Where

| File                               | Purpose                                | Audience     |
| ---------------------------------- | -------------------------------------- | ------------ |
| `AGENTS.md`                        | Machine-readable project standards     | AI tools     |
| `CONTRIBUTING.md` (AI section)     | Human-readable contribution rules      | Contributors |
| `.github/PULL_REQUEST_TEMPLATE.md` | PR disclosure and checklist            | Contributors |
| `docs/decisions`                   | Markdown Architectural Decision Records | Contributors |
| Commit trailer (`Assisted-by:`)    | Attribution in git history             | Maintainers  |
| Docs site policy page              | Full ethical framework and rules       | Everyone     |

[dep-check]: https://github.com/dependency-check/DependencyCheck
[osv]: https://github.com/google/osv-scanner
[renovate]: https://github.com/renovatebot/renovate
[gitleaks]: https://github.com/gitleaks/gitleaks
[scancode]: https://github.com/aboutcode-org/scancode-toolkit
[contributor-agreement-example]: https://github.com/hotosm/field-tm/blob/dev/.github/workflows/contribution-agreement.yml
[madr]: https://adr.github.io/madr/
[visidata]: https://www.visidata.org/blog/2026/ai/
[disclosure-penalty]: https://doi.org/10.1016/j.obhdp.2024.104405
[k8s-maintainership]: https://kubernetes.io/blog/2026/06/26/open-source-maintainership-in-the-age-of-ai
[zippy]: https://github.com/thinkst/zippy
[detect-code-gpt]: https://github.com/YerbaPage/DetectCodeGPT
[detector-bias]: https://arxiv.org/abs/2304.02819
[bot-pr-backdoor]: https://thehackernews.com/2026/08/claude-mythos-5-tried-to-backdoor-real.html
[chaoss-metrics]: https://nesbitt.io/2026/05/27/chaoss-metrics-in-2026.html

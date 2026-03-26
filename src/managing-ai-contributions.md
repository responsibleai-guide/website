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
- **Disclose it**: Mark AI-assisted PRs using the PR template.
- **Own it**: You are the author. If a reviewer asks "why?", you answer - not the AI.

AI tools must not be used to fix issues labelled `good first issue`.
These exist for human learning.

For full policy details, see: [AI-assisted coding guide](/ai-assisted-coding-guide/)
```

### 3. PR template with AI disclosure

Update `.github/PULL_REQUEST_TEMPLATE.md` to include an AI section. It should be lightweight - one checkbox and a few optional fields. Don't make it burdensome or people will skip it.

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

- [ ] No AI tools were used
- [ ] AI tools were used (complete below)

**If AI-assisted:**

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

This is becoming a de facto standard across open source (LLVM, Fedora, QGIS are all adopting similar conventions). It helps maintainers calibrate review attention without being punitive.

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
- **Contributor signature**: it's possible to add a GitHub workflow prompting a contributor to sign and agree to contribution terms within a new PR. This should filter out some bot contributions, and at least make human contributors think before continuing. An example can be found [here][cla-example].

### 7. Thoroughly Document Architectural Decisions

- While LLMs are great at creating documentation from the current repo **code**, don't forget the importance of documenting decisions made throughout the project's development **by hand**.
- There is nothing worse than telling an LLM continually why to **not implement that way because xxx**, and having to iterate again (wasted resources).
- An ideal format to document decisions taken by the team is [Markdown Architectural Decision Format][madr].
- Even better, you could reference these files within `AGENTS.md`, providing an authoritative source for why particular decisions were taken over time and what constraints that creates for future implementations.

### 8. Identifying AI-assisted Code

- This is a tricky one. There are a few options out there, but of course there is huge corporate and academic interest in developing approaches for this.
- For now, identifying a PR as generated by an LLM tends to rely on heuristics developed on a per-person basis:
  - Including lots of dash characters (—) and inline emojis (especially in logs).
  - Overly verbose commenting, particularly if it describes **what** code does on a line-by-line basis.
  - Unnecessary docstrings, for plainly obvious code functionality, e.g. a very simple function.
  - Strange variable names that you wouldn't typically see a human using.
  - Overly conformist and 'perfect' looking, lacking the messy or individual style of human developers.
- There are experimental approaches to automate this identification (that could be included in a CI pipeline):
  - https://github.com/thinkst/zippy
  - https://github.com/YerbaPage/DetectCodeGPT
  - Hugging Face LLM models for AI content detection.
- There is also a worrying trend of fully automated bot 'agents' making PRs to open-source projects:
  - It's _generally_ possible to identify this as AI-generated _for now_ by asking questions about the code and checking responses.
  - Telltale signs of a bot account: frequency of PRs open across a large range of repos, number of forks made in a short space of time, integration with OpenClaw or other 'AI assistant' tools.
  - Perhaps a list of 'bot' accounts could be compiled and included in a CI action to flag PRs as AI?

### 9. Handling AI-assisted PRs (Maintainers)

**Key points for reviewers:**

- If a PR is marked AI-assisted, ask "why this approach?" - the answer tells you if the contributor understands the code.
- Review for the same signs described in [Identifying AI-assisted Code](#8-identifying-ai-assisted-code).
- Use a standard response for non-compliant PRs (template below).
- If a contributor cannot answer basic questions about their code, the PR is not ready.
- If a contributor intentionally breaks rules laid out in the provided AI contribution policy, they may be subject to a 'ban' on future submissions (in the worst case, it is possible to block someone from interacting with organization or personal account repos).

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
[cla-example]: https://github.com/hotosm/field-tm/blob/dev/.github/workflows/cla.yml
[madr]: https://adr.github.io/madr/

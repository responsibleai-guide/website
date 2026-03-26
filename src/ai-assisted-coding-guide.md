---
layout: base.njk
title: AI-Assisted Coding Guide
---

# Practical Guide: Working with AI Tools as a Developer

**Boundaries, workflows, and expectations for AI-assisted development.**

---

## The Golden Rule

**You are the author. You are accountable.**

If you used AI to generate code, documentation, or analysis - and it is wrong, insecure, biased, or breaks something - that is on you. Not the tool. You submitted it. You own it.

This is not punitive. It is the same standard we hold for any tool.

Using AI is like delegating work to a junior team member. It can move fast and produce useful output, but it still requires your review, judgment, and sign-off. The final responsibility sits with you.

---

## What AI Is Good For (Appropriate To Use Here)

- **First drafts, boilerplate, prototyping:** Config files, repetitive scaffolding, test stubs, standard templates. And if you need to assess the feasibility of an idea, AI can generate a throwaway test quickly, before planning a well-architected solution.
- **Explaining unfamiliar code:** "What does this function do?" is a great use of AI.
- **Brainstorming approaches:** "What are three ways I could structure this migration?" - then evaluate each with your own judgment. Note that LLMs tend to be agreeable and may affirm your framing. Phrase questions in a neutral way, and be aware the response may not surface all options.
- **Drafting documentation:** Let AI produce a first pass, then rewrite it in your own words to ensure accuracy.
- **Debugging assistance:** Paste an error message and ask for possible causes. Verify before applying.
- **Refactoring suggestions:** AI can suggest cleaner patterns. If you only need a small refactor, be explicit about scope - AI has a tendency toward mission creep, and a tightly scoped request can expand quickly if you're not specific.
- **Splitting PRs into smaller chunks:** Sometimes PRs (particularly those produced by LLMs) can be far too large to properly review. AI can suggest a logical division of code into separate PRs / commits, allowing for easier human review.

## What AI Is Bad At (Be Cautious Here)

- **Architecture decisions:** AI does not understand your system's history, constraints, or users. It will happily suggest a redesign and large refactors that ignore your actual context. These decisions are yours to make.
- **Security-sensitive code:** AI-generated code frequently has vulnerabilities - missing input validation, insecure defaults, outdated patterns. All security-relevant code must be manually engineered and reviewed.
- **Domain-specific logic:** AI often fails on edge cases unique to your domain. _(Example from geospatial: AI routinely mishandles anti-meridian polygon wrapping because it doesn't understand spherical geometry unless explicitly guided.)_ Providing domain-specific context in your prompt can help, but in some cases general models are simply not the right tool for specialized knowledge.
- **Choosing dependencies:** LLMs frequently generate custom implementations instead of using well-tested libraries. Always ask: does a maintained library already solve this? If yes, use it.
- **Anything involving beneficiary or sensitive data:** Never paste personal data, donor information, or internal strategy into AI tools. Ensure access restrictions are set when sharing code repositories with agents (including ensuring `.env` files and secrets are not exposed).

## What AI Must Not Be Used For

- **Submitting code you have not read and understood.** If you cannot explain what it does line by line, do not submit it.
- **Generating reports or communications** that go to beneficiaries, donors, or partners without human review. AI can draft; a human must finalize.
- **Replacing your own learning.** If a task is assigned to help you learn a codebase or technology, do the thinking yourself first. Use AI to check your work, not to bypass it.
- **Feeding sensitive or private data into any AI tool.** This includes names, contact details, health data, financial information, and internal strategy documents.

---

## The Workflow: How to Actually Use AI Well

### 1. Think First, Then Prompt

Before asking AI anything, spend enough time forming your own mental model of the problem. What are the constraints? What approaches come to mind? Trying to answer a question yourself, even imperfectly, before consulting AI produces better understanding and retention.

### 2. Decide How You Want to Be Assisted

Matching your mode of assistance to the task matters. Some options to consider:

- **Ideas only:** Ask for options or approaches, then decide yourself. Good for architecture questions or unfamiliar territory.
- **Pair programming:** Work step by step alongside the AI, reviewing each small change before continuing. Good for most coding tasks.
- **Agentic / autonomous mode:** The AI plans and executes a larger task with less oversight. Use sparingly - see note below.
- **Review only:** Write your code first, then ask AI to review it. Good for learning and for catching issues before you commit.

> **A note on agentic coding:** As noted in [the ethical framework](/), agentic coding may erode understanding of a codebase and actually reduce productivity. If you use agentic mode, keep tasks small and reviewable, check in regularly, and don't walk away expecting a finished feature. If the agent is struggling after two or three iterations, step in and debug manually rather than looping indefinitely.

### 3. Write a Clear Spec, Not a Vague Request

- **Bad:** "Make this work better."
- **Good:** "Refactor this function to handle null input gracefully. Return an empty list instead of throwing. Keep the existing API signature. Add a unit test for the null case."

The more precise your prompt, the more useful the output.

### 4. Give AI a Way to Verify Its Own Work

Where possible, set the AI up with a feedback loop - a way to run tests, check output, or validate behavior. This significantly improves result quality. Without verification, the AI is guessing. With it, it can iterate until things actually work. This might mean running a test suite, executing a bash command that confirms expected behavior, or checking a UI in a browser. The form of testing matters less than its existence.

### 5. Review Everything

- Read every line of generated code.
- Check that it uses existing project libraries and patterns rather than reinventing solutions (see [prefer existing libraries](#prefer-existing-libraries)).
- Run the tests. If there are no tests, write them before accepting the code.
- Ask yourself: "Could I explain this to a colleague?" If not, dig deeper before committing.
- Ask yourself: "Am I overcomplicating this?". Often a problem is poorly framed & LLMs apply complex patterns to solve them. Perhaps reframing might lead to a simpler solution.
- Where possible, add detailed comments as to **why** a line of code is needed (not explaining _what_ the line of code does, which should be self-explanatory, and is often what LLMs do).

### 6. Keep Changes Small

Do not ask AI to generate an entire feature in one shot. Break work into small, reviewable increments. Smaller steps are easier to steer, easier to understand, and easier to debug. This mirrors good engineering practice regardless of whether AI is involved.

If you catch the AI going in the wrong direction, it's better to catch it early than after it has compounded the mistake across many files.

### 7. Label AI-Assisted Work

When committing code or submitting PRs that include substantial AI-generated content, note it in a Git `trailer`:

```
Assisted-by: [tool name, e.g. Claude, Copilot]
```

This is not about shame. It is about transparency and helping reviewers calibrate their attention. It is also becoming standard practice in major open source projects (LLVM, QGIS, Drupal, Fedora).

### 8. Do Not Iterate Blindly

If AI gives you broken code and you keep pasting the error back in hoping it will fix itself - stop. After two or three failed attempts, step back and debug manually. Blindly looping with an AI wastes time and teaches you nothing.

---

## Contributing to Open Source with AI

If your work involves contributing to upstream open source projects, additional care is required.

### Respect Maintainer Time

Every PR you submit costs someone time to review. Before submitting AI-assisted contributions:

- Read the project's contributing guidelines and AI policy (if one exists).
- Ensure the contribution is high quality, well-tested, and clearly explained.
- Write the PR description yourself - do not let AI generate it.
- Be prepared to answer questions about your code during review.

### Do Not Chase Volume

One excellent, well-tested PR is worth more than ten AI-generated patches that each require maintainer effort to evaluate. Quality over quantity. Always.

### Prefer Existing Libraries

LLMs have a strong tendency to generate bespoke implementations rather than using established libraries. Before accepting AI-generated code that solves a common problem, ask: is there an existing library the project already uses for this? If yes, use it.

### Do Not Use AI for "Good First Issues"

Several projects (including LLVM) explicitly forbid this. These issues exist as learning opportunities for new contributors. Using AI to solve them defeats the purpose and displaces human growth - even if the project has not formalised the rule.

---

## Security Checklist for AI-Generated Code

Before merging any AI-generated code, verify:

- [ ] Input validation is present and correct (see [OWASP Input Validation Cheat Sheet][owasp-input])
- [ ] No hardcoded secrets, tokens, or credentials
- [ ] Dependencies are pinned and from trusted sources (see [OWASP Vulnerable Dependency Management Cheat Sheet][owasp-deps])
- [ ] Error handling does not leak sensitive information (see [OWASP Error Handling Cheat Sheet][owasp-errors])
- [ ] No unnecessary permissions or access scopes (apply [least privilege][owasp-least-priv])
- [ ] SQL queries are parameterized (no string concatenation; see [OWASP SQL Injection Prevention Cheat Sheet][owasp-sqli])
- [ ] File paths are sanitized against [traversal attacks][owasp-traversal].
- [ ] Output encoding is appropriate for the context (HTML, JSON, etc.; see [OWASP Cross Site Scripting Prevention Cheat Sheet][owasp-xss])

---

## Protecting Your Own Skills

AI can make you faster, but it can also make you dependent if you are not deliberate about learning.

- **Space your learning.** Do not cram an entire AI-assisted session into one marathon. Return to the same problem area across multiple days.
- **Diversify tasks.** If AI is handling the rote work, use the free time for deeper exploration - reading documentation, understanding architecture, reviewing others' code.
- **Explain what you learned.** After an AI-assisted session, write a short note (even just for yourself) about what you now understand that you did not before.
- **Maintain manual skills.** Regularly write code, debug problems, and read error messages without AI. Like any skill, it atrophies without practice.
- **Breadth helps.** The developers getting the best results from AI tools tend to be broadly skilled - able to evaluate whether the AI's choices make sense across the stack, not just in a narrow slice.

---

## Approved Tools and Data Boundaries

| Category                                   | Guidance                                                                                                                                                                                                                                                     |
| ------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Code completion** (e.g. Copilot, Cursor) | Permitted for non-sensitive code. Review all suggestions.                                                                                                                                                                                                    |
| **Chat-based AI** (e.g. Claude, ChatGPT)   | Permitted for general development questions. Never paste sensitive data.                                                                                                                                                                                     |
| **AI code review tools**                   | Only if they keep a human in the loop. Automated review bots may post comments, but cannot change content without human approval.                                                                                                                            |
| **AI agents** (autonomous)                 | Not permitted to update code directly on shared platforms (e.g. within PRs) without explicit human approval of each action. May be used locally (e.g. in a developer's IDE) as long as workflows are manually approved and final code is quality controlled. |

In summary: **do not allow automated commits from AI agents**. Most other usage is permitted, with the boundaries described above.

---

## Policy Review

This guide should be reviewed every three months or whenever a significant change occurs in AI tooling, team composition, or organisational risk posture. AI capabilities are evolving rapidly; our practices must keep pace.

[owasp-input]: https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html
[owasp-deps]: https://cheatsheetseries.owasp.org/cheatsheets/Vulnerable_Dependency_Management_Cheat_Sheet.html
[owasp-errors]: https://cheatsheetseries.owasp.org/cheatsheets/Error_Handling_Cheat_Sheet.html
[owasp-least-priv]: https://owasp.org/www-community/controls/Least_Privilege_Principle
[owasp-sqli]: https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html
[owasp-traversal]: https://owasp.org/www-community/attacks/Path_Traversal
[owasp-xss]: https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html

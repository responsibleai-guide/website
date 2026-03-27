---
layout: base.njk
title: Using LLMs Responsibly
description: A living framework for NGOs, civil society organizations, and mission-driven teams navigating AI adoption in software development.
---

# Using LLMs to Develop Software: Ethics, Risks, and Responsible Practice

**A living framework for NGOs, civil society organizations, and mission-driven teams navigating AI adoption in software development.**

_First draft - due to update May 2026_

---

## Collaborators

This framework was built in collaboration and is adopted by the following partners:

<div class="collaborator-logos">
  <a href="https://www.hotosm.org" target="_blank" rel="noopener noreferrer">
    <img src="/images/hotosm-logo.png" alt="Humanitarian OpenStreetMap Team" width="330" height="80" loading="lazy">
  </a>
  <a href="https://510.global" target="_blank" rel="noopener noreferrer">
    <img src="/images/501-nl-red-cross-logo.jpg" alt="510 NL Red Cross" width="1221" height="289" loading="lazy">
  </a>
  <a href="https://www.opengis.ch" target="_blank" rel="noopener noreferrer">
    <img src="/images/opengisch-logo.png" alt="OPENGIS.ch" width="400" height="120" loading="lazy">
  </a>
</div>

## Introduction

AI coding tools have moved from novelty to daily workflow in under two years. Andrej Karpathy coined the term "vibe coding" in early 2025 - describing developers who prompt AI, accept all suggestions, and barely read the output. By early 2026, he had already moved on, calling the practice outdated and advocating instead for "agentic engineering": careful, supervised AI-assisted development with full human oversight [1]. While early-2025 AI models were shown in some cases to have a net negative impact on developer productivity [21], models have improved significantly by early 2026, alongside growing efforts within open-source communities to establish appropriate governance and usage policies.

This trajectory tells us something important: the tools are real and improving rapidly, but the hype cycle consistently outpaces responsible adoption. For any organization working in the public interest and given the prevalence of LLM use in software in this moment, a governance approach that provides deliberate attention to the ethics of LLM use in software is required.

This document provides a framework in three parts: the ethical concerns AI adoption raises and how to mitigate them; the specific responsibilities that arise when AI intersects with open-source practice; and the human dimensions - learning, craft, and cognition - that must be protected as these tools become pervasive.

---

## 1. Ethical Concerns

AI adoption is not just a tooling decision. It is a values decision. Below are the primary ethical risks, alongside practical mitigation strategies.

### 1.1 Data Privacy and Security

Prompts sent to proprietary AI services may be stored or reused. Pasting sensitive data - beneficiary records, donor information, strategy documents, personnel details - into a commercial AI tool creates privacy and security exposure. However, this risk is not as present during software development.

Research published in _Nature Scientific Reports_ highlights the cybersecurity risks inherent in AI-generated code, including injection vulnerabilities, insecure templates, and insufficient input validation [2].

**Mitigation approaches:**

- Treat all AI-generated code as untrusted third-party code, subject to mandatory human review before merging.
- Require at least two human reviewers for every change entering a codebase.
- Always ensure input documents and data are sanitized or anonymized before feeding them into agents.
- Maintain best practice automated security review for repos: static code analysis, dependency scanning, vulnerability scanning, CI-based tests before merge.

### 1.2 Bias and Discrimination

AI models are trained on internet-scale data that reflects existing societal biases. Research has shown LLMs associating specific ethnic groups with violence, reproducing gender stereotypes, and skewing outputs toward Western perspectives [3]. For organizations serving marginalized communities globally, this is not an abstract concern - it is an operational risk. AI-generated content, code, or analysis may silently encode assumptions that undermine the very populations an organization exists to serve.

In coding, bias can surface in less obvious ways: culturally narrow test data, dataset assumptions, internationalization blind spots, and biased evaluation criteria in synthetic datasets.

**Mitigation approaches:**

- Critically review AI-generated test data and synthetic datasets.
- Intentionally include diverse and internationalised test cases.
- Apply extra scrutiny when AI touches user-facing data structures or datasets.
- Maintain human oversight whenever AI is used beyond pure logic or refactoring.
- Actively evaluate every AI output that touches affected communities for embedded assumptions, stereotypes, or Western-centric framing.

### 1.3 Environmental Cost

Training large language models requires enormous computational resources. Each query consumes energy. For organizations with environmental or sustainability principles, uncritical adoption of AI tools creates a tension between productivity gains and ecological impact.

**Mitigation approaches:**

- Select fit-for-purpose models: use small or local models for inline suggestions, reserving larger models for complex reasoning tasks.
- Use AI deliberately, not habitually.
- Avoid vendor lock-in, maintaining the flexibility to shift to more efficient or open alternatives as they emerge.
- Acknowledge environmental cost explicitly in AI use policies.
- Consider offset strategies with regard to energy usage and emissions. Approximations in the [calculations appendix](#appendix-a-methodology-for-estimating-llm-energy--co-emissions-and-donation-proxy) recommend that a small team of 5 developers should donate ~$300 annually to offset.

### 1.4 Labour and Exploitation

The refinement of AI models often relies on low-paid human labor for data labeling and content moderation, frequently in low- and middle-income economies. The training data itself was often collected without consent from its creators. Using these tools means participating in a supply chain with unresolved ethical questions about consent, compensation, and intellectual property [20].

**Mitigation approaches:**

- Preference for open models where viable. See [recommended open models page](/recommended-open-models/).
- Maintain tooling flexibility to reduce dependence on a single corporate provider.
- Ensure transparency in usage, especially when contributing to open source.
- Advocate for equitable AI access through support of local, open solutions.

### 1.5 Intellectual Property and Copyright

Current AI models raise unresolved questions about copyright. The LLVM Project's AI policy states it clearly: using AI tools to regenerate copyrighted material does not remove the copyright, and contributors remain responsible for ensuring nothing infringing enters their work [4]. The risk includes inadvertently incorporating copyrighted code or text into publicly released outputs.

Unfortunately, there are not many strategies to avoid the underlying ethical concern of stolen intellectual property, aside from not using the models.
Even 'open' models will generally contain traces of stolen training material.

**Mitigation approaches:**

- Contributors must review and understand generated code, without blindly accepting it.
- Use code analyzers and copyright detection tools such as ScanCode Toolkit, built into CI pipelines.
- Substantial AI-generated contributions must be disclosed in commit messages.
- Recognize that producing open-source work means it may itself become training data - but that does not remove the responsibility to ensure no infringements enter the commons.

### 1.6 Digital Divide and Equity

AI coding tools are already more accessible to people in wealthy countries, and as the technology industry attempts to recoup its enormous capital investments, prices are likely to rise. At the same time, AI tools are eroding the equitable commons of free and open-source knowledge and universally accessible knowledge bases like Stack Exchange. There is a real risk of a two-tier system developing: massively powerful tools running in corporate data centers for the well-resourced, much less capable local instances for everyone else, and a diminished shared commons between the two [5].

**Mitigation approaches:**

- Produce open-source software that partners can adopt freely.
- Advocate for and invest in open-source models that can run locally.
- Avoid locking into a single proprietary ecosystem.
- Design systems that remain maintainable without AI dependence.
- Empower local partners with open tools, while remaining vigilant about access gaps.

---

## 2. AI in Open Source: Responsibility, Pressure, and Maintenance

AI affects not just how we code - but how we participate in the commons.

### 2.1 Asymmetric Pressure and Extractive Contributions

Dries Buytaert, lead of the Drupal project, describes the core problem precisely: AI makes it cheaper to contribute, but it does not make it cheaper to review [6]. More contributions are flowing into open-source projects, but the burden of evaluating them still falls on the same small group of maintainers. This creates asymmetric pressure that risks burning out the people who hold projects together.

The LLVM Project introduced the concept of an "extractive contribution" - one where the cost to maintainers of reviewing it exceeds the benefit to the project [4]. Before AI, posting a change for review signalled genuine interest from a potential long-term contributor. AI has decoupled effort from intent. A drive-by contributor can now generate a large patch in minutes and shift hours of review work onto volunteers.

Daniel Stenberg, maintainer of curl, canceled the project's bug bounty program after AI-generated reports flooded his seven-person security team - fewer than one in twenty turned out to be real bugs. Yet in the same period, an AI security startup used AI well and found all 12 zero-day vulnerabilities in a recent OpenSSL security release, some hiding for over 25 years [7]. The difference was not whether AI was used. It was expertise and intent.

AI-generated code also frequently reinvents the wheel - producing custom implementations rather than leveraging well-tested community libraries. This creates fragmentation and shifts maintenance burden onto the ecosystem [8].

**Mitigation: review discipline and contribution hygiene.** Good engineering practice matters more than ever. Organizations should formalize policies addressing AI in contributions. For practical guidance, see [Working with AI Tools as a Developer](/ai-assisted-coding-guide/) and [Repo Checklist](/managing-ai-contributions/).

Regardless of whether AI is used:

- Always submit small, focused pull requests.
- Separate refactors from features.
- Break large features into logical chunks.
- Never use AI to produce massive, opaque changes.
- Require at least two reviewers for every change entering the main branch.
- Ensure contributors respond to all review comments - including automated ones - before final approval.
- Hold external contributors and contractors to the same standards.
- Resist the temptation to "move fast" at the expense of understanding.

### 2.2 Long-Term Maintenance

While AI can be effective for quickly getting something up and running, it creates a significant pain point when it comes to maintaining or upgrading that code. If the people responsible for a codebase do not understand how it was built, they will eventually hit a wall - making maintenance, debugging, and upgrades extremely difficult. This can ultimately restrict an organization's ability to build anything new, because it is trapped by code it cannot confidently modify [9].

AI tools are demonstrably helpful when assisting someone who already understands the codebase and the broader technical landscape, but they are far less reliable as a substitute for that understanding.

See [AI-Assisted Coding Guide](/ai-assisted-coding-guide/#what-ai-is-good-for-appropriate-to-use-here) for details on appropriate usage of LLMs.

### 2.3 What Leading Projects Are Doing

Project responses range from cautious acceptance to outright bans. The landscape is moving fast, but the following represent the most significant approaches as of early 2026. Notably, the platforms hosting open-source projects have been slow to provide maintainer tooling for filtering or flagging AI-generated contributions - several projects cite this as a direct driver of their restrictive policies [10]. OSS foundations, meanwhile, have largely focused on licensing questions rather than the quality and burnout crisis maintainers are facing now [10].

**Disclosure and accountability:**

- **LLVM Project** - AI use permitted but must be disclosed. Contributors must understand and explain their code. "Good first issues" are reserved for human learning [4].
- **Linux Kernel** - AI code accepted, but undisclosed use results in the contributor being banned [12].
- **cURL** - AI contributions accepted with mandatory disclosure. Policy breach means a permanent ban. The bug bounty was canceled after AI reports overwhelmed the team - only 5% of submissions identified real vulnerabilities [13], [10].
- **CloudNativePG** - Permits AI-assisted contributions under strict human accountability rules. Contributors must fully understand and maintain AI-generated code, disclose usage via commit trailers, and _guarantee legal provenance_. "Shotgun refactoring" (wide-scale refactoring or clean-up), hallucinated features, and AI-written PR descriptions are explicitly prohibited. Maintainers reserve the right to close low-effort AI PRs without detailed critique [19].
- **Apache Spark** - Every PR must disclose AI use. Of ~8,500 commits over 2.5 years, only ~1.5% disclosed AI, but the rate is accelerating sharply [11].
- **Apache Airflow** - Updated contributing guidelines to require AI disclosure after a surge of low-quality AI-generated PRs [11].

**Still navigating:**

- **CPython** - No formal policy yet, though AI-co-authored commits are appearing. Python's centrality to the AI ecosystem makes its eventual stance significant [11].
- **QGIS / GDAL** - Drafting transparency and accountability policies [14, 15].
- **OpenDroneMap** - Still in active discussion with differing opinions within the community [16].
- **Debian** - Community remains undecided; a proposed General Resolution was withdrawn [17].
- **FluxCD** - Aligning with CNCF; experimenting with AI guidelines in sub-projects [10].

**Restrictive approaches or bans:**

- **Ghostty** - Banned AI-generated PRs from external contributors after escalating from disclosure requirements. Maintainers themselves still use AI daily [10].
- **tldraw** - Auto-closes all external PRs. The founder discovered his own AI-generated issue scripts were producing slop that contributors then fed to their own AI tools - "AI slop all the way down" [10].
- **NetBSD** - LLM-generated code is classified as "tainted" and requires prior written approval from core [11].
- **Gentoo** - Banned AI-generated contributions entirely, citing copyright, quality, and environmental concerns [10].

The common thread: human accountability, transparent AI use, respect for maintainer time, and protection of the commons. Notably, even projects that ban external AI contributions often use AI internally - the issue is not the tool itself but the absence of understanding, accountability, and genuine engagement behind the contribution.

---

## 3. Sustaining Human Skill, Judgment, and Craft

AI tools are powerful, but they interact with human cognition in ways that require deliberate management.

### 3.1 Cognitive Risks

Research confirms what many developers suspect: how AI is used matters as much as whether it is used. In a randomized study, participants who relied solely on generated code scored just 24–39% on follow-up comprehension tests, while those who asked for explanations scored 65–86% [22]. The delegation group finished fastest - but retained the least.

Several patterns can erode skill if left unchecked:

- **Cognitive offloading:** Delegating thinking to AI rather than using it to support thinking. Debugging is a particular risk area - repeatedly asking AI to fix errors without understanding why they occurred correlates with both slower completion times and worse comprehension.
- **Cramming effect:** AI can flood you with information in a single session, creating a false sense of learning. Spaced, deliberate engagement produces better long-term retention.
- **Reduced pre-testing:** Trying to solve a problem yourself before consulting AI produces stronger understanding than going straight to the generated answer. Skipping this weakens learning even when the final code is correct.
- **Metacognitive erosion:** The ability to monitor your own thinking is more important than ever. Developers who passively accept AI output without reflection gradually lose the ability to evaluate it critically.

For small teams, this is a serious risk. If developers stop deeply understanding the systems they maintain, there is no safety net - and the people least equipped to debug AI-written code may be those whose skills were eroded by relying on it.

**Mitigation approaches:**

- Attempt problem-solving before consulting AI.
- When using AI for code generation, follow up with questions that build understanding - ask for explanations, probe edge cases, read the output critically.
- Use AI as a pair-programmer, not an answer machine.
- Encourage discussion of approach before generation.
- Prioritize learning over speed, especially on unfamiliar tasks or technologies.
- Conduct regular internal reviews of architectural understanding.
- Encourage spacing and reflection, not continuous prompting.

### 3.2 Preserving the Craft of Engineering

AI can generate syntactically correct code quickly. But framing the right problem, designing architecture, evaluating trade-offs, aligning with stakeholder needs, and mentoring others - these remain deeply human tasks. As AI handles more of the mechanical work of coding, it becomes _more_ important, not less, for human interaction to focus on problem framing, approach discussion, and alignment before setting AI to do the implementation work.

This applies with particular force to junior developers. The errors and dead ends that feel frustrating during independent work are also where the deepest learning happens. Skipping that struggle in favor of AI-generated solutions can create a gap between apparent productivity and actual competence - one that may not become visible until something breaks in production.

**Practical commitments:**

- Discuss design before generating implementation.
- Encourage collaborative reasoning about approaches.
- Use AI to accelerate execution - not replace judgment.
- Protect time for difficult, meaningful problem-solving.
- Recognize that enjoying the craft of coding and tackling hard problems is essential to team morale and professional growth.

---

## Guiding Principles

1. **Human accountability is non-negotiable.** AI assists; humans decide, review, and own the output.
2. **Transparency is mandatory.** When AI is used, it should be disclosed - in code commits, in documents, in reports.
3. **Protect your maintainers.** Never allow AI to increase the burden on those who review and maintain code without providing corresponding relief.
4. **Prioritize learning over speed.** An organization's greatest asset is its people. If AI adoption undermines their ability to learn and grow, the short-term productivity gain is not worth it.
5. **Never input sensitive data into commercial AI tools.** Beneficiary data, personnel information, strategic documents, and donor details must not enter commercial AI systems without clear data governance.
6. **Interrogate bias actively.** Every AI output that touches the communities you serve should be critically evaluated for embedded assumptions.
7. **Respect the open-source commons.** Ensure AI-assisted contributions are high quality, transparent, and do not extract more from maintainers than they give back.
8. **Champion equitable access.** Advocate for and invest in open-source models that can run locally, ensuring communities are not left behind.
9. **Use fit-for-purpose models.** Match the tool to the task; do not default to the largest available model.
10. **Always favor small, reviewable changes.** Good engineering discipline is the best defense against AI-generated complexity.

---

## Living Document Commitment

AI capabilities, norms, and risks evolve rapidly. This document should be reviewed and updated at least every three months. Responsible AI adoption is not about maximizing automation - it is about responsibly augmenting human capacity while protecting beneficiaries, contributors, the open-source ecosystem, and the long-term capability of teams.

This framework is intended as a starting point for consultation among NGOs, civil society organizations, and mission-driven teams. Contributions, critique, and adaptation are welcome.

---

## References

1. Karpathy, A. (2025–2026). [From "vibe coding" to "agentic engineering."](https://x.com/karpathy/status/2019137879310836075)
2. Nature Scientific Reports (2026). [Cybersecurity risks in AI-generated code.](https://www.nature.com/articles/s41598-025-34350-3)
3. Queen Margaret University Library. [Generative AI: Ethics.](https://libguides.qmu.ac.uk/generative-ai/ethics)
4. LLVM Project. [AI Tool Policy.](https://llvm.org/docs/AIToolPolicy.html)
5. Stack Overflow Blog (2025). [Whether AI is a bubble or revolution, how does software survive?](https://stackoverflow.blog/2025/12/25/whether-ai-is-a-bubble-or-revolution-how-does-software-survive/)
6. Buytaert, D. (2025). [AI creates asymmetric pressure on open source.](https://dri.es/ai-creates-asymmetric-pressure-on-open-source)
7. [AI finds 12 of 12 OpenSSL zero-days while curl cancelled its bug bounty.](https://www.lesswrong.com/posts/7aJwgbMEiKq5egQbd/ai-found-12-of-12-openssl-zero-days-while-curl-cancelled-its)
8. Mapscaping Podcast. [Vibe coding and the fragmentation of open source.](https://mapscaping.com/podcast/vibe-coding-and-the-fragmentation-of-open-source/)
9. Caimito (2025). [The recurring dream of replacing developers.](https://www.caimito.net/en/blog/2025/12/07/the-recurring-dream-of-replacing-developers.html)
10. Holterhoff, K. (2026). [AI Slopageddon and the OSS Maintainers.](https://redmonk.com/kholterhoff/2026/02/03/ai-slopageddon-and-the-oss-maintainers/) RedMonk.
11. Nair, K. (2026). [AI usage in popular open source projects.](https://tirkarthi.github.io/programming/2026/02/13/ai-usage-open-source-projects.html)
12. [Linux kernel mailing list AI Policy.](https://lore.kernel.org/ksummit/20251114183528.1239900-1-dave.hansen@linux.intel.com/)
13. [cURL contribution policy: On AI use in curl.](https://curl.se/dev/contribute.html#on-ai-use-in-curl)
14. [QGIS Enhancement Proposal. AI Tool Policy.](https://github.com/qgis/QGIS-Enhancement-Proposals/pull/363)
15. [GDAL AI Tool Policy.](https://gdal--13880.org.readthedocs.build/en/13880/community/ai_tool_policy.html)
16. [OpenDroneMap AI contribution policy discussion.](https://github.com/OpenDroneMap/documents/pull/4)
17. [Debian AI General Resolution withdrawn.](https://lwn.net/Articles/1020968/)
18. Hicks, C. [Cognitive helmets for the AI bicycle.](https://www.fightforthehuman.com/cognitive-helmets-for-the-ai-bicycle-part-1/)
19. [Cloud Native PG AI Usage Policy](https://github.com/cloudnative-pg/governance/blob/main/AI_POLICY.md)
20. Regilme, S.S.F. (2024). [Artificial Intelligence Colonialism](https://doi.org/10.1353/sais.2024.a950958)
21. [Measuring the Impact of Early-2025 AI on Experienced Open-Source Developer Productivity](https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study)
22. Shen, H.S & Tamkin, A. (2026). [How AI Impacts Skill Formation](https://arxiv.org/abs/2601.20245)
23. Epoch AI. [How much energy does ChatGPT use?](https://epoch.ai/gradient-updates/how-much-energy-does-chatgpt-use)
24. OpenAI. [Data Residency Docs](https://help.openai.com/en/articles/9903489-data-residency-and-inference-residency-for-chatgpt)
25. International Energy Agency. [Global Emissions Report](https://www.iea.org/reports/electricity-2025/emissions)
26. Founders Pledge. [Climate And Lifestyle Report](https://www.founderspledge.com/research/climate-and-lifestyle-report)
27. Effective Environmentalism. [Climate Charity Recommendations](https://www.effectiveenvironmentalism.org/climate-charities)

[1]: https://x.com/karpathy/status/2019137879310836075 "Karpathy, A. (2025–2026). From 'vibe coding' to 'agentic engineering.'"
[2]: https://www.nature.com/articles/s41598-025-34350-3 "Nature Scientific Reports (2026). Cybersecurity risks in AI-generated code."
[3]: https://libguides.qmu.ac.uk/generative-ai/ethics "Queen Margaret University Library. Generative AI: Ethics."
[4]: https://llvm.org/docs/AIToolPolicy.html "LLVM Project. AI Tool Policy."
[5]: https://stackoverflow.blog/2025/12/25/whether-ai-is-a-bubble-or-revolution-how-does-software-survive/ "Stack Overflow Blog (2025). Whether AI is a bubble or revolution, how does software survive?"
[6]: https://dri.es/ai-creates-asymmetric-pressure-on-open-source "Buytaert, D. (2025). AI creates asymmetric pressure on open source."
[7]: https://www.lesswrong.com/posts/7aJwgbMEiKq5egQbd/ai-found-12-of-12-openssl-zero-days-while-curl-cancelled-its "AI finds 12 of 12 OpenSSL zero-days while curl cancelled its bug bounty."
[8]: https://mapscaping.com/podcast/vibe-coding-and-the-fragmentation-of-open-source/ "Mapscaping Podcast. Vibe coding and the fragmentation of open source."
[9]: https://www.caimito.net/en/blog/2025/12/07/the-recurring-dream-of-replacing-developers.html "Caimito (2025). The recurring dream of replacing developers."
[10]: https://redmonk.com/kholterhoff/2026/02/03/ai-slopageddon-and-the-oss-maintainers/ "Holterhoff, K. (2026). AI Slopageddon and the OSS Maintainers. RedMonk."
[11]: https://tirkarthi.github.io/programming/2026/02/13/ai-usage-open-source-projects.html "Nair, K. (2026). AI usage in popular open source projects."
[12]: https://lore.kernel.org/ksummit/20251114183528.1239900-1-dave.hansen@linux.intel.com/ "Linux kernel mailing list AI Policy."
[13]: https://curl.se/dev/contribute.html#on-ai-use-in-curl "cURL contribution policy: On AI use in curl."
[14]: https://github.com/qgis/QGIS-Enhancement-Proposals/pull/363 "QGIS Enhancement Proposal. AI Tool Policy."
[15]: https://gdal--13880.org.readthedocs.build/en/13880/community/ai_tool_policy.html "GDAL AI Tool Policy."
[16]: https://github.com/OpenDroneMap/documents/pull/4 "OpenDroneMap AI contribution policy discussion."
[17]: https://lwn.net/Articles/1020968/ "Debian AI General Resolution withdrawn."
[18]: https://www.fightforthehuman.com/cognitive-helmets-for-the-ai-bicycle-part-1/ "Hicks, C. Cognitive helmets for the AI bicycle."
[19]: https://github.com/cloudnative-pg/governance/blob/main/AI_POLICY.md "Cloud Native PG AI Usage Policy."
[20]: https://doi.org/10.1353/sais.2024.a950958 "Artificial Intelligence Colonialism."
[21]: https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study "Early-2025 AI-dev productivity."
[22]: https://arxiv.org/abs/2601.20245 "How AI Impacts Skill Formation."
[23]: https://epoch.ai/gradient-updates/how-much-energy-does-chatgpt-use "How much energy does ChatGPT use?"
[24]: https://help.openai.com/en/articles/9903489-data-residency-and-inference-residency-for-chatgpt "OpenAI Data Residency Docs."
[25]: https://www.iea.org/reports/electricity-2025/emissions "International Energy Agency Global Emissions Report."
[26]: https://www.founderspledge.com/research/climate-and-lifestyle-report "Founders Pledge Climate And Lifestyle Report."
[27]: https://www.effectiveenvironmentalism.org/climate-charities "Effective Environmentalism Climate Charity Recommendations."

## Additional Sources

The following sources informed the development of this framework but are not directly cited above.

- Pragmatic Institute. [The $304 million AI mistake.](https://www.pragmaticinstitute.com/resources/articles/product/the-304-million-ai-mistake-why-responsible-ai-isnt-just-about-regulations/)
- Osmani, A. (2025). [Writing a good spec.](https://addyosmani.com/blog/good-spec/)
- Bjarnason, B. (2025). [Trusting your own judgement on AI.](https://www.baldurbjarnason.com/2025/trusting-your-own-judgement-on-ai/)
- The Guardian (2026). [Tech AI bubble.](https://www.theguardian.com/us-news/ng-interactive/2026/jan/18/tech-ai-bubble-burst-reverse-centaur)
- [GRASS GIS Contributing Guidelines.](https://github.com/OSGeo/grass/blob/main/CONTRIBUTING.md)

---

_Disclaimer: Initial content summarized by Claude Opus 4.6 from the sources listed above, then manually reviewed and edited. This document is released for consultation and collaborative refinement._

---

## Appendix A: Methodology for Estimating LLM Energy & CO₂ Emissions and Donation Proxy

There is no easy way to estimate energy usage of LLM queries.

Below are some simple 'back-of-the-envelope' calculations to give a rough estimation of the potential magnitude of energy consumption.

### 1. Approximate LLM Usage

The most accurate approach would be to average token use per team member on a given provider.

However, as we do not have a prescriptive usage policy, and developers can use open models, we need approximations:

- 5 team members using LLMs.
- 64hrs potential LLM usage per team member, per month (4 day work week, 8hrs a day, average of 4hrs available for coding).
- Prompts per hour: ~20

**6400 prompts per month**

### 2. Convert Queries to Electricity Usage

- A recent analysis by Epoch AI [23] gave an estimate of ~0.0003 kWh per query to GPT-4o, for a 100 word context.
- There are no official published per-query figures for models like Opus 4.6.
- If we take the approximation from the analysis and multiply by a fudge factor of 50x, considering larger context loaded from a codebase, and longer spent on reasoning problems, plus a 1.2x multiplier for datacenter overhead:

Energy per query: ~0.018 kWh

**115.2 kWh usage per month** (for a 5 person team)

### 3. Convert Electricity to CO₂ Emissions

- This depends on **where the compute actually runs**, which is hard to determine.
- For example, OpenAI API inference can be hosted in multiple regions (Europe, USA, Singapore, Japan, India), as per their [data residency docs](https://help.openai.com/en/articles/9903489-data-residency-and-inference-residency-for-chatgpt).
- Based on an International Energy Agency 2025 report [25], global grid CO₂ intensity can be estimated at ~445 gCO₂/kWh.
- More accurate estimates per country can be found via the [Electricity Maps Project](https://www.electricitymaps.com).

tCO₂e = kWh_total × (gCO₂/kWh / 1000 / 1000)

**0.051 tonnes CO₂ equivalent produced per month**

### 4. Convert Emissions to Donation Proxy

- Research by Founders Pledge, suggests that donating 1000 USD to effective climate advocacy charities could avert approximately 100 tCO₂ from being omitted (expected-value estimate for high-impact policy/advocacy orgs) [26].
- **This does not offset emissions made, nor negate your personal responsibility to reduce your footprint**.
- However, considering how uncommon this type of donation is (for corporate entities that typically do not care much), it could be argued that this is an acceptable mitigation strategy at this scale, by proxy.
- Let's add another fudge factor of 100x to account for additional uncertainties: model used, frequency of use, and inaccuracies in various approximations. This gives us a nicely pessimistic estimate of ~2.55 tCO₂.

Recommendation: **~26 USD donation per month**, for a team of 5 devs using LLM assistance.

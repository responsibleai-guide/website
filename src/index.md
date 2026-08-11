---
layout: base.njk
title: Using LLMs Responsibly
description: A living framework for NGOs, civil society organizations, and mission-driven teams navigating AI adoption in software development.
---

# Using LLMs to Develop Software: Ethics, Risks, and Responsible Practice

**A living framework for NGOs, civil society organizations, and mission-driven teams navigating AI adoption in software development.**

_Second draft - due to update Nov 2026_

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
  <a href="https://www.geocat.net" target="_blank" rel="noopener noreferrer">
    <img src="/images/geocat-logo.png" alt="GeoCat" width="1222" height="268" loading="lazy">
  </a>
  <a href="https://byteroad.net/" target="_blank" rel="noopener noreferrer">
    <img src="/images/byteroad-logo.png" alt="ByteRoad" width="1222" height="268" loading="lazy">
  </a>
</div>

## Introduction

AI coding tools have moved from novelty to daily workflow in under two years. Andrej Karpathy coined the term "vibe coding" in early 2025 - describing developers who prompt AI, accept all suggestions, and barely read the output. By early 2026, he had already moved on, calling the practice outdated and advocating instead for "agentic engineering": careful, supervised AI-assisted development with full human oversight [1]. An early-2025 study found AI tools made experienced open-source developers slower [21], but the same team's early-2026 follow-up found weak evidence of a speedup instead, as models improved [38]. Alongside this, open-source communities have made growing efforts to establish appropriate governance and usage policies to keep up.

This trajectory tells us something important: the tools are real and improving rapidly, but the hype cycle consistently outpaces responsible adoption. For any organization working in the public interest and given the prevalence of LLM use in software in this moment, a governance approach that provides deliberate attention to the ethics of LLM use in software is required.

The gap is real: a 2026 survey found 93% of aid workers had used AI tools, but only 22% worked under a formal AI policy [54], and local organizations in crisis regions show the highest daily usage with the least governance support [55].

This document provides a framework in three parts: the ethical concerns AI adoption raises and how to mitigate them; the specific responsibilities that arise when AI intersects with open-source practice; and the human dimensions - learning, craft, and cognition - that must be protected as these tools become pervasive.

---

## 1. Ethical Concerns

AI adoption is not just a tooling decision. It is a values decision. Below are the primary ethical risks, alongside practical mitigation strategies.

This framework aims at harm reduction, not blame. Shaming individuals for using AI, or for refusing to, is counterproductive: it drives usage underground and makes honest assessment of risk impossible. Nobody should be forced to use AI tools, and nobody should be shamed for declining to work with AI output. What follows is intended to support honest dialogue about risks, practical mitigations, and better technical literacy.

### 1.1 Data Privacy and Security

Prompts sent to proprietary AI services may be stored or reused. Pasting sensitive data - beneficiary records, donor information, strategy documents, personnel details - into a commercial AI tool creates privacy and security exposure. In software development, this mostly surfaces as leaked credentials and logs.

Research published in _Nature Scientific Reports_ highlights the cybersecurity risks inherent in AI-generated code, including injection vulnerabilities, insecure templates, and insufficient input validation [2]. Agents themselves are also now a target: prompt injection through connected tooling is increasingly common [31], and coding agents account for the majority of reported agentic AI security incidents [32].

LLMs are exceptional tools for discovering security vulnerabilities in code [33], but be especially careful using them while investigating an undisclosed vulnerability, since prompts describing it may be stored, exposed, or later discoverable, creating a disclosure risk for the organizations maintaining the affected software. Chat logs carry no legal privilege and can be obtained in litigation, as shown when a US court ordered OpenAI to produce 20 million user conversations [30].

**Mitigation approaches:**

- Treat all AI-generated code as untrusted third-party code, subject to mandatory human review before merging.
- Require at least two human reviewers for every change entering a codebase.
- Always ensure inputs and data are sanitized or anonymized before feeding them into agents (documents, logs, issue trackers).
- Maintain best practice automated security review for repos: static code analysis, dependency scanning, vulnerability scanning, CI-based tests before merge.
- Never paste details of undisclosed vulnerabilities into AI tools; assume all prompts are stored and discoverable.

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

Training large language models requires enormous computational resources. Each query consumes energy. For organizations with environmental or sustainability principles, adoption of AI tools creates a tension between productivity gains and ecological impact.

The energy used by each LLM prompt does not tell the full story. When considering this point, the wider implications on society should be considered. The electricity consumption may be small per-prompt (see [calculations appendix](#appendix-a-methodology-for-estimating-llm-energy--co-emissions-and-donation-proxy)), but usage of proprietary LLM services overall sends a signal to corporations running them that they are desirable. We end up with a feedback loop: people/businesses use AI → utilization/revenue expectations rise → labs demand compute → cloud providers commit capacity → financing becomes available → GPUs/servers get ordered → component production expands → data centers get constructed → utilities add generation/transmission → decades-long physical assets exist [37]. This entire chain has huge repercussions on long term environmental impact.

**Mitigation approaches:**

- Select fit-for-purpose models: use small or local models for inline suggestions, reserving larger models for complex reasoning tasks.
- Use AI deliberately, not habitually.
- Avoid vendor lock-in, maintaining the flexibility to shift to more efficient or open alternatives as they emerge.
- Acknowledge environmental cost explicitly in AI use policies.
- Collective advocacy on data center location and energy policy has more leverage than individual restraint [40], [41]. But, note that this evidence is almost entirely US based and is not a substitute for adequate government oversight [42].
- Consider offset strategies with regard to energy usage and emissions, as a secondary method to alleviate environmental strain. Approximations in the [calculations appendix](#appendix-a-methodology-for-estimating-llm-energy--co-emissions-and-donation-proxy) recommend that a small team of 5 developers should donate ~$300 annually to offset.

### 1.4 Labour and Exploitation

The refinement of AI models often relies on low-paid human labor for data labeling and content moderation, frequently in low- and middle-income economies. The training data itself was often collected without consent from its creators. Using these tools means participating in a supply chain with unresolved ethical questions about consent, compensation, and intellectual property [20].

As investigative journalism and studies catch up, data supporting this is growing. Investigations found Kenyan workers paid under $2 per hour to filter traumatic content [43], with similar conditions reported in India [44]. Fairwork's 2025 ratings of cloudwork platforms show that, of the data-work suppliers assessed, none paid a living wage and only two guaranteed minimum wage [45]. Big tech firms also route this work through at least 30 intermediary platforms, fragmenting accountability [46]. Amnesty International concluded in 2026 that generative AI systems built on unlawfully scraped data are incompatible with international human rights law [47].

**Mitigation approaches:**

- Preference for open models where viable. See [recommended open models page](/recommended-open-models/).
- Maintain tooling flexibility to reduce dependence on a single corporate provider.
- Ensure transparency in usage, especially when contributing to open source.
- Advocate for equitable AI access through support of local, open solutions.

### 1.5 Intellectual Property and Copyright

Current AI models raise unresolved questions about copyright. The LLVM Project's AI policy states it clearly: using AI tools to regenerate copyrighted material does not remove the copyright, and contributors remain responsible for ensuring nothing infringing enters their work [4]. The risk includes inadvertently incorporating copyrighted code or text into publicly released outputs.

The law is getting clearer on three points. You only own AI-generated code if you meaningfully shaped it; prompting alone does not make you the author [56], and code nobody owns cannot be covered by your project's license. The main live lawsuit against GitHub Copilot is not about copying code, but about license and credit notices being stripped out [59], so keep those notices intact. Legal trouble has so far landed on the companies that trained on pirated material, not on the people using the tools [58]. In the EU, providers must now publish a summary of their training data [57], which is worth checking when picking a tool.

There is no way to fully avoid the underlying ethical problem - training material taken without consent - short of not using the models. Most models called 'open' only release their weights, and their training data carries the same problems as proprietary models. A small number are trained only on openly licensed material - see [recommended open models](/recommended-open-models/).

**Mitigation approaches:**

- Contributors must review and understand generated code, without blindly accepting it.
- Focus copyright review on the novel parts. Boilerplate and conventional patterns are not copyrightable whoever writes them; but if generated code contains a genuinely original approach or algorithm, assume it came from someone else's work and do not use it.
- Use code analyzers and copyright detection tools such as ScanCode Toolkit, built into CI pipelines, to catch copied code and missing license headers.
- Substantial AI-generated contributions must be disclosed in commit messages.
- Check that your AI tool's terms of service are compatible with your project's license.
- Recognize that producing open-source work means it may itself become training data - but that does not remove the responsibility to ensure no infringements enter the commons.

### 1.6 Digital Divide and Equity

AI coding tools are already more accessible to people in wealthy countries, and as the technology industry attempts to recoup its enormous capital investments, prices are likely to rise. At the same time, AI tools are eroding the equitable commons of free and open-source knowledge and universally accessible knowledge bases like Stack Exchange. There is a real risk of a two-tier system developing: massively powerful tools running in corporate data centers for the well-resourced, much less capable local instances for everyone else, and a diminished shared commons between the two [5].

Connectivity and access are not automatically benefits. Scholars of digital colonialism describe a recurring discourse of benevolence, where projects framed as bridging divides extend extraction and dependency instead [50]. Support for AI adoption should follow community consultation and demonstrated need, not the assumption that access to these tools is inherently good.

**Mitigation approaches:**

- Produce open-source software that partners can adopt freely.
- Advocate for and invest in open-source models that can run locally.
- Avoid locking into a single proprietary ecosystem.
- Design systems that remain maintainable without AI dependence.
- Follow the lead of local partners: support AI adoption only where communities identify a need, and avoid creating dependency on corporate platforms.

### 1.7 Economic and Financial Risk

Depending on AI tools is also a financial bet. Proprietary LLM pricing is volatile, models are deprecated with little notice, and current prices are widely believed to be subsidised by investor capital. The Bank for International Settlements warns that an AI investment bust, opaque circular financing between AI firms, and record debt levels are interlocking risks to the wider financial system [48]. The externalities are already visible: AI datacenter demand has driven severe global memory price rises, with DRAM contract prices up around 60% in a single quarter of 2026 [49], raising hardware costs for organizations and communities that never touch an LLM.

**Mitigation approaches:**

- Avoid hard dependencies on a single provider, and keep workflows functional without AI.
- Preference open models that can be self-hosted if provider pricing changes.
- Budget for rising hardware costs affecting both you and your partners.

---

## 2. AI in Open Source: Responsibility, Pressure, and Maintenance

AI affects not just how we code - but how we participate in the commons.

### 2.1 Asymmetric Pressure and Extractive Contributions

Dries Buytaert, lead of the Drupal project, describes the core problem precisely: AI makes it cheaper to contribute, but it does not make it cheaper to review [6]. More contributions are flowing into open-source projects, but the burden of evaluating them still falls on the same small group of maintainers. This creates asymmetric pressure that risks burning out the people who hold projects together [51].

The LLVM Project introduced the concept of an "extractive contribution" - one where the cost to maintainers of reviewing it exceeds the benefit to the project [4]. Before AI, posting a change for review signalled genuine interest from a potential long-term contributor. AI has decoupled effort from intent. A drive-by contributor can now generate a large patch in minutes and shift hours of review work onto volunteers.

Daniel Stenberg, maintainer of curl, canceled the project's bug bounty program after AI-generated reports flooded his seven-person security team - fewer than one in twenty turned out to be real bugs. Yet in the same period, an AI security startup used AI well and found all 12 zero-day vulnerabilities in a recent OpenSSL security release, some hiding for over 25 years [7]. The difference was not whether AI was used. It was expertise and intent behind the contributions. Further to this point, a study of agent-assisted pull requests on Github showed hand-written PRs are more frequently trusted and merged, with agent-assited PRs having a near 50% chance of needing reviewer revisions [52].

AI-generated code also frequently reinvents the wheel - producing custom implementations rather than leveraging well-tested community libraries. This creates fragmentation and shifts maintenance burden onto the ecosystem [8]. Repository data bears this out: an analysis of 623 million code changes found code reuse falling by a third, refactoring dropping from 21% to under 4% of changed lines, and duplicated code blocks up 81% since AI assistance became widespread [53].

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

Project responses range from cautious acceptance to outright bans. The landscape is moving fast, but the following represent the most significant approaches as of 2026. Notably, the platforms hosting open-source projects have been slow to provide maintainer tooling for filtering or flagging AI-generated contributions - several projects cite this as a direct driver of their restrictive policies [10]. OSS foundations, meanwhile, have largely focused on licensing questions rather than the quality and burnout crisis maintainers are facing now [10].

For an ongoing view, RedMonk maintains a policy landscape covering 86 organizations [60], and a community-maintained table tracks over 300 project policies [61].

**Disclosure and accountability:**

- **LLVM Project** - AI use permitted but must be disclosed. Contributors must understand and explain their code. "Good first issues" are reserved for human learning [4].
- **Linux Kernel** - AI code accepted, but undisclosed use results in the contributor being banned [12].
- **cURL** - AI contributions accepted with mandatory disclosure. Policy breach means a permanent ban. The bug bounty was canceled after AI reports overwhelmed the team - only 5% of submissions identified real vulnerabilities [13], [10].
- **CloudNativePG** - Permits AI-assisted contributions under strict human accountability rules. Contributors must fully understand and maintain AI-generated code, disclose usage via commit trailers, and _guarantee legal provenance_. "Shotgun refactoring" (wide-scale refactoring or clean-up), hallucinated features, and AI-written PR descriptions are explicitly prohibited. Maintainers reserve the right to close low-effort AI PRs without detailed critique [19].
- **Apache Spark** - Every PR must disclose AI use. Of ~8,500 commits over 2.5 years, only ~1.5% disclosed AI, but the rate is accelerating sharply [11].
- **GeoServer** - Permits AI-assisted contributions with responsibility, understanding, and correctness requirements. Additional feasibility guidance on contributor overreach, and maintainability guidance to address complexity. AI policy extends to use for security vulnerability reports and community communication channels [28].
- **Kubernetes** - AI use must be disclosed in PR descriptions, but AI must not be credited as a co-author, and trailers like `Assisted-by` are prohibited on the grounds that they dilute human accountability [29].

**Still navigating:**

- **CPython** - No formal policy yet, though AI-co-authored commits are appearing. Python's centrality to the AI ecosystem makes its eventual stance significant [11].
- **QGIS / GDAL** - Drafting transparency and accountability policies [14, 15].
- **OpenDroneMap** - Still in active discussion with differing opinions within the community [16].
- **Debian** - A General Resolution on LLM usage went to a vote in August 2026, with proposals ranging from a full ban written into the Social Contract to conditional acceptance [17].

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
2. **Transparency is mandatory.** When AI is used, it should be disclosed - in code commits, in documents, in reports. Disclosure must never be punished: research shows that when disclosing AI use attracts stigma, people simply hide it [39].
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
17. [Debian General Resolution: LLM usage in Debian.](https://www.debian.org/vote/2026/vote_002)
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
28. GeoServer Project. [AI Policy.](https://geoserver.org/ai)
29. Hannon, K. (2026). [Open Source Maintainership in the Age of AI.](https://kubernetes.io/blog/2026/06/26/open-source-maintainership-in-the-age-of-ai/) Kubernetes Blog.
30. Data Privacy + Security Insider (2026). [When Chats Become Evidence: Court Affirms Order Requiring OpenAI to Produce 20 Million De-Identified ChatGPT Logs.](https://www.dataprivacyandsecurityinsider.com/2026/01/when-chats-become-evidence-court-affirms-order-requiring-openai-to-produce-20-million-de-identified-chatgpt-logs/)
31. Cloud Security Alliance (2026). [Agentjacking: MCP Injection Hijacks AI Coding Agents.](https://labs.cloudsecurityalliance.org/research/csa-research-note-agentjacking-mcp-sentry-injection-20260612/)
32. Help Net Security (2026). [Prompt Injection Still Drives Most Agentic AI Security Failures in Production.](https://www.helpnetsecurity.com/2026/06/11/owasp-prompt-injection-ai-security-failures/) Reporting on OWASP GenAI Security Project, _State of Agentic AI Security and Governance_ v2.01.
33. Anthropic (2026). [Project Glasswing: An Initial Update.](https://www.anthropic.com/research/glasswing-initial-update)
34. Oviedo, F. et al. (2026). [Energy Use of AI Inference: Efficiency Pathways and Test-Time Scaling.](https://arxiv.org/abs/2509.20241) Joule.
35. Jegham, N. et al. (2025). [How Hungry is AI? Benchmarking Energy, Water, and Carbon Footprint of LLM Inference.](https://arxiv.org/abs/2505.09598)
36. Masley, A. (2025). [A Cheat Sheet for Conversations About AI's Environmental Impact.](https://blog.andymasley.com/p/a-cheat-sheet-for-conversations-about) Argues individual AI use is climate-negligible; cited here as a counterpoint.
37. Zitron, E. (2026). [The More You Buy, The More You Lose.](https://www.wheresyoured.at/the-more-you-buy-the-more-you-lose/) Where's Your Ed At. Documents hyperscaler capacity commitments, circular financing between AI labs and their suppliers, and the resulting component demand loop.
38. METR (2026). [We Are Changing Our Developer Productivity Experiment Design.](https://metr.org/blog/2026-02-24-uplift-update/)
39. Schilke, O. & Reimann, M. (2025). [The Transparency Dilemma: How AI Disclosure Erodes Trust.](https://doi.org/10.1016/j.obhdp.2024.104405) Organizational Behavior and Human Decision Processes.
40. Data Center Watch (2026). [$64 Billion of Data Center Projects Blocked or Delayed.](https://www.datacenterwatch.org/report)
41. Food & Water Watch (2026). [How to Stop a Data Center Near You.](https://www.foodandwaterwatch.org/2026/03/05/how-to-stop-a-data-center-near-you/)
42. Brookings (2026). [Data Center Moratoriums Are Not a Substitute for Oversight.](https://www.brookings.edu/articles/data-center-moratoriums-are-not-a-substitute-for-oversight/)
43. Perrigo, B. (2023). [OpenAI Used Kenyan Workers on Less Than $2 Per Hour.](https://time.com/6247678/openai-chatgpt-kenya-workers/) Time.
44. The Guardian (2026). ["In the End You Feel Blank": India's Female Workers Watching Hours of Abusive Content to Train AI.](https://www.theguardian.com/global-development/2026/feb/05/in-the-end-you-feel-blank-indias-female-workers-watching-hours-of-abusive-content-to-train-ai)
45. Fairwork (2025). [Cloudwork Ratings 2025.](https://fair.work/en/fw/publications/cloudwork-report-2025/) Oxford Internet Institute.
46. SOMO (2026). [Big Tech Sets Unfair Terms and Conditions for AI Data Workers Globally.](https://www.somo.nl/big-tech-sets-unfair-terms-and-conditions-for-ai-data-workers-globally/)
47. Amnesty International (2026). [Violations in the Shell: Exposing the Human Rights Costs of Generative AI.](https://www.amnesty.org/en/documents/pol40/0996/2026/en/)
48. Bank for International Settlements (2026). [Annual Economic Report 2026.](https://www.bis.org/publ/arpdf/ar2026e.htm)
49. Tom's Hardware (2026). [Memory Price Surge Begins to Cool as Consumers Hit Affordability Limit.](https://www.tomshardware.com/pc-components/ram/memory-price-surge-begins-to-cool-as-consumers-hit-affordability-limit-ai-demand-still-keeps-dram-and-nand-prices-climbing-through-q3-2026)
50. Nothias, T. (2025). [An Intellectual History of Digital Colonialism.](https://doi.org/10.1093/joc/jqaf003) Journal of Communication.
51. Baltes, S., Cheong, H. & Treude, C. (2026). ["An Endless Stream of AI Slop": How Developers Discuss the Burden of AI-Assisted Software Development.](https://arxiv.org/abs/2603.27249)
52. Watanabe, Y. et al. (2026). [On the Use of Agentic Coding: An Empirical Study of Pull Requests on GitHub.](https://arxiv.org/abs/2509.14745)
53. GitClear (2026). [The AI Code Quality Maintainability Gap.](https://www.gitclear.com/the_ai_code_quality_maintainability_gap)
54. Nhando, D. (2026). [Uncovering the Humanitarian and Nonprofit Sectors' AI Governance Crisis.](https://www.techpolicy.press/uncovering-the-humanitarian-and-nonprofit-sectors-ai-governance-crisis/) TechPolicy.Press.
55. Parkinson, K.M. (2026). [How Are Humanitarians Using AI in 2026?](https://www.humanitarianleadershipacademy.org/resources/opinion-how-are-humanitarians-using-ai-in-2026-the-case-for-governance-and-local-leadership/) Humanitarian Leadership Academy.
56. US Copyright Office (2025). [Copyright and Artificial Intelligence, Part 2: Copyrightability.](https://www.copyright.gov/ai/)
57. EU AI Act. [Article 53: Obligations for Providers of General-Purpose AI Models.](https://artificialintelligenceact.eu/article/53/)
58. Authors Guild (2025). [What Authors Need to Know About the Anthropic Settlement.](https://authorsguild.org/advocacy/artificial-intelligence/what-authors-need-to-know-about-the-anthropic-settlement/)
59. Joseph Saveri Law Firm. [GitHub Copilot Litigation.](https://githubcopilotlitigation.com/)
60. Holterhoff, K. (2026). [Generative AI Policy Landscape in Open Source.](https://redmonk.com/kholterhoff/2026/02/26/generative-ai-policy-landscape-in-open-source/) RedMonk.
61. [Open Source AI Contribution Policies (community-maintained).](https://github.com/melissawm/open-source-ai-contribution-policies)

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
[17]: https://www.debian.org/vote/2026/vote_002 "Debian General Resolution: LLM usage in Debian."
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
[28]: https://geoserver.org/ai "GeoServer Project AI Policy."
[29]: https://kubernetes.io/blog/2026/06/26/open-source-maintainership-in-the-age-of-ai/ "Kubernetes: Open Source Maintainership in the Age of AI."
[30]: https://www.dataprivacyandsecurityinsider.com/2026/01/when-chats-become-evidence-court-affirms-order-requiring-openai-to-produce-20-million-de-identified-chatgpt-logs/ "When Chats Become Evidence: OpenAI ordered to produce 20 million ChatGPT logs."
[31]: https://labs.cloudsecurityalliance.org/research/csa-research-note-agentjacking-mcp-sentry-injection-20260612/ "Agentjacking: MCP Injection Hijacks AI Coding Agents."
[32]: https://www.helpnetsecurity.com/2026/06/11/owasp-prompt-injection-ai-security-failures/ "Prompt injection still drives most agentic AI security failures in production."
[33]: https://www.anthropic.com/research/glasswing-initial-update "Anthropic: Project Glasswing initial update."
[34]: https://arxiv.org/abs/2509.20241 "Energy Use of AI Inference: Efficiency Pathways and Test-Time Scaling."
[35]: https://arxiv.org/abs/2505.09598 "How Hungry is AI? Benchmarking Energy, Water, and Carbon Footprint of LLM Inference."
[36]: https://blog.andymasley.com/p/a-cheat-sheet-for-conversations-about "Masley, A. A cheat sheet for conversations about AI's environmental impact."
[37]: https://www.wheresyoured.at/the-more-you-buy-the-more-you-lose/ "Zitron, E. The More You Buy, The More You Lose."
[38]: https://metr.org/blog/2026-02-24-uplift-update/ "METR 2026 uplift update."
[39]: https://doi.org/10.1016/j.obhdp.2024.104405 "The Transparency Dilemma."
[40]: https://www.datacenterwatch.org/report "Data Center Watch report."
[41]: https://www.foodandwaterwatch.org/2026/03/05/how-to-stop-a-data-center-near-you/ "How to Stop a Data Center Near You."
[42]: https://www.brookings.edu/articles/data-center-moratoriums-are-not-a-substitute-for-oversight/ "Data Center Moratoriums Are Not a Substitute for Oversight."
[43]: https://time.com/6247678/openai-chatgpt-kenya-workers/ "OpenAI Used Kenyan Workers on Less Than $2 Per Hour."
[44]: https://www.theguardian.com/global-development/2026/feb/05/in-the-end-you-feel-blank-indias-female-workers-watching-hours-of-abusive-content-to-train-ai "India's Female Workers Watching Hours of Abusive Content to Train AI."
[45]: https://fair.work/en/fw/publications/cloudwork-report-2025/ "Fairwork Cloudwork Ratings 2025."
[46]: https://www.somo.nl/big-tech-sets-unfair-terms-and-conditions-for-ai-data-workers-globally/ "Big Tech Sets Unfair Terms for AI Data Workers."
[47]: https://www.amnesty.org/en/documents/pol40/0996/2026/en/ "Violations in the Shell."
[48]: https://www.bis.org/publ/arpdf/ar2026e.htm "BIS Annual Economic Report 2026."
[49]: https://www.tomshardware.com/pc-components/ram/memory-price-surge-begins-to-cool-as-consumers-hit-affordability-limit-ai-demand-still-keeps-dram-and-nand-prices-climbing-through-q3-2026 "Memory Price Surge Begins to Cool."
[50]: https://doi.org/10.1093/joc/jqaf003 "An Intellectual History of Digital Colonialism."
[51]: https://arxiv.org/abs/2603.27249 "An Endless Stream of AI Slop."
[52]: https://arxiv.org/abs/2509.14745 "On the Use of Agentic Coding."
[53]: https://www.gitclear.com/the_ai_code_quality_maintainability_gap "The AI Code Quality Maintainability Gap."
[54]: https://www.techpolicy.press/uncovering-the-humanitarian-and-nonprofit-sectors-ai-governance-crisis/ "The Humanitarian and Nonprofit Sectors' AI Governance Crisis."
[55]: https://www.humanitarianleadershipacademy.org/resources/opinion-how-are-humanitarians-using-ai-in-2026-the-case-for-governance-and-local-leadership/ "How Are Humanitarians Using AI in 2026?"
[56]: https://www.copyright.gov/ai/ "Copyright and Artificial Intelligence, Part 2."
[57]: https://artificialintelligenceact.eu/article/53/ "EU AI Act Article 53."
[58]: https://authorsguild.org/advocacy/artificial-intelligence/what-authors-need-to-know-about-the-anthropic-settlement/ "The Anthropic Settlement."
[59]: https://githubcopilotlitigation.com/ "GitHub Copilot Litigation."
[60]: https://redmonk.com/kholterhoff/2026/02/26/generative-ai-policy-landscape-in-open-source/ "Generative AI Policy Landscape in Open Source."
[61]: https://github.com/melissawm/open-source-ai-contribution-policies "Open Source AI Contribution Policies."

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

- A 2026 peer-reviewed analysis of production LLM inference measured a median of 0.31 Wh per typical query, and 3.91 Wh for long reasoning queries, including datacenter overhead [34].
- Earlier public estimates, including the Epoch AI figure this appendix previously used [23], overstated per-query energy by 4-20x by ignoring production optimizations [34].
- Coding assistance leans heavily on long context and reasoning, and agentic workflows chain many queries per prompt, so we take roughly double the long-query median: ~8 Wh per prompt.

Energy per query: ~0.008 kWh

**51.2 kWh usage per month** (for a 5 person team)

### 3. Convert Electricity to CO₂ Emissions

- This depends on **where the compute actually runs**, which is hard to determine.
- For example, OpenAI API inference can be hosted in multiple regions (Europe, USA, Singapore, Japan, India), as per their [data residency docs](https://help.openai.com/en/articles/9903489-data-residency-and-inference-residency-for-chatgpt).
- Based on an International Energy Agency 2025 report [25], global grid CO₂ intensity can be estimated at ~445 gCO₂/kWh.
- More accurate estimates per country can be found via the [Electricity Maps Project](https://www.electricitymaps.com).

tCO₂e = kWh_total × (gCO₂/kWh / 1000 / 1000)

**0.023 tonnes CO₂ equivalent produced per month**

- Note that datacenters also consume fresh water: roughly 0.2 to 1.2 litres per kWh on site for cooling, plus around 4 to 6 litres per kWh in the electricity supply chain [35]. At the usage above, that is on the order of a few hundred litres per month.

### 4. Convert Emissions to Donation Proxy

- Research by Founders Pledge, suggests that donating 1000 USD to effective climate advocacy charities could avert approximately 100 tCO₂ from being omitted (expected-value estimate for high-impact policy/advocacy orgs) [26].
- **This does not offset emissions made, nor negate your personal responsibility to reduce your footprint**.
- However, considering how uncommon this type of donation is (for corporate entities that typically do not care much), it could be argued that this is an acceptable mitigation strategy at this scale, by proxy.
- Let's add another fudge factor of 100x to account for additional uncertainties: model used, frequency of use, and inaccuracies in various approximations. This gives us a nicely pessimistic estimate of ~2.3 tCO₂.
- This 100x factor is a deliberate margin of safety, not an emissions estimate: the unmultiplied proxy cost is only a few dollars per year, and good-faith critics argue that multipliers like this overstate the real footprint [36]. We keep it because it also stands in for training amortisation and for costs we cannot price here.

Recommendation: **~23 USD donation per month**, for a team of 5 devs using LLM assistance.

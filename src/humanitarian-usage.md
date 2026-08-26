---
layout: base.njk
title: Using LLMs in a Humanitarian Context
description: Humanitarian response has both urgency and complexity that needs to be navigated.
---

# Using LLMs in a Humanitarian Context

**Standing up new software during an emergency can feel empowering. It's worth being honest about both its potential to help and its potential to get in the way.**

---

Nothing here is an argument against building software. Some of the most useful tools in this sector were written in days, by people living through a crisis. But it's worth knowing what actually holds a response back, before reaching for the tooling.

## 1. Response Is a Coordination Problem First

Responding to a crisis is primarily a logistical challenge. Dozens of organizations, government agencies and volunteer groups have to work from a shared picture:

- **One place to register missing people**, so families search once and duplicates can be resolved.
- **One assessment of where aid is needed**, so supplies are not delivered three times to one neighbourhood and never to the next.
- **Agreed data standards**, so what volunteers collect can actually be used by the agencies with the resources to act on it.

Almost every hard problem in a response is one of centralization and interoperability, not missing software. Collaboration is the single largest determinant of whether a response succeeds - and the one thing no tool can do for you.

## 2. The Temptation to Vibe-Code an Emergency

LLMs make it possible to stand up a map, a form and a database in an afternoon. Sometimes that is useful: a tool built by people with local knowledge can fill a real gap faster than any procurement process, and communities have every right to build for themselves.

But speed of building was never the bottleneck. The bottleneck is trust, verification and coordination - and those cannot be prompted into existence.

A tool built in isolation, however good, tends to:

- **Fragment the picture.** Every new missing-persons database splits the search across another silo, and makes "how many people are still unaccounted for?" harder to answer.
- **Distort demand signals.** Uncoordinated damage or needs maps produce conflicting numbers that responders must reconcile before they can act.
- **Create data protection risk.** Missing-persons records, locations and photographs of damaged homes are sensitive personal data. A prototype rarely has a lawful basis, a retention policy or a security review.
- **Publish unverified data at scale.** Automated scraping of social media into a live map propagates rumour with the authority of a dashboard.
- **Present model output as fact.** Automated building-damage detection after the 2026 Venezuela earthquakes caught most damaged buildings, but fewer than one in ten buildings it flagged were actually damaged; only where several independent products agreed did a flag become reliable. That is good enough to rank which neighbourhoods to visit first, and nowhere near good enough to say whether one building is safe - and a blank space on the map may mean "undamaged" or just "never looked at".
- **Be abandoned.** Initial response lasts weeks, but rebuilding lasts years. Tools built in week one are often unmaintained by month three, while people still depend on them.

A working tool is only a small part of a service. It has to fit how an organization already works, have someone to answer users and fix their records, and carry a name people already trust. Families give a missing relative's details to an agency they recognise, not to an unfamiliar domain.

Proliferation itself does harm too. The well-known version is unsolicited donated goods: containers of well-meant supplies clogging the customs and warehouse capacity that prioritised relief needs, then spoiling. Every consignment was well intended; the aggregate was a second emergency. A dozen overlapping dashboards do the same to attention, data and trust.

There's rarely bad intent involved. Usually it's just not knowing what already exists.

## 3. Before You Build, Ask

Use LLM-assisted tooling with caution in an active response, ideally in consultation with the primary actors: national disaster management agencies, local government, and the established coordination bodies.

1. **Does this already exist?** Ask the coordinating agencies before assuming there is a gap. There may already be systems in place.
2. **Who asked for it, and whose job does it become?** Name the team and the decision it feeds, and ask what they are actually deciding this week. Search teams working building-by-building on the ground often need imagery of one inaccessible area far more than another damage map. A tool nobody requested is a tool nobody will use.
3. **Can I contribute to an existing effort instead?** Adding to an established platform is almost always higher impact than launching a competing one - especially one the users already trust.
4. **Who answers the users?** Questions, corrections and retraining are usually a bigger commitment than the software. Each extra tool also costs responders time, working out which of a dozen sources is the most accurate and the most current.
5. **Where does the data go?** If it does not flow into the official systems, in a format they can ingest, it will not reach the people making response decisions.
6. **What is my duty of care?** You are collecting sensitive data about people at their most vulnerable. Consent, minimisation, retention and deletion are not optional because it is an emergency.
7. **Who maintains this in six months?** A suitable handover plan should be in place, else the data migrated.

Answer those honestly and it may well be appropriate to build. If you do: publish openly, use recognised humanitarian data standards, make the data exportable, keep it light enough to open on a poor connection, say plainly what it does not cover, and offer it to the coordinating bodies rather than competing with them.

## 4. Case Study: Venezuela / Colombia Earthquake, June 2026

Many community mapping and reporting initiatives launched within days of the June 2026 earthquake, several with AI-assisted development. Some provided real value - but collectively they illustrate the fragmentation problem.

**Missing persons** - at least four separate public databases ran in parallel:

| Initiative                            | Notes                                                                       |
| ------------------------------------- | --------------------------------------------------------------------------- |
| `sismovenezuela.com`                  | 14,000+ missing persons aggregated from 2 sources; also ingested YouTube, X/Twitter and Instagram posts every 10 minutes |
| `venezuelatebusca.com`                | Search by name and ID number; synchronised with SismoVenezuela every 30 minutes |
| `venezuelareporta.org`                | 15,977 reported missing, 633 located; explicitly unverified, community-reported |
| `desaparecidosterremotovenezuela.com` | 8,000+ records with a public API; also synchronised every 30 minutes         |

Some built cross-synchronisation and duplicate detection between each other: engineering effort spent solving a problem created by the fragmentation itself. Meanwhile no single number for "who is still missing" existed, and families had to search several sites, in several formats, to be sure.

**Damage, needs and aid coordination** - a further set of overlapping tools, including `terremotovenezuela.com` (building-damage map, 153 buildings reported), `terremotovenezuela.app` (open-source rescue coordination), `ayudacolombia.xyz`, `sismoayudave.com`, `radarvenezuela.org`, `crisisvenezuela.org`, `crisis-pulse-ve.netlify.app`, `reportaven.com`, `terremoto.hazlohoy.org`, `red-de-esperanza-lime.vercel.app`, and a Sentinel imagery-derived damage cost estimator.

Note that `terremotovenezuela.com`, `terremotovenezuela.app` and `sismovenezuela.com` were three different projects with near-identical names - confusing for anyone trying to report or find information under pressure.

**Institutional and professional efforts running alongside:**

- [Cartografía de Evaluación de Daños - Estado La Guaira](https://drive.google.com/drive/folders/1vT99wmn2CqNlucLeaVa00FrBKKDz_alQ?usp=drive_link) - 61 standardised, openly licensed maps at 1:3,000 produced by university technical volunteers (USB / UCV / UNC-CH and others), designed to support search-and-rescue quadrant assignment and EDAN damage assessment by any responding body.
- [Colegio de Ingenieros de Venezuela building assessments](https://danielleon.maps.arcgis.com/apps/dashboards/63964f8102b74b1b8b6907db536d4c74) - georeferenced structural evaluations by qualified engineers.
- [ESRI Spain disaster response hub](https://drp-venezuela-disastersesriven.hub.arcgis.com/) and [wider disaster response geodata collections](https://rodolfofrancoweb.com/geodata/geodata_mundial/geodata-riesgos-y-desastres/geodata_de_respuesta_a_desastres/).

**The same thing happened above the community level.** More than a dozen automated building-damage layers were produced for this earthquake by international agencies and research bodies. No two of them agreed on more than half the buildings they covered. Field teams were left to judge for themselves which was current and which was accurate - work that fell to the people with the least time for it.

### What to take from this

There was no shortage of capable people, and no shortage of software. Effort was simply spread across a dozen incompatible systems instead of a few authoritative ones. The most useful initiatives - the La Guaira cartography being the clearest example - produced standardised, openly licensed, exportable data that existing responders could act on, rather than competing to be the definitive front-end. Worth noting who was behind them: universities and a professional engineering body, already known to the agencies. That's a large part of why the work got used.

Hindsight is 20:20, and this is no criticism of people building for their own communities during a disaster. But the same energy, focused on a shared goal, goes a lot further.

So the lesson is not "do not build". It is: **find out what exists, talk to the people coordinating the response, and add to the shared picture rather than starting another one.**

Build *with* the responders, not in parallel to it.

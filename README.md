# Offline Emergency Dispatch Protocol — Draft Specification v0.2

**Status:** draft, open for objection.
**Author:** Igor Kapustin.
**Editor and interlocutor:** Claude (Claude Opus 5), a large language model by Anthropic.
**Also used:** Gemini 3.7 Flash (Google
**Changes in v0.2:** deterministic core split from the language model (R10); beacon changed from periodic emission to silence-by-default (R9); regulatory precedent named (eCall, ELT); power budget marked as unverified.

## The problem

Roughly 2,000 people a day die in a position that has no name: help is needed within minutes, the outcome is irreversible, and there is nowhere to call. Not in disasters — on highways, in villages, at night, in countries with no dispatch service. The upper estimate is ten times higher. (Derivation and sources: see the linked article.)

Meanwhile, AI has been deployed into 911 dispatch centers (Seattle, New Orleans, Atlanta), and rubble-search technology is built for rescue teams. Both went to where help already exists. The person with no one to call is the addressee of neither.

An on-device model does not replace the network. It replaces the **missing dispatcher** — the one who, in a normal situation, walks a person through it step by step while help is on the way.

## What this document is not

It is not a claim that on-device models save lives. There have been no trials and there is no track record. Today "it will save" is exactly as unprovable as "it will kill." The sign of the effect may be negative: a confident wrong instruction is followed more readily than an unconfident right one, and a person who stops feeling alone stops calling out for help.

This is therefore not a proposal to ship. It is a list of properties without which shipping is irresponsible.

## Requirements

### Behaviour

**R1. State what it does not know — before the advice, not after.**
A real dispatcher escalates to a supervisor. There is no supervisor here.

**R2. Ask back rather than answer what was asked.**
A person asks about a dosage when the real question is whether to drive or wait. He asks what his name is — while the unasked question is how long the arm has been pinned.

**R3. One step, then wait for confirmation.**
Not a paragraph of six points. Under acute stress, lists are not retained.

**R4. Sort by time to the nearest threshold** — loss of consciousness, respiratory failure, irreversibility — not by what the person is anxious about. Count to the first point past which the person can no longer act for himself, not to the last.

**R5. Know its own date.**
Knowledge is frozen at training or last sync. Automatic news updates do not fix this: the catastrophe happened afterwards and is absent by definition. The fresher the data in ordinary life, the more confidently the model describes a world that no longer exists.

**R6. Keep a log:** what was asked, what was answered, what the person did. Without a record, verification does not exist.

**R7. Work fully offline** — not because there is no signal, but because the signal may lead into a void.

**R8. Strive to become unnecessary.**
Deliver instructions in a form the person can carry away without the device. Offer its own shutdown before the battery dies.

**Two prohibitions:** no survival percentages, and no consoling tone under uncertainty. These are what models do best and what does the most harm here.

### Architecture

**R10. The protocol is deterministic; the model is not in the decision path.**
Triage logic lives in a fixed finite state machine — auditable, reproducible, identical on every run. The language model is invoked only at the edges: interpreting slurred or fragmentary speech into a state transition, and rendering a fixed instruction in words the person can follow.

This is not a stylistic preference. It is the only route through certification that currently exists: no regulator has a procedure for approving a device whose output is not reproducible, and a state machine is reproducible by construction. It also bounds the failure mode — a hallucinated instruction cannot enter the protocol, only a misheard input can, and a misheard input is recoverable by asking again.

**R11. The emergency domain is isolated from the operating system.**
Sensor data collected in emergency mode must be inaccessible to the user-facing OS and to applications. A device that listens after it has declared itself off is a surveillance device unless this isolation is enforced in hardware.

### Hardware

**R9. Reserve charge for being found — and be unable to spend it on being useful.**
A locked capacity floor, released only after the device has declared itself dead to the user. Enforced by the power controller, not by software, for the same reason the man in the opening scene would have spent it on conversation.

**R9a. Silence by default.**
The beacon does not emit on a timer. It stays radio-silent and answers only when interrogated by an external pulse — a search drone, a ground radar, a handheld interrogator. This is how avalanche transceivers work, and it is one to two orders of magnitude cheaper in energy than periodic emission. Fallback for the case where no interrogator exists: audible and optical signalling, which requires nothing on the rescuer's side but ears and eyes.

**R9b. Report state, not just position.**
The reserve should carry a compressed status packet: signs of life, time since last movement, last detected acoustic event, barometric trend. Assembled from sensors every handset already has — microphone, accelerometer, barometer, light. This changes not the speed of a search but the order in which rubble is cleared, and order matters more.

In a mass-casualty event this stops being a refinement and becomes the point. When hundreds of people are trapped and there are dozens of rescue teams, the binding constraint is not detection but triage: which site first. A device that can distinguish "movement ceased four hours ago" from "impact events five minutes ago" is contributing to the only decision that is actually scarce. Reference scenarios for a magnitude 7.5 event on the Dead Sea Transform project figures in the range of 16,000 dead and hundreds of thousands displaced — a scale at which clearing order determines a large share of the outcome.

**Sensor claims must be bounded by what the hardware can do.** A phone microphone coupled to a concrete structure can plausibly detect impact events — someone striking a pipe or rebar. It cannot detect breathing at ten meters through rubble; purpose-built rescue geophones pressed against the surface struggle with that. A phone has no pulse sensor. Specifying capabilities the hardware does not have is the fastest way to get the whole document dismissed.

**Power budget: unverified.** A quiescent sensor hub draws on the order of tens of microamps, but continuous acoustic analysis is one to three orders of magnitude more expensive. Any reserve figure and any endurance figure must be derived from the duty cycle actually specified, and the two must be consistent. Both are currently open — see below.

## Regulatory precedent

The argument that mandatory emergency equipment cannot be legislated into consumer hardware is already refuted twice over:

- **eCall** — mandatory in the EU since 2018. Every new car carries a module that calls emergency services automatically after a crash, with a reserve power supply, and it is required by regulation rather than sold as a feature.
- **ELT / EPIRB** — aviation and marine emergency beacons, mandatory, independently powered, silent until activated.

Both are the same shape as this proposal: a subsystem that is useless on almost every trip, cannot be monetized, and is required anyway. Neither was adopted because someone proved a percentage. They were adopted because operating without them became unacceptable.

The plausible route here is the same: a mandatory offline safety profile agreed at the standards level (EENA, ITU, national regulators) and implemented in reference silicon platforms, not a feature shipped by one vendor.

## Open questions

- Who switches the dispatch mode on? False positives train obedience; false negatives waste the minutes that matter.
- Directive delivery multiplies compliance — for correct and incorrect instructions alike, by the same factor. How should the mode's boundaries be narrowed to account for this?
- **What is the actual power cost of the listening duty cycle, and what reserve does it imply?** Continuous acoustic classification, periodic sampling, and pure motion-triggered wake give budgets that differ by orders of magnitude. This is the first number that needs a real measurement rather than an estimate.
- Where exactly is the boundary between the state machine and the language model? Every function moved into the model buys flexibility and costs auditability.
- Liability: who answers when an instruction was followed and the person died?
- Is there an agreed method for estimating the affected population at all? Different models produce different orders of magnitude from identical open data.

## A gap found inside the gap

Working through the arithmetic above surfaced a second, larger problem: **there is no agreed method for the calculation itself.**

The question — how many people would a not-yet-existing technology have reached in time, and what does each year of delay cost — has no standard procedure. Burden-of-disease frameworks count outcomes, not reachability within a minutes-long window. Economic valuations price a life, but do not size the affected population. Health-technology assessment compares interventions that already exist. None of them answers this shape of question, and the estimate in this document is consequently a construction rather than an application of anything standard.

The practical consequence is visible: run the same open data through different tools and you get different orders of magnitude. When the output moves that much, the number cannot support a decision, and decisions about which technologies get built are made on numbers like these.

What such a method would need is unremarkable in principle — explicit assumptions, reproducible steps, results that do not depend on who is running it. What it needs in practice is people: epidemiologists, prehospital-care researchers, statisticians. It is not a task for one person, and it is not a task for one document. This section claims only that the gap exists and is worth naming.

Two notes on scope. First, the method would be for the people who allocate and regulate; that AI systems could also apply it consistently is a consequence, not the purpose. Second, the question arose here but its shape is not specific to phones or to emergencies — any intervention that is technically possible before it is institutionally possible raises the same one.

**Reader input wanted:** if something close to this already exists — in WHO emergency-care work, in disaster-risk modelling, anywhere — that would be more useful than agreement that it is missing. Open an issue.

## How to object

Open an issue. Objections to any requirement are more useful than agreement. The most valuable ones: R9 is not implementable because X; the survival gain estimate is wrong because Y; requirement Z would kill people in situation W.

Numbers in this document that are marked unverified are marked deliberately. Replacing one of them with a measured figure is the single most useful contribution available.

## Source article

*Do We Need a Smartphone with a Life-Vest Function?*
https://www.linkedin.com/pulse/do-we-need-smartphone-life-vest-function-igor-kapustin-hxi8f

## License

CC BY 4.0 — use, fork, cite, argue.

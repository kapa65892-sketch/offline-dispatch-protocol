# Offline Emergency Dispatch Protocol — Draft Specification v0.3

**Status:** draft, open for objection.
**Author:** Igor Kapustin.
**Editor and interlocutor:** Claude (Claude Opus 5), a large language model by Anthropic.
**Also used:** Gemini 3.7 Flash (Google), GPT-5.6 Sol (OpenAI).

**Changes in v0.3** — all four corrections came from outside review, and all four fixed errors in v0.2:
- R9a: the reference to avalanche transceivers was wrong. Those are active transmitters; the unpowered interrogated reflector is a different technology (RECCO). The corrected reference is noted, and a possible passive second layer is described as an open engineering question rather than as a requirement.
- R9a: the "one to two orders of magnitude cheaper" energy claim is withdrawn. It was an unmeasured figure in a document that forbids unmeasured figures.
- R10: the justification is now engineering, not regulatory. The claim that a deterministic core is "the only route through certification" was false — regulators already approve machine-learning medical devices, including ones with pre-authorised model change plans.
- R9b: barometers are not present in every handset. Sensor availability must degrade gracefully.

The four corrections shared a single pattern — a true statement placed at a higher level of certainty than the evidence supported. The reading rule at the top of the requirements section was extracted from that pattern and added so the same error can be caught without an external reviewer.

**Changes in v0.2:** deterministic core split from the language model (R10); beacon changed from periodic emission to silence-by-default (R9a); regulatory precedent named (eCall, ELT); power budget marked as unverified.

## The problem

Roughly 2,000 people a day die in a position that has no name: help is needed within minutes, the outcome is irreversible, and there is nowhere to call. Not in disasters — on highways, in villages, at night, in countries with no dispatch service. The upper estimate is ten times higher. (Derivation and sources: see the linked article.)

Meanwhile, AI has been deployed into 911 dispatch centers (Seattle, New Orleans, Atlanta), and rubble-search technology is built for rescue teams. Both went to where help already exists. The person with no one to call is the addressee of neither.

An on-device model does not replace the network. It replaces the **missing dispatcher** — the one who, in a normal situation, walks a person through it step by step while help is on the way.

## What this document is not

It is not a claim that on-device models save lives. There have been no trials and there is no track record. Today "it will save" is exactly as unprovable as "it will kill." The sign of the effect may be negative: a confident wrong instruction is followed more readily than an unconfident right one, and a person who stops feeling alone stops calling out for help.

This is therefore not a proposal to ship. It is a list of properties without which shipping is irresponsible.

## Requirements

### How to read this specification

Statements in this document sit at three different levels:

- **Requirement** — what the system must do, independent of how it is implemented.
- **Mechanism** — one possible way to satisfy a requirement. A mechanism is not a requirement and may be replaced without changing it.
- **Open question** — something not established by measurement, experiment, or field evidence. It must not be treated as a capability or used as a premise until resolved.

The same idea may move between these levels as evidence changes. Its place is determined by what is known, not by how plausible or useful the idea appears.

A correct statement can still sit at the wrong level, and that is the error this document has most often made. Reviewing it therefore means asking two questions, not one: is this true, and is it established well enough to stand where it stands.

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

The argument for this is engineering, not regulatory. Regulators do approve machine-learning devices — the FDA has authorised many, including devices shipping with pre-authorised plans for changing the model after approval — so non-determinism alone is not a bar to certification, and v0.2 was wrong to claim it was.

The reason to keep the model out of the decision path is narrower and stronger: it bounds the failure surface. A fabricated instruction cannot enter the protocol at all; only a misheard input can, and a misheard input is recoverable by asking again. It also makes the system testable in the ordinary sense — the same input produces the same path every time, so a failure can be reproduced, and a fix can be shown to work. In a domain where the user cannot verify the output and cannot undo the action, that is worth more than the flexibility it costs.

**R11. The emergency domain is isolated from the operating system.**
Sensor data collected in emergency mode must be inaccessible to the user-facing OS and to applications. A device that listens after it has declared itself off is a surveillance device unless this isolation is enforced in hardware.

### Hardware

**R9. Reserve charge for being found — and be unable to spend it on being useful.**
A locked capacity floor, released only after the device has declared itself dead to the user. Enforced by the power controller, not by software, for the same reason the man in the opening scene would have spent it on conversation.

**R9a. Silence by default.**
Nothing emits on a timer. Detection is answered, not broadcast.

*Interrogated response from reserve.* A low-power responder that stays silent until an external pulse arrives — a search drone, ground radar, a handheld interrogator — and then answers with the status packet described in R9b. This is what makes triage possible, and it is what the reserve in R9 is for.

*Fallback where no interrogator exists:* audible and optical signalling, which requires nothing on the rescuer's side but ears and eyes. What triggers it is undecided and matters more than it appears — user activation, an acoustic event nearby, or a slow periodic pulse are three different devices with three different energy budgets, and the third contradicts the rule above. Stated here as an open question rather than resolved by preference.

*A possible second layer, not a requirement.* Passive interrogated reflection — a component that needs no power at all and re-radiates a searcher's directed signal — would survive a fully dead device and carry one bit: something is here. RECCO demonstrates that the principle works in avalanche rescue, where the searcher carries the transmitter and the buried person carries an unpowered reflector. Whether an equivalent can be integrated into a handset is an open engineering question, not an established one: antenna geometry, frequency selection, detectability through building structures, and compatibility with the search equipment rescuers actually field are all unresolved. It is listed here because the split it suggests is worth considering — locating an object and reading its state need not depend on the same mechanism — not because it is known to be buildable.

Answering rather than broadcasting may reduce energy expenditure, depending entirely on the wake and listening architecture: a receiver that must stay alert has its own standing cost, and false wake-ups have theirs. Whether the net is favourable, and under which design, is unmeasured. See the power budget note below.

**R9b. Report state, not just position.**
The reserve should carry a compressed status packet of observations, not conclusions: movement events and time since the last one, acoustic events and time since the last one, and — where a barometer is present — pressure readings and their trend.

What can be inferred from those observations is a separate and unproven question. "Signs of life" is itself an inference, not a reading, and so are depth of burial and flooding risk: all are plausible candidates, not established capabilities. Nobody has shown what a handset barometer under rubble reads, or with what reliability. The packet should carry the measurement and leave the interpretation to whoever has validated it.

The observations come from sensors already present in handsets — microphone and accelerometer in effectively all of them, barometer and ambient light in many but not all. Availability varies by device, so the packet must degrade gracefully: absent a barometer, that field is simply missing, not the whole report.

What this changes is not the speed of a search but the order in which rubble is cleared, and order matters more.

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
- What triggers the audible/optical fallback without violating the no-timer rule?
- What does a handset barometer actually read under rubble, and what can be inferred from it? Depth and flooding risk are assumed here, not demonstrated.
- Can an unpowered interrogated reflector be integrated into a handset at all — antenna geometry, frequency, detectability through building structures, compatibility with fielded search equipment? The principle is demonstrated elsewhere; the implementation is not.
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

Every change in v0.3 came from someone pointing out an error rather than from the author improving his own text. That is the intended failure mode of this document, and objections that break something are welcome on the same terms.

## Source article

*Do We Need a Smartphone with a Life-Vest Function?*
https://www.linkedin.com/pulse/do-we-need-smartphone-life-vest-function-igor-kapustin-hxi8f

## License

CC BY 4.0 — use, fork, cite, argue.

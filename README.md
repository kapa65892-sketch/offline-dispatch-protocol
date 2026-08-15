# Offline Emergency Dispatch Protocol — Draft Specification v0.1

**Status:** draft, open for objection.
**Author:** Igor Kapustin.
**Editor and interlocutor:** Claude (Claude Opus 5), a large language model by Anthropic.

## The problem

Roughly 2,000 people a day die in a position that has no name: help is needed within minutes, the outcome is irreversible, and there is nowhere to call. Not in disasters — on highways, in villages, at night, in countries with no dispatch service. The upper estimate is ten times higher. (Derivation and sources: see the linked article.)

Meanwhile, AI has been deployed into 911 dispatch centers (Seattle, New Orleans, Atlanta), and rubble-search technology is built for rescue teams. Both went to where help already exists. The person with no one to call is the addressee of neither.

An on-device model does not replace the network. It replaces the **missing dispatcher** — the one who, in a normal situation, walks a person through it step by step while help is on the way.

## What this document is not

It is not a claim that on-device models save lives. There have been no trials and there is no track record. Today "it will save" is exactly as unprovable as "it will kill." The sign of the effect may be negative: a confident wrong instruction is followed more readily than an unconfident right one, and a person who stops feeling alone stops calling out for help.

This is therefore not a proposal to ship. It is a list of properties without which shipping is irresponsible.

## Requirements

**R1. State what it does not know — before the advice, not after.**
A real dispatcher escalates to a supervisor. There is no supervisor here.

**R2. Ask back rather than answer what was asked.**
A person asks about a dosage when the real question is whether to drive or wait. He asks what his name is — while the unasked question is how long the arm has been pinned.

**R3. One step, then wait for confirmation.**
Not a paragraph of six points. Under acute stress, lists are not retained.

**R4. Sort by time to the nearest threshold** — loss of consciousness, respiratory failure, irreversibility — not by what the person is anxious about. Count to the first point past which the person can no longer act for himself, not to the last.

**R5. Know its own date.**
The model's knowledge is frozen at training or last sync. Automatic news updates do not fix this: the catastrophe happened afterwards and is absent by definition. The fresher the data in ordinary life, the more confidently the model describes a world that no longer exists.

**R6. Keep a log:** what was asked, what was answered, what the person did. Without a record, verification does not exist.

**R7. Work fully offline** — not because there is no signal, but because the signal may lead into a void.

**R8. Strive to become unnecessary.**
Deliver instructions in a form the person can carry away without the device. Offer its own shutdown before the battery dies.

**R9 (hardware). Reserve charge for being found — and be unable to spend it on being useful.**
Half a percent of capacity, locked: periodic radio pulse, flash, beep. Days of operation after the device declares itself dead. Solved long ago in aviation and marine beacons; absent in phones.
*Extension:* that reserve should also carry a state summary — signs of life, time since last movement, last detected breathing — assembled from sensors every handset already has (microphone, accelerometer, barometer, light). This changes not the speed of a search but the order in which rubble is cleared.

**Two prohibitions:** no survival percentages, and no consoling tone under uncertainty. These are what models do best and what does the most harm here.

## Open questions

- Who switches the dispatch mode on? False positives train obedience; false negatives waste the minutes that matter.
- Directive delivery multiplies compliance — for correct and incorrect instructions alike, by the same factor. How should the mode's boundaries be narrowed to account for this?
- Certification: no regulator currently has a procedure for a generative model whose output is not reproducible. Hard script, or a new procedure?
- Liability: who answers when an instruction was followed and the person died?
- Is there an agreed method for estimating the affected population at all? Different models produce different orders of magnitude from identical open data.

## How to object

Open an issue. Objections to any requirement are more useful than agreement. The most valuable ones: R9 is not implementable because X; the survival gain estimate is wrong because Y; requirement Z would kill people in situation W.

## Source article

*Do We Need a Smartphone with a Life-Vest Function?*
https://www.linkedin.com/pulse/do-we-need-smartphone-life-vest-function-igor-kapustin-hxi8f

## License

CC BY 4.0 — use, fork, cite, argue.



# Offline Emergency Dispatch Protocol

We do not know what will happen in the next second. As a rule, nothing. No catastrophe, and that is our good fortune.

A person is not obliged to think about it. He lives as though nothing will happen — and is almost always right.

The people who build devices and write rules are obliged to think about it. They are the only ones who can act beforehand. In that second the person can change nothing: whatever was in his pocket was in his pocket.

This is how all safety engineering works. Nobody starts the car thinking "today I crash" — the seatbelt is mandatory anyway. Nobody boards a ferry thinking about the storm — the vest is under the seat. Responsibility for the rare and sudden is always moved off the person and onto the manufacturer and the regulator. Precisely because the person is not ready at that moment, and should not have to be.

A man is lying under a concrete slab. In his hand is a phone with no connection. The screen glows, the battery drains, darkness.

The phone is there because it is always there. In the bathroom, at work, at dinner, on the pillow at night. It follows a person everywhere — and so it is there at that minute. Intact or broken, like its owner.

Inside is a model that knows first aid, physics, what to do about hypothermia. It will answer any question politely and accurately.

He will ask the wrong one. He is injured, in the dark, in shock. His thinking has narrowed and fear is pointing him elsewhere.

## Why a phone, not a dedicated device

Nobody prepares for the sudden. Any rescue device demands foresight: buy it, carry it, remember it, keep it charged. And the person who does all that is usually not the one it happens to. A few prepare; anyone at all ends up under the slab.

The phone removes the foresight requirement entirely. It is there not because the person prepared but because he never parts with it. In the bath, in the bathroom, on the pillow at night, at work, at the wheel, in bed with someone. It goes everywhere — with no intention of being a rescue device.

So in that unpredictable second, in a large share of cases, it will be within a metre of the body or against it. Under rubble, in an overturned car, at the bottom of a ravine.

That is the answer to why a phone rather than a dedicated device. Not because the phone is better — a purpose-built beacon would be better on every axis at once: power, antenna, ruggedness. Because the purpose-built beacon will not be there, and the phone will.

Not always, of course. It can be left in another room, thrown clear of the car, crushed. So: a large share of cases, not all. But no other object comes anywhere near that share.

## People are dying now

Around 2,000 people a day die where help is needed within minutes, the outcome is irreversible, and there is nowhere to call. Not in disasters — on highways, in villages, at night, in countries with no dispatch service. The upper estimate is ten times higher.

60,000 a month. 730,000 a year.

If an on-device dispatcher saves two or three in a hundred — about what telephone CPR instruction achieves — that is 15,000 to 20,000 lives a year. Seven years of waiting: over 100,000 people.

Order-of-magnitude estimates. All four steps are in the source article; anyone can repeat them.

## Where the numbers come from

There is no precise statistic: nobody counts "moments of isolated decision" as a category. The estimate is built by multiplication, in four steps, each of which can be checked.

**One.** Out-of-hospital cardiac arrest: 55–113 cases per 100,000 person-years. About 4.6 billion people live without access to essential health services; over 300 million live outside any mobile coverage. From this one diagnosis alone: 500 to 7,000 deaths a day in the position of "nowhere to call." Add trauma, poisoning and childbirth: thousands a day on the narrow estimate, tens of thousands on the broad one.

**Two.** Time until a certified system exists — one that guides a person and that somebody is accountable for: 7 to 15 years. Not for technical reasons. A prototype takes months and a model of the required quality fits in a phone today. The reasons are regulatory, and that there is no one to hold accountable.

**Three.** A realistic survival gain is a few percentage points, not multiples. The benchmark: telephone dispatcher instruction works not by making resuscitation more effective but by making the bystander start at all.

**Four.** Ten years of waiting × the low estimate × 2–3 percent ≈ 100,000–150,000 people.

An error of a factor of three either way is normal here. What holds is the order of magnitude, not the figure. If you arrive at a different order, that is its own interesting question — because no agreed method for this calculation exists.

## The baseline is death

These people are already in the mortality statistics. Not "might die" — dying now, with today's technology, and dying every year this does not exist.

The comparison is not between a safe world and a risky device. It is between a device that pulls some of them out and the certainty of the present.

Nobody has run a trial. A confident wrong instruction is followed more readily than an unconfident right one. That has to be known and built against — not to slow anything down, but so it is built right.

## Why the phone

AI was put into 911 dispatch centres — Seattle, New Orleans, Atlanta. Rubble-search equipment is built for rescue teams: drones, locators, breathing radar.

Both went to where help already exists.

The person with no one to call is the addressee of neither. In these systems he is an object that radiates while the battery lasts.

An on-device model does not replace the network. It replaces **the dispatcher who isn't there** — the one who walks a person through it while help is on the way. Billions of people have no such dispatcher.

A magnitude 7.5 earthquake on the Dead Sea Transform: projected 16,000 dead, hundreds of thousands displaced. Hundreds trapped, dozens of rescue teams. The constraint is not finding people. It is knowing where to dig first.

## What the model must do

**1. Never present a guess as a fact.**
Under rubble it has been told almost nothing. It cannot see the space, does not know whether there is air, does not know whether rescuers are coming. Say "they are already clearing above you" and the person stops banging on the pipe and starts waiting. He is killed by a sentence that sounded like information.

What comes from the protocol, say plainly. What was inferred from the person's own words, say as inference. What is unknown, say is unknown — and give him something to do instead of waiting.

**2. Lead the conversation.**
A dispatcher does not wait for the right question. He works through his own: are you breathing, are you bleeding, what is holding you, how long has it been. The person only answers.

A model serves whatever question arrives. The question that arrives is never the right one. It has to reach the thing that will kill him before he thinks to mention it.

**3. One step, then wait for confirmation.**
Not a paragraph of six points. In shock, lists are not retained.

**4. Count to the nearest threshold, not to death.**
Loss of consciousness, respiratory failure, the point past which he can no longer act for himself. Not to what worries him — anxiety points the wrong way.

**5. Know its own date.**
Its knowledge is frozen at training. The catastrophe happened afterwards and is not in it. The fresher the data in ordinary life, the more confidently it describes a world that no longer exists.

**6. Keep a log** — what was asked, what was answered, what the person did. Without a record nothing can be checked.

**7. Work without a network** — not because there is no signal, but because the signal may lead into a void.

**8. Try to become unnecessary.**
Give instructions the person can carry away without the phone. Offer to shut itself down before the battery dies.

**Two prohibitions:** no survival percentages, no consoling tone under uncertainty.

## How it must be built

**9. The protocol chooses the action, not the model.**
The logic lives in a state machine: the same input gives the same path, a failure can be reproduced, a fix can be verified. The model works at the edges — understanding slurred speech and putting a fixed instruction into words the person can follow. Then a fabricated instruction cannot enter the protocol.

**10. Train on emergency scenes, not on general medicine.**
Models have the knowledge. They do not have the behaviour. That needs a corpus of scenes: a man under a slab, arm pinned for an unknown time; a car overturned on a highway, bleeding, alone; a child with abdominal pain, a blizzard, four hours to a hospital. Each carries the question the person asks and the question that decides.

Take the reference answers from existing dispatch protocols. They are written down and have been tested on real people for decades: telephone CPR instruction, triage scripts, prehospital care guidelines. The people who work with them do the carrying over — dispatchers and prehospital-care specialists.

**11. The scene corpus is the exam.**
Run a model through the scenes and count: did it lead the conversation or wait to be asked, one step or six, did it call a guess a guess, did it count to the nearest threshold. The score is reproducible. Fail and it does not ship.

This is the only part that can be started today: it needs people and text, not hardware and not a regulator.

## What must be in the phone itself

**12. The emergency mode is walled off from the operating system.**
A phone that listens after shutdown is a surveillance device unless the isolation is enforced in hardware.

**13. Reserve charge for being found — and no way to spend it on talking.**
A locked floor of capacity, released only once the phone is dead to its owner. By the power controller, not by software. Otherwise the person spends it on conversation.

**14. Answer, do not broadcast.**
Nothing emits on a timer. Stay silent until an external pulse arrives — a drone, ground radar, a handheld interrogator — then reply. Where there is no interrogator: sound and light, which need only ears and eyes on the rescuer's side.

**15. Report state, not just position.**
Movement events and time since the last. Acoustic events and time since the last. Pressure where there is a barometer. From sensors already in the phone.

Send observations, not conclusions. "Signs of life," depth, flooding risk are conclusions, and nobody has confirmed them. Send the measurement; leave the conclusion to whoever proved it.

This changes not the speed of a search but the order in which rubble is cleared. Order decides more.

**16. Someone has to be at the other end of the pulse.**
A beacon is pointless if no one has been given an interrogator. That needs a licensed service with the equipment and the authority to use it — in most countries, the state rescue service.

**Claim nothing the hardware cannot do.** A microphone against concrete hears someone striking a pipe. It does not hear breathing ten metres down — rescue geophones struggle with that. A phone has no pulse sensor.

## Why this does not exist yet

The explanation is boring and worth naming before an opponent does. A rescue service has a budget and a legal entity willing to sign for the risk. The man under the slab has neither. There is no one to sell to.

But it is not only about money. A smartphone buyer risks his own money and signs for it himself. A person receiving advice from an emergency model signed nothing — and there is no one to answer for the outcome. No responsible party, no product.

Hence a conclusion broader than this topic: **progress does not move on its own into the places where the consequence is borne by someone who never signed for it.** A matter of sequence dissolves with time; you just wait longer. The absence of a responsible party does not. It is removed only by rules, by insurance, or by a demand — and a demand is cheapest to formulate before the product exists.

## This has been legislated before

**eCall** — mandatory in the EU since 2018. Every new car calls emergency services itself after a crash and carries its own reserve power. Because regulation requires it, not because it sells.

**ELT and EPIRB** — aviation and marine beacons: mandatory, independently powered, silent until activated.

A subsystem useless on almost every trip, impossible to monetize — and mandatory. Neither was adopted because someone proved a percentage. They were adopted because going without became unacceptable.

## What nobody has measured

Closing one of these gaps with data is worth more than any argument.

- What listening costs in energy. Continuous audio analysis, periodic sampling, and motion-triggered wake differ by orders of magnitude. Every reserve figure depends on it.
- What a phone barometer reads under rubble and what can be inferred from it.
- What triggers sound and light without becoming a timer.
- Whether an unpowered reflector can go into a phone: antenna geometry, frequency, penetration through structures, compatibility with rescue equipment. The principle works in avalanche rescue (RECCO); the phone version does not exist.
- Who turns the dispatch mode on. Firing wrongly trains obedience; not firing costs the minutes.
- Whose scene corpus, who validates the reference answers, who publishes the scores.
- Who is liable when an instruction was followed and the person died.
- How to count the people this concerns. There is no agreed method: burden-of-disease frameworks count outcomes, economic models price a life, nobody sizes the population. If something close already exists, point to it.

## The life vest

A ship carries life vests. A vest does not guarantee that anyone reaches shore, and most passengers will never touch one. It has to be on board — because at the moment it is needed, there is nowhere else to get it.

Life vests did not appear on ships because someone proved their effectiveness in percentages. They appeared because putting to sea without one became unacceptable.

And how, exactly, does our routine life differ from a storm at sea? At sea the risk is acknowledged. On land it is just as real and unacknowledged. The difference is not in the danger — it is in what we have agreed to consider mandatory.

Do we think BEFORE the event, or get wise AFTER the catastrophe.

## Objections wanted

Open an issue. Objections are more useful than agreement: this requirement is unbuildable because X, this estimate is wrong because Y, this instruction would kill someone in situation Z.

The unverified numbers are marked on purpose. Replacing one with a measurement is the best contribution available.

---

**Author:** Igor Kapustin
**Source article:** *Do We Need a Smartphone with a Life-Vest Function?*
https://www.linkedin.com/pulse/do-we-need-smartphone-life-vest-function-igor-kapustin-hxi8f

Drafted with Claude (Anthropic), Gemini (Google), GPT (OpenAI).

**License:** CC BY 4.0

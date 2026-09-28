# Theory Notes - Superyacht HVAC Concept Sizing

*A plain-language walkthrough of the engineering logic behind this notebook, written so a reviewer with a general engineering background - not necessarily an HVAC specialist - can follow the reasoning end to end.*

---

## 1. The one idea everything else builds on

Sizing an HVAC system is really just answering one question, carefully, several times over:

> **"What is the worst moment this space will ever see, and how much capacity do I need to cover it - safely, even if something breaks?"**

Every section of this notebook is a different piece of that same question: how much heat gets into a room, how much of that is temperature vs. humidity, how much fresh air has to be conditioned separately, and how much spare capacity to keep in reserve so that one failed unit doesn't mean a hot, sticky cabin.

---

## 2. Why "peak load" isn't just adding up the heat sources

It's tempting to think the hottest moment in a room is simply *"whenever the sun is strongest."* But that's not how heat actually moves through a building - or a hull.

Walls, furniture, and structure behave the same way - some store heat and release it hours later, others respond almost immediately. The **CLTD/CLF method** (Cooling Load Temperature Difference / Cooling Load Factor) is essentially a lookup-table way of encoding this time-lag behavior, so that the "peak load" used to size equipment reflects *when the space is actually hottest*, not just when the sun happens to be strongest. That's why this notebook reports a specific **peak cooling hour** rather than just a single load number - the hour matters as much as the value.

---

## 3. Humidity is a second, hidden load

Air conditioning does two jobs at once:

1. **Sensible cooling** - lowering the air's temperature (what a thermometer measures).
2. **Latent cooling** - removing moisture from the air (what makes air feel "sticky" even at a mild temperature).

This is why the notebook keeps the **fresh-air AHU** (which has to fight hot, humid outdoor air - the harder job) separate from the **local fan-coil units** serving each space (which mostly recirculate already-dry indoor air - the easier job). Lumping the two together would both oversize the FCUs and undersize the part of the system actually struggling with humidity.

---

## 4. From room load to hardware: FCUs and chilled water

Once a room's peak load (in kW) is known, that number has to become something a piece of equipment can actually deliver. A **fan-coil unit (FCU)** does this by blowing room air across a coil of cold water - the colder the water and the more of it flows through, the more heat the coil can absorb.

The chilled water's flow rate is calculated so that the temperature rise of the water (its "return minus supply," here 12 °C − 6 °C = 6 K) times its flow rate matches the required cooling duty - the same logic as figuring out how fast you need to refill a bucket with a small leak to keep the water level steady.

---

## 5. Sizing the plant: chillers and N+1 redundancy

The **chiller plant** is where all those individual room and AHU loads get added up and turned into a decision: how many chillers, and how big each one should be.

The critical design choice here is **N+1 redundancy**. Sizing three chillers to *exactly* split the peak load evenly is fragile - if just one unit trips offline (for maintenance, a fault, anything), the remaining two can't cover the full load, and the whole vessel starts overheating at the worst possible time.

The **COP** used here (electrical input vs. cooling output) is the same yardstick used to judge a heat pump's performance elsewhere - a higher COP means the same cooling output for less electrical power, which matters directly for a vessel's total energy budget.

---

## 6. Concept duct sizing

Sizing the main fresh-air duct is a straightforward trade-off: a bigger duct means lower air velocity (quieter, less fan energy, but takes up more space); a smaller duct is more compact but noisier and costs more fan power to push air through.

**Analogy:** it's like choosing the diameter of a garden hose - a wide hose lets water flow gently and quietly, a narrow hose forces the same flow through faster and louder, with more resistance. The **equal-friction method** used here picks a duct size so that the pressure drop per meter of ducting stays consistent throughout the system - a practical starting point before anyone worries about actual routing, bends, and fittings.

---

## 7. Reusing the same model for winter

The same room geometry that drove the summer cooling calculation is reused for the winter heating check - only now the question flips: instead of "how much heat needs to be removed," it's "how much heat is escaping through the walls and through air leaks, and how much has to be added back to stay warm?" No credit is taken for solar gain or internal heat sources in winter, because a design should hold up on the coldest, darkest expected day - not an average one.

---

## 8. Checking a supplier's numbers - the habit that ties it all together

The final and, arguably, most important piece of this notebook isn't a calculation at all - it's a comparison. A supplier's proposal is only useful once it's checked against an independent, first-principles requirement.

**Analogy:** a vendor's datasheet is a claim, not a fact - the same way you wouldn't buy a car based only on the brochure's fuel-economy number without checking it against your own driving. This notebook's final check simply asks, for each key requirement (fresh-air flow, AHU cooling duty, N+1 chiller capacity): *does the proposed equipment actually meet or exceed what the independent calculation says is needed?* That one habit - verify before you trust the paperwork - is the difference between reviewing a proposal and just rubber-stamping it.

---

## 9. How the pieces connect

| Question | Section in notebook | Concept covered |
|---|---|---|
| When is the room actually hottest, and by how much? | CLTD/CLF cooling load | Thermal mass / time-lag, peak hour |
| How much of that load is humidity, not just temperature? | Psychrometrics / AHU sizing | Sensible vs. latent load, condensate |
| How does a room's load become a piece of hardware? | FCU sizing | Chilled-water flow, ΔT |
| How many chillers, and how big, to survive a failure? | Chiller plant + N+1 | Redundancy, COP |
| How big should the main duct be? | DuctSizer | Equal-friction sizing |
| Does the same model work in winter? | HeatingLoad | Steady-state heat loss, no solar credit |
| Is the vendor's quote actually enough? | Supplier-proposal check | Independent verification |

Together, these cover the conventional, everyday backbone of ship HVAC design - the foundation that innovation work (like waste-heat recovery) has to sit on top of, not replace.

---

*These notes are a simplified teaching summary, not a substitute for the notebook itself, which contains the actual equations, assumptions, and `hvacpy` calls used to produce every number referenced here.*

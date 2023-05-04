# Carbon Footprint

Related: [[counterfactuals]]

#bib #green


Most common definition: the total amount of CO2 emissions that is directly or indirectly, but exclusively caused by an activity or a total life of a product. Often, other gases are translated into "equivalent amount of CO2".

# Wiedmann2008

Wiedmann, T., & Minx, J. (2008). A definition of ‘carbon footprint’. Ecological economics research trends, 1, 1-11.
http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.467.6821&rep=rep1&type=pdf

Often no clear definition:
* Direct, or full-impact?
    * Upstream production
    * Downstream effects, including eventual disposal
    * _Not mentioned in the intro, but wouldn't redistribution of consumption / activity be here as well?_
* Only CO2, or all greenhouse gases (methane, NO2), weighted by their impact?
* Only from fossil fuels, or including emissions from soil etc?

The first group is a question of defining a **boundary**, critical to avoid both undercounting and double-counting. This boundary is also extremely important if we want to talk about **carbon offsets**, as this very idea presumes drawing a boundary around several activities, some causing positive, and some - negative footprint.

> Does it it mean that carbon offset is currently impossible in principle (until we start doing sequestration), as any planted forest would eventually decompose and return CO2 in the atmosphere? Unless you can somehow guarantee that this land will never be deforested again, but can you?

Two approaches to calculation:
1. Bottom-up: **Process Analysis**. You id every point that generates emissions, and allocate it to activities. Huge problem with boundaries (the word "exclusively" in the definition), hard to aggregate over abstract entities (states, industries). Ref: Tukker and Jansen 2006
2. Top-down: **Environmental Input-Output**. Typically works at a meso-level (industry, sector, country). By defining a very large boundary, it basically ensures the calculation of the total impact is correct, but it's harder to analyze.
3. Hybrid methodologies that try to unite the two (for example, works by Heijungs and Suh 2002, 2006).

Some concrete case studies:
* **UK Schools Carbon Footprint Scoping Study (GAP et al. 2006)**. 1.3% of total UK emossions; 26% on-site (heating), 22% electricity, 20% transport, and the rest - all the good that are consumed in the process.
* **Counting Consumption report (SEI et al. 2006)** for UK as a whole. A third of all emissions - from electricity and heating.

Argue against the inclusion of other gases in the "carbon footprint" measure (unless it is renamed into "climate footprint" or something), as it dilutes the definition.


# Cucek2012

Čuček, L., Klemeš, J. J., & Kravanja, Z. (2012). A review of footprint analysis tools for monitoring impacts on sustainability. Journal of Cleaner Production, 34, 9-20.
https://www.sciencedirect.com/science/article/pii/S0959652612001126?casa_token=Cno48t_cFuYAAAAA:YW6JWPnAB5An-dmWwUE20yo8zxtes4EcjIj1Dtz2UHczi0N9mWGZ-9YENVUDYer1XH0da6kLuQ

**Life Cycle Assessment (LCA)** - cradle to grave analysis. To stress that grave of a product isn't the end of the story (we need to utilize waste), sometimes people call it "Cradle-to-Cradle" instead.  Origins of Carbon Footprint: Høgevold, 2003. Now standardized:  ISO 14040, ISO 14044. This is important to avoid "problem-shifting" from one process / product / region to another (fudging boundaries).

Other footprints: Water, Energy, Emissions, Nitrogen, Land, Biodiversity

Optimizing footprints is a **multi-objective problem**, and it's hard.

A list of tools (as of 2012):
*  Process Network Synthesis (PNS) (Friedler et al., 1995) 
*  P-graph approach (Friedler et al., 1992; Sűle et al., 2011). Uses advanced branch-and-bound (ABB) algorithm (Friedler, 2010).
*  Linear programming

# Planetly

https://www.planetly.org/en/how-do-you-analyze-the-carbon-footprint-of-your-company/

Standard way of calculating CF:
1. Scope 1: Direction Emissions - all the fuel the company burns directly
2. Scope 2: Electricity and heating
3. Scope 3: Everything else: all products & services purchased (upstream), and everything brought to life by companies activity, or the product produced (downstream)

https://www.planetly.org/en/how-to-offset-your-carbon-emissions/

Offsetting may also include:
* Investment in renewable energy (to shift the structure of energy production)
* Community projects (like, sponsoring projects that would reduce impact of communities)
* Waster-to-Energy projects

Note that most cost-efficient ways to offset actually include investment in developing countries. But that also potentially makes these communities vulnerable (balance of power), so careful consideration of side-effects is critical.

https://www.planetly.org/en/how-to-reduce-carbon-emissions-in-your-company/

30% of emissions in Germany - heating for buildings. Data centers - 2% worldwide. Meat production - 14% worldwide (they recommend only catering vegan options, and only reimbursing vegetarian options during travel).

https://www.planetly.org/en/ghg-protocol-and-iso-standards/

**CDP (Carbon Disclosure Project)**. More and more companies report it, similar to how financial reporting is done. There is a standard now, with those 3 scop	es described above. ISO 14064 is another attempt to standardize the process (but the scope breakdown is simpler: just "direct" and "indirect").


#  Maple Leaf Bacon

https://civileats.com/2020/01/28/can-an-industrial-scale-meat-company-be-carbon-neutral/

A Canadian meat company said they're gonna be carbon-neutral. But they included only scope 1 and 2, as they are a distributor, and the overwhelming share of emissions is generated at feed production and animal raising stages (including manure emissions, as that's not about carbon footprint strictly speaking, but about climate footprint, which involves other gases).


#  Meat

Less meat is almost always better than sustainable meat

https://ourworldindata.org/less-meat-or-sustainable-meat

Cheese and pork are about 3-4 times better than beef. Chicken and eggs are 6-7 times better. But beans are again 6 times better than chicken.

Nuts are technically carbon-negative (as they grow on trees). _But are they sustainable? I don't think one can feed the entire world with nuts, can one?_

So we have two contradictory conclusions here:
* No meat can even remotely compete with plant-based food
* Most meat (75% of production) produces only 30% of emissions, but the remaining 25% of meat (beef) produces 70% of emissions. That's insane.

But importantly it only applies to so-called **industrially intensive** beef (US-style). Some local ways of raising cattle may be quite sustainable. So local situations may be complicated again. But at the scale of the world, yep, no beef please.
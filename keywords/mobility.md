# Mobility

See also: [[gis]], [[carsharing]]

#geo #urbanism #gdpr #bib


# Library

Alessandretti, L., & Szell, M. (2022). Urban Mobility. _arXiv preprint arXiv:2211.00355_.
https://arxiv.org/pdf/2211.00355.pdf
A book chapter (like a review, but better!) Analysing individual route trajectories. Brief overview of data sources. Statistical properties (with formulas): entropy, exploration, heterogeneity, periodicity. Distributions of displacements, waiting times, gyration radius. Modeling: random walk, EPR model (Exploration and Preferential Return), PEPR model (Preferential Exploration and Preferential Return), Container model (hierarchy of levels). Then several sections on improving urban mobility networks, and a section on tools.

Choi, S. J., Jiao, J., Lee, H. K., & Farahi, A. (2023). Combatting the mismatch: Modeling bike-sharing rental and return machine learning classification forecast in Seoul, South Korea. _Journal of Transport Geography_, _109_, 103587.
 https://www.sciencedirect.com/science/article/abs/pii/S0966692323000595
They looked at the balance of outgoing and incoming trips, classified zones into "sources" and "sinks", and then used various variables to build a predictive model (aka explain) what makes them sources and sinks. Basically, when bikes are distributed all over, people come from where they are (high population density become sources, or what they call "rentals hot spots"), but don't necessarily return there. People also rent more bikes in areas with nature (they use some SIDI index for that...) and in hot areas. On top of that, one fun variable that promotes dead-ends is called SHEI , or Shannon’s evenness index (not sure what it is).

Vehicle-to-grid: https://en.wikipedia.org/wiki/Vehicle-to-grid
The idea of using electric cars as a giant pool of batteries that are vaguely available to balance the load on the electric network, or maybe even feed back into the network.

# Data sharing standards

## MDS (Mobility Data Specification)

Developed in 2018 in LA to track scooters. (This is important because the standard assumes that not just vehicles ids and exact GPS locations are shared, but also the routes taken by the vehicles). Of important conceptual ideas, vehicles are assigned states (available, reserved, trip, inactive etc.) Providers are supposed create an API, agencies can read from it, and get full info about every trip, all the time. Which means that it can in principle de-anonymized, and is potentially unsafe (a well known weakest aspect of the approach). The standard is maintained by the Open Mobility Foundation. 

Links:
* https://www.transitwiki.org/TransitWiki/index.php/Mobility_Data_Specification
* specifications: https://github.com/openmobilityfoundation/mobility-data-specification
    * among other things, a schema for data exchange: https://github.com/openmobilityfoundation/mobility-data-specification/blob/main/provider/README.md#trips 

## CDS-M

"City Data Specification - Mobility": an attempt to create a European standard for sharing mobility data with municipalities and provinces that is safe (secure), yet informative. Developed by 5 municipalities in the NL (Amsterdam, Utrecht, Groningen, Eindhoven, Rotterdam) since 2019; now they invite other EU locations in. Mostly seems to be developed so that cities had some insight into how their public planning projects are working: for example:

* How to allocate and reallocate parking spots
* Where to build the charging infrastructure
* Which legislative actions cities can take to discourage private cars (encourage shared mobility)
* Which actions cities can take to achieve a desired split of transportation modes

The idea seems to be that there's a standard, and then there are use cases. All companies are required to buy into the standard (?), but actual data exchange is governed via individual use cases, where a process of getting a data also includes privacy (DPIA = Data Protection Impact Assessment), a security assessment, legal agreements between the parties that exchange the data, e.g. pre-fabricated NDAs etc.. But CDS-M offers "use case templates", which means that one does not have to negotiate the terms and conditions, and then develop the reports, from scratch every time. For every city, and every provider, it will be the same solution and the same format (or rather, a set of possible solutions).

A "central authority" of sorts will be in charge of developing these usecase templates and share them via the usecase store. There are 34 templates there already. But at the same time, the text repeatedly insists that the provider needs to be involved early on, that only the provider can help to draft the KPI, to interpret the numbers etc.

GDPR concerns: as the concerns coming from user-level (or even trip-level) data are basically the same for different use cases, we can "solve them" in a generfal form. DPIA is a process they developed for this purposes.

Links:
* https://www.cds-m.com/ - main presentation
* https://github.com/CDSM-WG/CDS-M - technical specifications for the data exchange standards
* Whitepaper: https://www.polisnetwork.eu/wp-content/uploads/2021/03/CDS-M_Blueprint_v0.0.1.docx2_.pdf
* Main page for Amsterdam: https://www.amsterdam.nl/innovatie/mobiliteit/city-data-standard-mobility/
* https://dashboarddeelmobiliteit.nl/ - A dashboard showing all shared bikes in the NL
* https://usecasestore.cds-m.com/ - The usecase store

# Markets

Second tier cities with mobility potential in Europe (a consensus of various studies): Brussels, Barcelona, Oslo, Warsaw, Bordeaux, Prague, Rotterdam

# Notes

Fleet war (like a price war but with high fleet): if customers develop a habit, and are more likely to take the car they drove the last time around, then higher-fleet provider will naturally have an advantage, and their share will grow, while a smaller competitor will disappear. The only way to prevent it is for the smaller competitor to reduce prices, but assuming similar fixed expenses it will mean that the smaller provider will have even less money to reinvest. Does it mean that the dynamics favors monopolization? Is there a standard classification of business into those that are prone vs not succeptible to monopolization?

The most popular car in Germany is VW Golf. The cost of owning it is ~300€/mo for the car itself + some 50€/mo for insurance, so ~350€/mo.  On the other hand, a typical rental in cities is probably some 30-60 minutes long (very roughly), which with typical prices on the market, gives us about ~10€ per trip, roughly.

If we are going to compare car ownership to car sharing, then fuel cost should be excluded from these 10€, as a person owning a car woud pay them as well. VW Golf does 8L/100 km, fuel costs ~1.8 €/L, Berlin (a very large city by EU standards) is about 30 km across, so fuel would cost about 4€ per trip. Which means that fuel excluded, a person would pay about 6€/trip for the shared car as a service. So a profitability line between owning and sharing a car lies at about 350/6 = 58 trips a month, or about 2 trips a day.

Now, according to my rough estimations, a dedicated carsharing location becomes profitable once it achieves about 10 trips/day (if trips are a poisson process, aka trips of opportunity, and not daily commutes) Which means that for occasional commuters (aka, in places with reliable public transportation), a group of about 5-10 households switching to shared cars would be a win-win (profitable) for both the people, and the shared cars provider!

If people want to commute for work, the calculation is a bit different, and the rates would go up (as we have to assume that in the worst case the car is doing only 2 trips a day). A daily cost of a rented car, for the shared mobility provider, would have to be justified by its daily use, and then the rate would include a variable part as well. The daily cost of a rented car for the provider can be guessed from leasing rates (about 20€ per day), plus we have the variable part (mostly fuel, which we estimated above as about 5€, multiplied by 2 trips), which gives us about 30€/day. As the default price of a rental was estimated as 10€ per trip (above), to get 30€ from 2 trips, the company would have to put a drop-off fee of €10 at this location. Which is a lot of course, but still, with 30€ per working day, it would be more profitable for a household not to own a car, but to share it, if they commute less than 12 times a month. Which roughly corresponds to working hybrid, with 3 days in the office (assuming of course that these days are different for different people, or the concept of sharing won't work). This cenario also increases the size of a minimally profitable locaitonto about 10 / 12 * 30 = 30 households. Which means that in the long-term, with good public transportation and/or hybrid work, any group of houses that has some 30 households within a walking distance from a shared transportation point, would benefit from switching to car-sharing, and this operation would be profitable for the provider.

Interestingly, it means that post-pandemic switch to hybrid work, as well as companies switch to shared offices, both synergize with car-sharing, as they reduce the number of costly car-trapping use cases (reduce daily commute), and also encourage people to stagger this commute across the week! It is a good news for cities and the environment.

# References

Fluctuo Mobility reviews for EU markets:
* 2021: https://mcusercontent.com/baa57cfb15e41471e5dd992db/files/61c9c628-2b97-0e0b-9943-40c70e00c886/Fluctuo_European_Index_Annual_Review_2021.pdf
* Q1 2022: https://9202560.fs1.hubspotusercontent-na1.net/hubfs/9202560/Fluctuo%20-%20Q1%202022%20European%20Shared%20Mobility%20Index.pdf
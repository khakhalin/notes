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

# References

Fluctuo Mobility reviews for EU markets:
* 2021: https://mcusercontent.com/baa57cfb15e41471e5dd992db/files/61c9c628-2b97-0e0b-9943-40c70e00c886/Fluctuo_European_Index_Annual_Review_2021.pdf
* Q1 2022: https://9202560.fs1.hubspotusercontent-na1.net/hubfs/9202560/Fluctuo%20-%20Q1%202022%20European%20Shared%20Mobility%20Index.pdf
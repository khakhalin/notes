# Mobility

See also: [[gis]], [[maps]]

#geo #urbanism #gdpr #bib


Subtopics:
* [[carsharing]] - papers specifically about carsharing go there

# Queue

How people actually move
https://bsky.app/profile/louisboucherie.com/post/3lxrbdnjhu22j
🔥 to compare to my assumptions in the model

Santi, P., Resta, G., Szell, M., Sobolevsky, S., Strogatz, S. H., & Ratti, C. (2014). Quantifying the benefits of vehicle pooling with shareability networks. Proceedings of the National Academy of Sciences, 111(37), 13290-13294. 🔥🔥🔥
Quantifying the benefits of vehicle pooling with shareability networks
https://www.pnas.org/doi/full/10.1073/pnas.1403657111

Alonso-Mora, J., Samaranayake, S., Wallar, A., Frazzoli, E., & Rus, D. (2017). On-demand high-capacity ride-sharing via dynamic trip-vehicle assignment. Proceedings of the National Academy of Sciences, 114(3), 462-467. https://www.pnas.org/doi/pdf/10.1073/pnas.1611675114

Vazifeh, M. M., Santi, P., Resta, G., Strogatz, S. H., & Ratti, C. (2018). Addressing the minimum fleet problem in on-demand urban mobility. Nature, 557(7706), 534-538.
https://senseable.mit.edu/papers/pdf/20180524_Vazifeh_AddressingMinimum_Nature.pdf
What minimal fleet do you need to serve a given demand. 🔥 

Shaheen, S., & Cohen, A. (2019). Shared ride services in North America: definitions, impacts, and the future of pooling. Transport reviews, 39(4), 427-442.
https://escholarship.org/content/qt2wr9q8c2/qt2wr9q8c2_noSplash_8ecf5a9cf8b06ae43cf99befdfabd144.pdf

http://michael.szell.net/downloads/talk_szell2021mcb.pdf

Kwon, O. H., Hong, I., Jung, W. S., & Jo, H. H. (2023). Multiple gravity laws for human mobility within cities. arXiv preprint arXiv:2305.15665.
https://arxiv.org/pdf/2305.15665.pdf
Among other things, analyses and comparisons of several cities (in the US)

Mobility as a feature (academic publication): https://www.tandfonline.com/doi/full/10.1080/01441647.2022.2159122

Alessandretti, L., & Szell, M. (2022). Urban Mobility. arXiv preprint arXiv:2211.00355. (Actually a book chapter)
https://arxiv.org/pdf/2211.00355.pdf

https://en.wikipedia.org/wiki/Location_theory
https://en.wikipedia.org/wiki/Central_place_theory

Optimization Model of Taxi Fleet Size Based on GPS Tracking Data
https://www.mdpi.com/2071-1050/11/3/731

Shaw, M. (2018). Twenty years of car sharing: a case study on the city of Vancouver’s role in the growth of car sharing in Vancouver.
http://summit.sfu.ca/system/files/iritems1/18613/etd19861.pdf

https://www.oliverwymanforum.com/content/dam/oliver-wyman/ow-forum/mobility/2022/Value%20Pool%20Report.pdf

An Explanatory Model Approach for the Spatial Distribution of Free-Floating Carsharing Bookings: A Case-Study of German Cities
* https://pdfs.semanticscholar.org/343e/d69c2ad96ef30e0fb3e8e03110598f540d5d.pdf?_ga=2.157482803.881533839.1633099311-97609317.1633099311
* Process all links to this article, add to the list ([link](https://scholar.google.com/scholar?cites=127356670159689516&as_sdt=2005&sciodt=0,5&hl=en))

Ke, J., Yang, H., Li, X., Wang, H., & Ye, J. (2020). Pricing and equilibrium in on-demand ride-pooling markets. Transportation Research Part B: Methodological, 139, 411-431.
https://www.sciencedirect.com/science/article/abs/pii/S0191261520303611

Orozco, L. G. N., Alessandretti, L., Saberi, M., Szell, M., & Battiston, F. (2021). Multimodal urban mobility and multilayer transport networks. arXiv preprint arXiv:2111.02152.
https://journals.sagepub.com/doi/10.1177/23998083221108190
https://arxiv.org/pdf/2111.02152
(lit review on the topic)

Sprei, F., Habibi, S., Englund, C., Pettersson, S., Voronov, A., & Wedlin, J. (2019). Free-floating car-sharing electrification and mode displacement: Travel time and usage patterns from 12 cities in Europe and the United States. Transportation Research Part D: Transport and Environment, 71, 127-140.
https://research.chalmers.se/publication/510282/file/510282_Fulltext.pdf

Münzel, K., Arentshorst, M., Boon, W., & Frenken, K. (2022). Assessing policies to scale up carsharing. In Innovations in Transport (pp. 242-268). Edward Elgar Publishing.
https://dspace.library.uu.nl/bitstream/handle/1874/425265/9781800373372_book_part_9781800373372_18.pdf?sequence=1

Vossebeld, J. A. (2022). Explaining the variation in the demand for roundtrip B2C carsharing in neighbourhoods in the G44 cities in the Netherlands: an explanation based on neighbourhood characteristics and the current distribution of shared cars (Master's thesis, University of Twente).
http://essay.utwente.nl/89531/1/Vossebeld_MA_EEMCS%20_openbaar.pdf

# Library (papers)

Schläpfer, Markus, Lei Dong, Kevin O’Keeffe, Paolo Santi, Michael Szell, Hadrien Salat, Samuel Anklesaria, Mohammad Vazifeh, Carlo Ratti, and Geoffrey B. West. "The universal visitation law of human mobility." Nature 593, no. 7860 (2021): 522-527.
https://www.nature.com/articles/s41586-021-03480-9
https://par.nsf.gov/servlets/purl/10309392
Analyzed lots of mobility data from different cities (Boston, Lisbon, Singapore etc.; 7 in total; mobile tracking data) and found that "the number of visitors to any location decreases as the inverse square of the product of their visiting frequency and travel distance". That's a weird way to describe it, but essentially they say: the interesting thing is that, if you first group visitors by cohort, in terms of how often them visit a location, then in some sense the normalized visitor count per unit of area $ρ=C/(rf)^2$. Here C is a constant for each location (attractiveness of a location), r is radius, and f is the "visitation frequency" (the share of days that this particular cohort of visitors visited this location). One interesting consequence is that the "average distance per visitor, as they travel to the location" doesn't depend on location attractiveness C, if you look across locations. (If I got it right, they claim that locations tend to affect close-by and far-way visitors similarly: if it's fancy, then it's fancy, everyone visit, average distance traveled per customer doesn't change). But also, in practical terms, what they seem to be trying to say, is that concentrating on N(r) is futile, as N(r) scales differently depending on how frequently the location is visited by individual people (integral over f). The further the location - the more rare are the trips. If however you renormalize to rf, everything scales nicely. The problem is that it's so painful to use. When they model their data however, they use probabilistic trip distances that scale as $P(d) = d^{-0.5}$ (in the paper they use polar coordinates and report $r^{-1.5}$, but the delta area scales linearly, so in Cartesian coordinates the power should be −0.5). They claim, it leads to the emergence of spatial clusters. When analyzing mobility date, used either a 1000 or 500 m grid.

Song, C., Koren, T., Wang, P., & Barabási, A. L. (2010). Modelling the scaling properties of human mobility. Nature physics, 6(10), 818-823.
https://arxiv.org/pdf/1010.0436
Mention [[levy_walks]] and "continuous time random walks" (CTRW) as ways to model human mobility. Refer to device-tracking data, and similar to Schläpfer2021 (but before them) arrive at $p(d) \sim d^{-0.5}$. Contrast human mobility an diffusion: humans return back, so don't really diffuse anywhere.

Choi, S. J., Jiao, J., Lee, H. K., & Farahi, A. (2023). Combatting the mismatch: Modeling bike-sharing rental and return machine learning classification forecast in Seoul, South Korea. _Journal of Transport Geography_, _109_, 103587. 
 https://www.sciencedirect.com/science/article/abs/pii/S0966692323000595
They looked at the balance of outgoing and incoming trips, classified zones into "sources" and "sinks", and then used various variables to build a predictive model (aka explain) what makes them sources and sinks. Basically, when bikes are distributed all over, people come from where they are (high population density become sources, or what they call "rentals hot spots"), but don't necessarily return there 💎💎💎. People also rent more bikes in areas with nature (they use some SIDI index for that...) and in hot areas. On top of that, one fun variable that promotes dead-ends is called SHEI , or Shannon’s evenness index (not sure what it is).

Fielbaum, A., Bai, X., & Alonso-Mora, J. (2021). On-demand ridesharing with optimized pick-up and drop-off walking locations. Transportation research part C: emerging technologies, 126, 103061.
https://www.sciencedirect.com/science/article/pii/S0968090X21000887
In case of taxis, inroducing a fixed network of "starting points" and requiring people to walk towards it for just one minute drastically improves the level of service (fulfillment rate), boosts vehicle utilization. Very notably, a win-win solution for both parties involved: users, and businesses. 💎💎💎 _Mention: meaning that a business can afford to redirect these savings into whatever investments that matter in each particular case: cleanness, coverage etc._

Alessandretti, L., & Szell, M. (2022). Urban Mobility. _arXiv preprint arXiv:2211.00355_.
https://arxiv.org/pdf/2211.00355.pdf
A book chapter (like a review, but better!) Analysing individual route trajectories. Brief overview of data sources. Statistical properties (with formulas): entropy, exploration, heterogeneity, periodicity. Distributions of displacements, waiting times, gyration radius. Modeling: random walk, EPR model (Exploration and Preferential Return), PEPR model (Preferential Exploration and Preferential Return), Container model (hierarchy of levels). Then several sections on improving urban mobility networks, and a section on tools.

Vehicle-to-grid: https://en.wikipedia.org/wiki/Vehicle-to-grid
The idea of using electric cars as a giant pool of batteries that are vaguely available to balance the load on the electric network, or maybe even feed back into the network.

Mishina, M., Mityagin, S., Belyi, A., Khrulkov, A., & Sobolevsky, S. (2024). Towards Urban Accessibility: Modeling Trip Distribution to Assess the Provision of Social Facilities. Smart Cities, 7(5), 2741-2762.
https://www.mdpi.com/2624-6511/7/5/106
Use DL to predict trip probability on a graph (not on a grid). St. Pete as the source of data. Compare their model with two other approaches: Linear Allocation Model and Doubly Constrained Gravity Model. The paper is mostly concerned by peple vs. kindergartens; catchment areas, availability, etc. Not mobility. The language is rather heavy and hard to read.

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

If people want to commute for work, the calculation is a bit different, and the rates would go up (as we have to assume that in the worst case the car is doing only 2 trips a day). A daily cost of a rented car, for the shared mobility provider, would have to be justified by its daily use, and then the rate would include a variable part as well. The daily cost of a rented car for the provider can be guessed from leasing rates (about 20€ per day), plus we have the variable part (mostly fuel, which we estimated above as about 5€, multiplied by 2 trips), which gives us about 30€/day. As the default price of a rental was estimated as 10€ per trip (above), to get 30€ from 2 trips, the company would have to put a drop-off fee of €10 at this location. Which is a lot of course, but still, with 30€ per working day, it would be more profitable for a household not to own a car, but to share it, if they commute less than 12 times a month. Which roughly corresponds to working hybrid, with 3 days in the office (assuming of course that these days are different for different people, or the concept of sharing won't work). This scenario also increases the size of a minimally profitable locaitonto about 10 / 12 * 30 = 30 households. Which means that in the long-term, with good public transportation and/or hybrid work, any group of houses that has some 30 households within a walking distance from a shared transportation point, would benefit from switching to car-sharing, and this operation would be profitable for the provider.

Interestingly, it means that post-pandemic switch to hybrid work, as well as companies switch to shared offices, both synergize with car-sharing, as they reduce the number of costly car-trapping use cases (reduce daily commute), and also encourage people to stagger this commute across the week! It is a good news for cities and the environment.

# References

Fluctuo Mobility reviews for EU markets:
* 2021: https://mcusercontent.com/baa57cfb15e41471e5dd992db/files/61c9c628-2b97-0e0b-9943-40c70e00c886/Fluctuo_European_Index_Annual_Review_2021.pdf
* Q1 2022: https://9202560.fs1.hubspotusercontent-na1.net/hubfs/9202560/Fluctuo%20-%20Q1%202022%20European%20Shared%20Mobility%20Index.pdf
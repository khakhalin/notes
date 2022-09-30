# Mobility

See also: [[gis]]

#geo #urbanism #gdpr #bib


# Data sharing standards

## MDS (Mobility Data Specification)

Developed in 2018 in LA to track scooters. Vehicles are assigned states (vailablek reserved, trip, inactive etc.) Providers create an API, agencies can read from it, and typically get full info about every trip all the time. Which means that it can in principle de-anonymized, and is potentially unsafe. The standard is maintained by the Open Mobility Foundation. 

Links:
* https://www.transitwiki.org/TransitWiki/index.php/Mobility_Data_Specification
* specifications: https://github.com/openmobilityfoundation/mobility-data-specification

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

Interesting second line cities: Brussels, Barcelona, Oslo, Warsaw, Bordeaux, Prague. (Also Rotterdam)

# References

Fluctuo Mobility reviews for EU markets:
* 2021: https://mcusercontent.com/baa57cfb15e41471e5dd992db/files/61c9c628-2b97-0e0b-9943-40c70e00c886/Fluctuo_European_Index_Annual_Review_2021.pdf
* Q1 2022: https://9202560.fs1.hubspotusercontent-na1.net/hubfs/9202560/Fluctuo%20-%20Q1%202022%20European%20Shared%20Mobility%20Index.pdf
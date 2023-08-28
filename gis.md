# GIS

See also: [[mobility]]

#geo #gis #bib


Subtopics - tools:
* [[shapely]] - including geopandas
* [[openstreetmap]] - APIs etc
* [[crs]] - Coordinate Reference Systems
* [[maps]] - just a collection of random maps

Subtopics - broadly speaking:
* [[mobility]] - moving in the city
* [[carsharing]] - carsharing specifically
* [[mobile_geoloc]] - how to locate phones from signals
* [[urbanism]] - urban planning

# Tools

If you have a geo json and you need to visualize it, save it, then upload here:
https://geojson.io/

Kepler: some sort of an js-based pretty map visualizer; great for quickly creating fancy 3D maps
https://kepler.gl/

# Courses and intros

* Geospatial Data Science: https://github.com/mszell/geospatialdatascience
* Look through these videos (about geopandas mostly): https://www.youtube.com/c/OpenGeoHubFoundation

# Refs

Boeing, G. (2022). Street network models and indicators for every urban area in the world. Geographical Analysis, 54(3), 519-535.
https://arxiv.org/pdf/2009.09106.pdf
Mostly very simple analysis of maps (not network / graph, really), such as N intersections per person, and road length per person. If you consider all locations (lots and lots of automatically connected points), they correlate in different ways, and this short paper tries to make a story about it.

# Resources and data

Municipal borders by country, several formats
https://gadm.org/download_country.html

Population density (European Terrestrial Reference System 1989 Lambert Azimuthal Equal Area (ETRS989-LAEA, EPSG:3035) projection, 1 km grid)
https://ec.europa.eu/eurostat/web/gisco/geodata/reference-data/population-distribution-demography/geostat

# Queue

http://michael.szell.net/downloads/talk_szell2021mcb.pdf

Schläpfer, M., Dong, L., O’Keeffe, K., Santi, P., Szell, M., Salat, H., ... & West, G. B. (2021). The universal visitation law of human mobility. Nature, 593(7860), 522-527.
https://www.nature.com/articles/s41586-021-03480-9

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
* [link](https://pdfs.semanticscholar.org/343e/d69c2ad96ef30e0fb3e8e03110598f540d5d.pdf?_ga=2.157482803.881533839.1633099311-97609317.1633099311)
* Process all links to this article, add to the list ([link](https://scholar.google.com/scholar?cites=127356670159689516&as_sdt=2005&sciodt=0,5&hl=en))

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

Mahajan, V., Kuehnel, N., Intzevidou, A., Cantelmo, G., Moeckel, R., & Antoniou, C. (2022). Data to the people: a review of public and proprietary data for transport models. _Transport reviews_, _42_(4), 415-440.
https://www.tandfonline.com/doi/pdf/10.1080/01441647.2021.1977414

Look through this wiki, identify most interesting topics: https://www.transitwiki.org/TransitWiki/index.php/Table_of_Contents

Hexagonal universal grid from Uber:
* https://eng.uber.com/h3/
* https://h3geo.org/
* See also: Google's open-source grid (aka "S2 square grid")

## Mobility Pricing

A two-stage incentive mechanism for rebalancing free-floating bike sharing systems: Considering user preference
https://www.sciencedirect.com/science/article/abs/pii/S1369847821001789

The effects of pricing and service configurations on a ride-pooling service with pick-up and drop-off points
https://repository.tudelft.nl/islandora/object/uuid:3e9426a7-a3ec-4943-af7c-55a26592beaa

Viglia, G., Maras, M., Schumann, J., & Navarro-Martinez, D. (2019). Paying before or paying after? Timing and uncertainty in pay-what-you-want pricing. Journal of Service Research, 22(3), 272-284. ([link](https://pure.port.ac.uk/ws/portalfiles/portal/13177493/VIGLIA_2019_cright_JSR_Paying_before_or_paying_after_Timing_and_uncertainty_in_pay_what_you_want_pricing.pdf))

## Fancy network stuff

Yap, W., Chang, J. H., & Biljecki, F. (2022). Incorporating networks in semantic understanding of streetscapes: Contextualising active mobility decisions. Environment and Planning B: Urban Analytics and City Science, 23998083221138832.
[link](https://journals.sagepub.com/doi/pdf/10.1177/23998083221138832?casa_token=8MGV-jXu3L8AAAAA:cGdqX1ZgZZRROlXnzjeeNWnyiAZA3LzlgDlmZlN0gphEK5Qp40bPBEbwGZJmI493Htdi7bhcu5Ee)

Emergence of complex network topologies from flow-weighted optimization of network efficiency
Sebastiano Bontorin, Giulia Cencetti, Riccardo Gallotti, Bruno Lepri, Manlio De Domenico
https://arxiv.org/abs/2301.08661
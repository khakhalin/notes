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

Mahajan, V., Kuehnel, N., Intzevidou, A., Cantelmo, G., Moeckel, R., & Antoniou, C. (2022). Data to the people: a review of public and proprietary data for transport models. _Transport reviews_, _42_(4), 415-440.
https://www.tandfonline.com/doi/pdf/10.1080/01441647.2021.1977414

Interesting narrow thematic wiki:
https://www.transitwiki.org/TransitWiki/index.php/Table_of_Contents

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
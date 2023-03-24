# Geolocalization from mobile data

Parent: [[09_Graphs]], [[gis]]

#graph #gis


# Geolocalization

How to figure out how people more from their mobile data?

Limitations and data specifics:
* No access to GPS, signal strength, signal direction.
* Each transceiver (node) has a variable coverage (service zone), as it depends on the total load, activity of neighboring zones, etc. So the gelocation-node binding is not necessarily fixed.
* The distribution of events per user is extremely wide (a person walking through a city generates orders of magnitude more low-level events than a mobile sitting on table in the countryside).
* Data size: ~10^10 events daily, ~10^7 users, ~10^5 service nodes.

Very heavy **data obfuscation** and depersonalization in Germany (Bundesdataschutzgesetz). Most practical problems in mobile analytics these days is not cause by the raw data, but by this obfuscation: both in terms of what data comes in, but also in terms of what data is reportable by the analyst.
* User ids are renewed every day
* If data feel too identifiable, they are noisified (say, pings are shifted in time)
* If a node had too few users within an hour, this entire data is removed

Interesting **questions**:
* Typical "wells" where users spend more time on their way (park, home, etc.)
* Classifying user types from their behavior
* Detecting anomalies in network functioning (transport accidents, service interruptions etc.)
* Describing the service zone of each node, and how it changes

**Data flow:** Low-level events → daily stats (because ids are reset daily) → cumulative stats → filtering out non-plausibles → Atomic Dwell Detection (dwell = one "place" that has a coherent behavior to it, and where users tend to stay) → further analysis

It's better to live on graphs rather than in xy space, as the landscape is very fragmented, especially in urban areas (subway, reflections from buildings etc).

**Timing** is important. If the same user goes back and forth between 2 nodes with ~1s interval, probably they don't move, but rather the service zones overlap. Industry slang: "Air Adjacency" - there exist some geographic area from which both nodes are reachable. Like an **edge** on a graph. From there, we can go to **cliques** - groups of nodes that form a fully connected subgraph.

Technically this graph is oriented, as transitions between nodes A→B may be very different from B→A, especially in urban areas (subway tunnels, car tunnels). Moreover, people may be forced from one node to another just because their use of the network changes	(say, a person makes a call, and is downgraded to a simpler protocol on a different node, just because it has more capacity).

# Refs

Geolocalization app
https://www.cellmapper.net/apps
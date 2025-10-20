# Petri nets

Parents: [[graphs]]
See also: scheduling, [[job_shop]]

#graph


Petri Nets are not strictly speaking graphs, but an extension of a graph: it's a sort of a process (token propagation) on a bipartite graph, made of **places** and **transitions**.

Draw and simulate PNs:
* https://www.fernuni-hagen.de/ilovepetrinets/fapra/wise23/rot/index.html
* https://pes.vsb.cz/petrineteditor/#/model

# Papers with applications

Best introductory paper for PN in practical scheduling. 100+ citations (but overall this is a rather niche topic, it seems)
van der Aalst, W. M. (1996). Petri net based scheduling. Operations-Research-Spektrum, 18(4), 219-229.
https://www.vdaalst.com/publications/p33.pdf
Introduces timed PN, then a [[job_shop]] scheduling problem, with an illustration that already looks overwhelming.

Introductory book:
Wolfgang, R. (2013). Understanding petri nets-modeling techniques. Analysis Methods, Case Studies. Springer.
https://pagesperso.ls2n.fr/~attiogbe-c/mespages/MSFORMEL/IUT/13-02-20_UnderstandingPNet_final_Reisig.pdf

Sachweh, T., Haritz, P., & Liebig, T. (2024, June). Using Petri Nets as an Integrated Constraint Mechanism for Reinforcement Learning Tasks. In 2024 IEEE Intelligent Vehicles Symposium (IV) (pp. 1686-1692). IEEE.
https://www.semanticscholar.org/reader/b875efee4970312c9580ad62c404cf4a3110a0b9

Lassoued, S., & Schwung, A. (2024). Introducing PetriRL: An innovative framework for JSSP resolution integrating Petri nets and event-based reinforcement learning. Journal of Manufacturing Systems, 74, 690-702.
https://arxiv.org/html/2402.00046v1
Speficialy for [[job_shop]] optimization. Reasonable review. Their network is a classic JSS, so parallel and uniform in architecture, and it feels that it is what made using this formalism possible without that much cognitive overload.

Modeling job shop scheduling with batches and setup times by timed Petri nets. 2009.
https://www.sciencedirect.com/science/article/pii/S0895717708000952

# Refs

Colored Petri Nets - in which tokens also carry variables:
* https://dl.acm.org/doi/pdf/10.1145/2663340

University courses
* https://www2.informatik.uni-hamburg.de/TGI/PetriNets/introductions/
* https://www.fernuni-hagen.de/ilovepetrinets/

Python Packages:
* https://github.com/bpogroup/simpn
* https://cpntools.org/
* https://projects.laas.fr/tina/manuals/formats.html - this one is really about analysis, not use
* pnet - a description: https://arxiv.org/pdf/2302.12054
* SimPN: https://pure.tue.nl/ws/portalfiles/portal/344323785/paper-12.pdf

Languages to describe Petri Nets (neither seem to be particularly good):
* https://pnml.lip6.fr/
* https://www.pnml.org/
* https://en.wikipedia.org/wiki/Petri_Net_Markup_Language

Hammedi, S., Elmelliani, J., Nabli, L., Namoun, A., Alanazi, M. H., Aljohani, N., ... & Alshmrany, S. (2024). Optimizing Production in Reconfigurable Manufacturing Systems with Artificial Intelligence and Petri Nets. International Journal of Advanced Computer Science & Applications, 15(10).
https://thesai.org/Downloads/Volume15No10/Paper_44-Optimizing_Production_in_Reconfigurable_Manufacturing_Systems.pdf
A very short review, no math, no helpful suggestions.
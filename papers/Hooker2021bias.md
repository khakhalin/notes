# Beyond bias as a "data problem"

Hooker, S. (2021). Moving beyond “algorithmic bias is a data problem”. Patterns, 2(4), 100241.
https://www.cell.com/patterns/fulltext/S2666-3899%2821%2900061-1

Parent: [[fairness]]

#fairness #ethics


Does the model only reflect existing biases (potentially coming from the dataset)? Or does algorithm design contribute to the problem?

First of all, to fix bias through the data you know what you are looking for, have the data, and have the labels (for whatever attributes that you want to protect). "Taxonomy of race and gender" is a mess, so good labels are often impossible. And people often hate providing them (if you try to crowd-source them), as it feels intrusive.

And trade-offs are inevitable. For example, degradation of less represented classes during pruning.

Also a uniquely human effect that comes with data emphasis: "diffusion of responsibility".

# Refs

An alternative take on these topics (short, and careful, but knowing the background of these discussions, critical) by Yannic Kilcher:  https://www.youtube.com/watch?v=J7CrtblmMnU 
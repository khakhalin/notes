# HMAX

Parents: [[vision]]
See also: [[convnet]]

Old (1990) model of primate vision.

Explicitly define primate-vision-like layers:
1. S1: Simple cells - fit the image with 2nd derivatives of 2D Gaussians, output in -1..1 (modeling different phases during an oscillation). Act as edge detectors.
2. C1: Complex cells (in the V1 sense); soft-max over local S1 units
3. S2: Composite cells - output in 0..1 (modeling rate code)
4. C2: Complex Composite cells
5. View-tuned cells - at this point, global pooling, to achieve position-invariance
6. Object-tuned cells - dense layer
7. Task-related cells - dense layer, softmax classification output

Weights of middle layers are learned from labeled examples.

# Refs

https://maxlab.neuro.georgetown.edu/hmax.html - actual lab that developed it

Original publication:
Riesenhuber, M. & Poggio, T. (1999). Hierarchical Models of Object Recognition in Cortex. Nature Neuroscience 2: 1019-1025.

Technical details:
Serre, T., & Riesenhuber, M. (2004). Realistic Modeling of Simple and Complex Cell Tuning in the HMAX Model, and Implications for Invariant Object Recognition in Cortex. CBCL Paper 239/AI Memo 2004&x2012;004, Massachusetts Institute of Technology, Cambridge, MA, July 2004.
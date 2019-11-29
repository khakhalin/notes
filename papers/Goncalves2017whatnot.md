# “What not” detectors help the brain see in depth

Goncalves, N. R., & Welchman, A. E. (2017). “What not” detectors help the brain see in depth. Current Biology, 27(10), 1403-1412.
https://www.sciencedirect.com/science/article/pii/S0960982217304049

#neuro #vision

How do we see in 3D? Intuitively one could think: find same details (corresponding to the same object), then triangulate. But if you think deeper, doesn't work, as the space for searching is too sparce, and generally, if 3D relies on _differences_ between L and R eyes, why would it start with finding similarities? 

Also we know from studies in V1 that RFs for L and R eyes are actually very different, making any sort of "matching" problematic. Here they use ANNs and a principle of optimal coding to show that actually RFs should be complementary, as then neurons can look for differences right away.

Their ANN: 2 layers:
* 2 images from 2 eyes as an input
* bank of trainable linear filters ("simple units"): essentially, a convolutional layer, but looking at matching segments of images from both eyes at the same time
* ReLu
* 2 units in the output layer: "Far" and "Near". 
Supervised learning of all weights with true Far/Near labels. 

Achieved 99%+ accuracy. The first layer developed something like Gabor filters, but not matching between L and R images. Instead, phase-shifted (in extreme case - reversed, corresponding to a pi/2 phase shift). 

Tested with noise stimuli, confirmed that the network as a whole responds to translational shift of the pattern.

This result actually explained "a long-standing puzzle from the psychophysical literature ref 22, 23 that demonstrated better judgments for stimuli comprising dark and bright dots (mixed polarity) compared to only dark or only bright dots (single polarity) /""

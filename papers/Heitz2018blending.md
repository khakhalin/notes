# Heitz 2018 histogram-preserving blending

Heitz, E., & Neyret, F. (2018). High-performance by-example noise using a histogram-preserving blending operator. Proceedings of the ACM on Computer Graphics and Interactive Techniques, 1(2), 1-25.

#graphics #generative

The goal is to generate pattern from a small sample. The problem is preserving histograms during blending, to avoid blotchiness. Usually people do it with local correction, but it still leaves artifacts. It's not a problem for Gaussian signal though, as you can just ajust the mean and the sd (amplitude) directly (after calculating them explicitly). So they transform their empirical histogram into normal distribution, adjust, then transform back. And it works surprisingly well

For tiling, they use a triangular grid, with overlapping hexagonal zones, each sampled from a random part of the input square; blended from one to another linearly (I'm assuming, but not sure?), with blending performed in this roundabout "Gaussianize, then de-gaussianize" way.

Do comparison with other alternative techniques, show that their approach works well; lots of examples, 2d power spectra. Their solution does look better visually.

Finally, show that their approach works well for applying textures to 3D objects via "triplanar blending": that's when instead of wrapping, you just project the noise from 3 directions, and blend between them. They are the kings of blending!
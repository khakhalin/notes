# Activation Functions for DL

#dl #bib

Parent: [[06_DL]]
See also: [[regularization]], [[hyperparameters]]

See also (special types of units):
* [[lstm]] - For RNs, Long-Short-Term-Memory
* [[gru]] - Gated Recurrent Unit; Similar to lstm, but with fewer parameters

# ReLU

# LeakyReLU

# Tanh

# Fancy

Sitzmann, V., Martel, J. N., Bergman, A. W., Lindell, D. B., & Wetzstein, G. (2020). Implicit Neural Representations with Periodic Activation Functions. arXiv preprint arXiv:2006.09661.
https://arxiv.org/abs/2006.09661
Video abstract:
https://www.youtube.com/watch?v=Q2fLWGBeaiI
Claim that sinusoidal activation is much better than anything else, at least in domains that resonate well with Fourier transform: images, audio, 3d modeling, and fitting wave function solutions. Vaguely connect their idea to **grid cells** (coz periodicity of RF).
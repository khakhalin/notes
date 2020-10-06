# Tone transfer and DDSP (Differential Digital Sound Processing)

Engel, J., Hantrakul, L., Gu, C., & Roberts, A. (2020). Ddsp: Differentiable digital signal processing. arXiv preprint arXiv:2001.04643.
https://arxiv.org/pdf/2001.04643.pdf

#sound

* Blog: https://magenta.tensorflow.org/ddsp
* Demo: https://sites.research.google/tonetransfer

Models trained for ~3 hours (not sure on what), using ~10 min recording. Monophonic, single instrument. Models get to control additive synths (of a standard DSP kind). Encode audio as the fundamental + latent space, then decode it back into sound.

Synths they used:
* Harmonic Additive Synthesizer (adds overtones)
* Subtractive Noise Synthesizer (filters white noise with time-varied filters)

Plus also reverberation on top (_without parameters? Fixed? Not clear_)

**Loss** = comparison of spectrograms. Some custom loss that apparently works well as a perceptual loss on histograms; first go from sound to spectral domain, getting vectors of amplitudes for different frequencies. Then calcualte L1-difference for each point in time $\sum_i |s_i-\hat s_i|$, and sum over time. But then also do the same for log(s_i), and add to this same loss. _They don't explain why they added this log, and don't cite anything._

All elements of this pipeline are differentiable, and so one can train the model directly. No GAN, nothing; just basic autoencoder.

> This part I didn't quite understand: like, how did they make this subtractive noise synthesizer differentiable? But I haven't read carefully. Also they have a collab with the code.

Is lightweight enough for new source audios to be processed on-device (with a pretrained model of course).

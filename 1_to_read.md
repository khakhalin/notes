# Articles and links to read

Yet another hopeful list =)

https://arxiv.org/abs/1911.04252 
Self-training with Noisy Student improves ImageNet classification Qizhe Xie, Eduard Hovy, Minh-Thang Luong, Quoc V. Le
Something weird semi-supervised learning with noisy teachers and distillation. Essentially, it seems that a badly labeled large dataset is better than a well-labeled small dataset, so it's better to train one model on a small dataset, then have it label a huge dataset (even tho many labels will be wrong), and then use this large dataset to train the next model. Or something like that. Weird.

https://www.frontiersin.org/articles/10.3389/fnsys.2019.00042/full 
The Dialectics of Free Energy Minimization Evert A. Boonstra and Heleen A. Slagter
Seems to be a review explaining the free energy minimization principle for an organism. It's not math though; looks almost like philosophy?

https://www.biorxiv.org/content/10.1101/838383v1
Training deep neural density estimators to identify mechanistic models of neural dynamics
Gonçalves .. Macke 2019
About how to use deep learning to guess neuronal parameters to fit the actual activity of the network (?) They seem to be looking at actual V(t) though.

https://www.biorxiv.org/content/10.1101/837567v1 
Interrogating theoretical models of neural computation with deep inference
Bittner .. Cunningham 2019
Apparently a very similar paper (to Goncalves 2019), but independent.

https://arxiv.org/abs/1905.01164 
SinGAN: Learning a Generative Model from a Single Natural Image Tamar Rott Shaham, Tali Dekel, Tomer Michaeli

https://www.biorxiv.org/content/10.1101/798553v1 
Recurrent neural network models of multi-area computation underlying decision-making Michael Kleinman, Chandramouli Chandrasekaran, Jonathan C Kao
It seems that they try recurrent networks (RNN) running in parallel, and compare their decision times to that from monkey cortex. Then show that one RNN doesn't match the distribution of times, but if you have several running in parallel, and then synthesizing info, you get similar results? Claim that different modes of distributed computation are experimentally testable like that.
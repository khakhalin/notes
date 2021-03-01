# Deep supervised, but not unsupervised, models explain representation in the visual cortex

Khaligh-Razavi, S. M., & Kriegeskorte, N. (2014). Deep supervised, but not unsupervised, models may explain IT cortical representation. PLoS computational biology, 10(11), e1003915. https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4222664/

#neuro #vision

Related: [[vision]], [[convnet]]

**Inferior Temporal Cortex** (IT) for object recognition. fMRI recordings from humans, unit recordings (cells) from monkeys. Clustering of responses to objects predicts how similar these objects seem to a human. **Dissimilarity matrices**as a tool. Animate vs inanimate division for example is very strong. There's a debate about whether semantics influences IT responses and representation, or whether they are purely visual. DL can be used as a model: if a vision-only DL model can behave similarly 

While we have activation data from both human brains (fMRI), monkeys (unit recordings) and DL models (obviously), building correspondences (especially with a goal of saying if they are "alike" in some sense or not) is hard. So they look only at the output: **dissimilarity**. Test a large variety of models (37 in total), including explicitly neuro-inspired models at one extreme, and unsuperveised models at the other. Most models aren't exactly those you hear at ML conferences or self-driving car descriptions though, all seem very academic and Neuro-obsessed (an almost full list, for keyword purposes: Radon, Silhoette, BT, Gist, GB, dSIFT, PHOW, PHOG, ssim, gssim, LBP, HMAX, SLF, GMAX, combi27)

All unsupervised models failed to cluster human and animal faces together, while in the brain they are felt as similar. Deep supervised models did ok tho.

> That's interesting, as faces and general agency-detection is supposedly hardwired in humans, which means that even we're looking at a "trainable" higher-level region, it would have indeed had access to internally generated labeled data.	 And lots of it, thus avoiding the typical "biology vs ML" contrast, in terms of data hungriness (see [[Zador2019pure]]).

Then they did some trick with "remixing and reweighing features" to "highlight divisions" between different categories. Not sure how exactly, but in the 2nd half of experiments they seem to be even more focused on animal and human parts and faces as opposed to inanimate objects. At least based on the pics, it seems to be really about just balancing sensitive, contrasted clusters, to have best 50/50 sensitivity for some particular interesting divisions.

Main visual: lots of noisy weakly blocked dissimilarity matrices that look kinda like silicon chips under a scope. And we're supposed to visually compare them. Hmm.

However creating an "**ensemble** classifier" in which opinions of unsupervised models is added to that of supervised models actually improved resemblance with brain data. So some unsupervised learning is still sitting there in the brain, and DL doesn't fully explain it?

Better models (in terms of label prediction accuracy) are also more brain-like.

> Do we have a philosophical loop here? Labels are produced by humans, which necessarily makes "better" models more human like, just because "better" is already defined as "human-like"? Some alien for whom large and small hammers feel like two distinctly different objets would have provided them with different labels...
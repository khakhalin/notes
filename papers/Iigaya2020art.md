# Preference for art emerges from features

Aesthetic preference for art emerges from a weighted integration over hierarchically structured visual features in the brain (2020)
Kiyohito Iigaya, Sanghyun Yi, Iman A. Wahle, Koranis Tanwisuth, John P O'Doherty
https://www.biorxiv.org/content/10.1101/2020.02.09.940353v1

#neuro #art #vision #deepneuro

Really fun idea overall, but some strange details in math and reporting that make it hard to relate to this paper. In a way, one of the key findings is not that aesthetic preference can be explained, but that most people are really simplistic, but also ideosyncratic in their judgement of art, so not much can be predicted actually.

Can preference be predicted from an image? Human people rated images (1001 different paintings) on an 1-4 scale; ~1360 participants online (each rating random 60 images), and 7 in-lab (rated every single image). Used real art, including abstract, cubist, landscapes, flowers and other stillife, and Röthke-like. Gave only 6 seconds to rate a picture.

> I almost feel that first 6 s are not even about aesthetic value, but more about the first perception. Which is not only simpler, but also may be only weakly correlated with aesthetic value you develop even after just 10-20 s of starting at the image. And then even this thing was so highly idiosyncratic (see below). Which is not surprising if it was a first impression, as first impressions are probably closer to test than to objective property of a piece… In short, great idea, but the limitations, and the execution, makes it really hard to interpret it meaningfully.

Computationally extracted a bunch of pre-conceived image qualities, such as hue, brightness, bluriness, edginess (40 in total, from Li 2009) + local features on 2 largest segments after image segmentation, including segment symmetry, color entropy, color skew and variance, segment position, and more. Then went down from a total of 83 features to 9 "best" qualities using a heavily normalized ridge regression with lasso metrix? This process is paring features down is described a bit differently in different parts of the paper, which is rather confusing.

On top of these 9 features they also added four high-level features: "abstract/concrete", "dynamic/stille", "hot/cold", and "positive/negative" (emotionally), that they collected from 13 more participants. 

Then from 13 features (9 low-level + 4 high-level, but it's not clear which of those 83 made it to 9, and why), they used linear regression to predict aesthetic value for each participant (they call it **linear feature summation model**). Or a general model across people. 10-fold cross validation; all features with a lasso penalty (say it was deep, but don't give the value of λ).

Their reporting of results is rather sketchy: they don't, for example, report their average prediction quality within-individual. But it seems that while for some pariticipants the accuracy (Pearson r) was close to 80%, for some it was about 20%; with an average of about 50%. A "general model" performs at about 40% level. They don't explain it well, but based on images, it seems that most variability is not between individuals, but within individuals (as different models largerly perform either equally bad or equally good within each individual).

At the same time, they claim that most people (~80%) strongly prefer concrete stuff, ~15% like abstract art and care about positive colors, and about 5% are all artsy (cubist).

While the prediction quality is quite low, it is still much better than chance. Correspondingly, they show that they can use the same model to predict how well people would like photos (with r of about 20%). To do so, they estimated values of 4 high-level features from low-level features (accuracy or about 0.75, except for "dynamics" that proved more conceptual, and harder to predict).

Then they took a convolutional ANN pre-trained on ImageNet, and trained last 3 layers to predict ratings (only for in-lab participants). Got about 20-30% correlations. From layers, could predict their features, ~80% or so for low-level, and about 50% to 80% for high-level. They did it for every layer, and show that high-level features representation increases through the network (that had depth of 15), while low-level representation decreases. Which is quite obvious, honestly.

But then they also did fMRI of 6 individuals, and show that medial prefrontal cortex is prepresents the "subjective value of art". And if you go through other cortical regions, then similarly their ability to guess various low-level features from fMRI signal lowers if they go downstream through different cortical areas, while for high-level features it increases. This part is fun! 

The analysis is rather weird though: they first only looked at voxels that had a significant F-test for at least one feature. And then across all these voxels within each cortical region, they calculated the % of voxels that correlated with a high-level feature, as opposed to a low-level feature. Hmm.

# References

C. Li and T. Chen, “Aesthetic visual quality assessment of paintings,” IEEE Journal of selected topics in Signal Processing, vol. 3, no. 2, pp. 236–252, 2009.

# Relevant work

Vessel, E. A., Maurer, N., Denker, A. H., & Starr, G. G. (2018). Stronger shared taste for natural aesthetic domains than for artifacts of human culture. Cognition, 179, 121-131. _Taste for art is highly variable across individuals_
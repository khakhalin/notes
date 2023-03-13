# Self-supervized photo upsampling

Menon, S., Damian, A., Hu, S., Ravi, N., & Rudin, C. (2020). PULSE: Self-Supervised Photo Upsampling via Latent Space Exploration of Generative Models. In Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (pp. 2437-2445).
https://arxiv.org/abs/2003.03808

#image #self-supervised #bias

Github:
https://github.com/adamian98/pulse
Tweetprint:
https://twitter.com/tg_bomze/status/1274098682284163072

The paper took some photos, downsampled them, then trained the "upsampler". But it became really famous as it "upsampled" Obama into a white dude :) The original tweet: https://twitter.com/Chicken3gg/status/1274314622447820801

As it turns out, they trained on the StyleGAN original dataset, and it (I think?) used some "FlickFaceHQ" dataset for faces, so their data was heavily biased towards younger white dudes.

The image will now obviously be the best texbook case of bias in AI. Unfortuantely, many people's response was to blame the data (that it was biased), which is of course true, but I think the fact that the bias exists, and how hard it is to remove it, is more important than the particular reasons this biased appeared. In a sense that we don't want people to "unbias" their applications by carefully balancing (cherry-picking) their datasets; we want more robust, stable solutions.

This paper also caused a lot of controversy on twitter, as some prominent AI researchers started to argue increasingly ad-hominem about whether the problem is with the algorithm, or with the data (a biased, but detailed youtube summary here, 14 min: https://www.youtube.com/watch?v=n1SXlK5rhR8 ).
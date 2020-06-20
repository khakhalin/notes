# Perceptual losses for real-time style transfer and super-resolution

Johnson, J., Alahi, A., & Fei-Fei, L. (2016, October). Perceptual losses for real-time style transfer and super-resolution. In European conference on computer vision (pp. 694-711). Springer, Cham.
https://arxiv.org/pdf/1603.08155.pdf

#image #loss

Parents: [[bib_generate]] / [[perceptual_loss]]

The **loss network** is pre-trained as an image classifier (16 layers, convolutional, on ImageNet). Just calculate the Σ(ϕ1-ϕ2)^2 where ϕ are feature representations **in early layers** (values in the latent space) learned by the classifier. They say: "image content and overall spatial structure are preserved, but color, texture, and exact shape are not".

> I'm not sure how it works. Don't image classifiers mostly rely on patterns, texture, and materials? Or does it only happen at deep layers, while here they seem to be taking activation of early layers? Is it what the difference is? But also, why would early layers represent image content better than the texture; wouldn't it be the other way around?

Perceptual loss can be further paired with **Style reconstruction loss**: ||G1-G2|| where G is a Gram matrix: G_ij ~ ⟨ϕ_i , ϕ_j⟩, and ϕ_i are features in the latent space. Here G shows which features are activated together across different parts of the image. It so happens that minimizing this loss creates images of similar style (they claim so, and show some images, that are kinda OK, but don't really look SOTA to a modern eye)
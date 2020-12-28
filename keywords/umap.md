# UMAP

#clustering #dim #features

Parents: [[04_Features]]
Related: [[curse_dim]], [[tsne]]

# Refs

* https://umap-learn.readthedocs.io/en/latest/how_umap_works.html
* [https://pair-code.github.io/understanding-umap/](<https://pair-code.github.io/understanding-umap/>)
* [https://towardsdatascience.com/how-exactly-umap-works-13e3040e1668](<https://towardsdatascience.com/how-exactly-umap-works-13e3040e1668>)
* [https://towardsdatascience.com/how-to-program-umap-from-scratch-e6eff67f55fe](<https://towardsdatascience.com/how-to-program-umap-from-scratch-e6eff67f55fe>)
* Category theory behind UMAP: https://johncarlosbaez.wordpress.com/2020/02/10/the-category-theory-behind-umap/

Pretty simulation / interactive explanation: https://tiga1231.github.io/umap-tour/

Narayan, A., Berger, B., & Cho, H. (2020). Density-Preserving Data Visualization Unveils Dynamic Patterns of Single-Cell Transcriptomic Variability. bioRxiv.
https://www.biorxiv.org/content/10.1101/2020.05.12.077776v1?ct=
Like UMAP but also doesn't equalize density of points, but tries to preserve it. Important for good visualizations of real data. Trains a small deep network in the background (TF + Keras) to learn the transformation.
https://umap-learn.readthedocs.io/en/latest/densmap_demo.html
As of Dec 2020, the `densmap=True` version of IMAP wasn't yet in the default conda install, even tho the documentation totally describes it.

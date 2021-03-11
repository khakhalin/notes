# Inception in visual cortex: in vivo-silico loops reveal most exciting images

Walker, E. Y., Sinz, F. H., Froudarakis, E., Fahey, P. G., Muhammad, T., Ecker, A. S., ... & Tolias, A. S. (2018). . *bioRxiv*, 506956. 
https://www.biorxiv.org/content/biorxiv/early/2018/12/28/506956.full.pdf 

Parent: [[deepneuro]]
 
#deepneuro

Show images to an animal, while chronically recording from the cortex. Train ANN to produce cortical recordings from these images. Then use this ANN to synthesize "most exciting images" for various neurons. Test these images, show that they work better than "linear RFs".

" Each network consisted of a convolutional core computing nonlinear features from the image, a readout predicting the neuronal responses from the core, a shifter network accounting for eye movements, and a modulator network predicting an adaptive gain for each neuron based on behavioral variables " - ???

The model is described in the methods, but I skipped it for now.
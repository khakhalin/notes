# TF and Keras

#tools #dl

Related: [[01_Tools]], [[06_DL]]

Intro concepts:
* **Tensor object**:  type, shape, and a bunch of numbers. For example, when working with images, we have a 4D structure: image# × W × H × ColorChannels.
* TF relies on a function that iterates through (features, labels) as tuples. For parallel (aka lazy) execution, instead of directly linking to the data, it can receive a data-generating function.

**Data Pipeline**: 
TF can take a Python iterable. If data can be loaded in memory, use `tf.data.Dataset.from_tensors()` or `from_tensor_slices()`. Later, a dataset object may be transformed using various TF methods, like `shuffle()` for reshuffling, or `batch(bath_size)` to create a batched dataset.

When building the model, for the first layer, set `input_shape` to a tuple describing the dimensions, assuming that the first (silent, unspecified) dimension will be the batch size. TF can also directly 'cosume' Python generators (functions that produce values lazily, using `yield` instead of `return`), although documentation warns that there are some potential traps with this.

Three different ways to fit: _Isn't it obsolete now? I think it is actually! At least the first two are now combined as two modes of working with fit(), I think; check the 3d one!_
* `fit` - best choice for small datasets. Takes x and y explicitly; you specify `batch_size` as a parameter, as it is the fit that will do batching. Good if entire set fits in RAM, and there's no data augmentation (no on-the-fly data, so to say). [ref](https://www.pyimagesearch.com/2018/12/24/how-to-use-keras-fit-and-fit_generator-a-hands-on-tutorial/)
* `fit_generator` - accepts a data generator instead of static data, and in this case it is the Dataset generator object (or your custom function) that specifies the shape, including batch size. With `steps_per_epoch`, you can tell it just how many new data points to generate per epoch. Note that with fit_generator, you'll have an extra dimension for batch in the model, at the inputs level, so for prediction you'd either have to use `predict_generator` (which makes sense if you generate data for it on the fly), or expand x with a leading empty axis using numpy (np.reshape(1, … )), and then np.squeeze(y) the output back to normal.
* `train_on_batch` - trains on one batch and updates the model.

Practical advice from FCH:
1 - Test the parts before you test the whole
2 - Use `model.summary()` and `plot_model()` to check output shapes and connectivity graph
3 - Use `run_eagerly=True` in `compile()` to debug training step-by-step
4 - Use the TF profiler (simple callback) to fix performance issues

# Eager execution

In TF2, enabled by default. To test, check the value of `tf.executing_eagerly()` (should be True).

Refs:
* Official description: https://www.tensorflow.org/guide/eager

# Selected references

On sequential model maniuplations: freezing-unfreezing (for transfer learning), extracting outputs of deep layers (for representation building), pre-training etc:
https://keras.io/guides/sequential_model/

Several reasonable tutorials: https://github.com/sayakpaul/TF-2.0-Hacks/blob/master/README.md


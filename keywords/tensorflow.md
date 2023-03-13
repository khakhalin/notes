# TF and Keras

#tools #dl

Related: [[01_Tools]], [[06_DL]]

## Intro concepts

* **Tensor object**:  type, shape, and a bunch of numbers. For example, when working with images, we have a 4D structure: image# × W × H × ColorChannels. See also: [[tensor]]
* TF relies on a function that iterates through (features, labels) as tuples. For parallel (aka lazy) execution, instead of directly linking to the data, it can receive a data-generating function.

**Data Pipeline**: 
For data, TF can take a Python iterable. If data can be loaded in memory, reference it directly; otherwise it's possible to make a function that feeds data lazily (`yield` instead of `return`).

> What does this mean? : Is it obsolete, or is it another way to use the library? 

> use `tf.data.Dataset.from_tensors()` or `from_tensor_slices()`. Later, a dataset object may be transformed using various TF methods, like `shuffle()` for reshuffling, or `batch(bath_size)` to create a batched dataset.

When building the model, for the first layer, either use `keras.layers.input` with `shape` param, or set `input_shape` param in a "normal layer". Shape is  a tuple describing the dimensions **of one data point**. Later, when running `model.summary()`, another silent dimension of value None will be added upfront, for minibatch **batch size**.

To **fit the model**, TF2.0 has only one universal function (not 3 different ones as was the case in TF1.0): `fit`. It takes x and y explicitly; you specify `batch_size` as a parameter, as it is the fit that will do batching. Good if entire set fits in RAM, and there's no on-the-fly data augmentation.

> I is also possible to work with generators, that in TF1.0 was handled by `fit_generator`, with variable `steps_per_epoch`, but I'd need to read about it when I need it.

To check if the model is reasonable: `model.summary()`, or `tf.keras.utils.plot_model(model)` to see a nice block diagram with lines connecting them and all sizes specified (this one requires pydot).

# Eager execution

In TF2, enabled by default. To test, check the value of `tf.executing_eagerly()` (should be True).

Refs:
* Official description: https://www.tensorflow.org/guide/eager

# Computational Graph

How to make TF2.0 code fast by building a computational graph using tf.function:
https://medium.com/towards-artificial-intelligence/learning-tensorflow-2-use-tf-function-and-forget-about-tf-session-67848eb33a2

# TF profiler

#todo

# Refs

On sequential model maniuplations: freezing-unfreezing (for transfer learning), extracting outputs of deep layers (for representation building), pre-training etc:
https://keras.io/guides/sequential_model/

Several reasonable tutorials: https://github.com/sayakpaul/TF-2.0-Hacks/blob/master/README.md


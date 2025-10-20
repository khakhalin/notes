# TF and Keras

Related: [[tools]], [[dl]]

#tools #dl


# Intro concepts

**Tensor object** knows its  type, shape, and contains a bunch of numbers. For example, when working with images, we have a 4D structure: image × W × H × ColorChannels. See also: [[tensor]]

> use `tf.data.Dataset.from_tensors()` or `from_tensor_slices()`. Later, a dataset object may be transformed using various TF methods, like `shuffle()` for reshuffling, or `batch(bath_size)` to create a batched dataset.

# Building a model

When building the model, for the first layer, either use `keras.layers.input` with `shape` param, or set `input_shape` param in a "normal layer" object. Shape is  a tuple describing the dimensions **of one data point**. Later, when running `model.summary()`, another silent dimension of value None will be added upfront, for minibatch **batch size**.

To check if the model is reasonable: `model.summary()`, or `tf.keras.utils.plot_model(model)` to see a nice block diagram with lines connecting them and all sizes specified (this one requires pydot).

# Training the model

To **fit the model**, TF2.0 has only one universal method: `fit()` (TF1 had 3 different methods). It takes x and y explicitly; you also specify `batch_size` as a parameter, as it is this method that will do batching. Good if entire set fits in RAM, and there's no on-the-fly data augmentation.

When `validation_split` is specified, validation data is taken from the end of the data input, so if you want it to be shuffled, shuffle it in advance.

# Eager execution

In TF2, enabled by default. To test, check the value of `tf.executing_eagerly()` (should be True).

Refs:
* Official description: https://www.tensorflow.org/guide/eager

# Lazy execution (the one with iterators)

TF relies on a function that iterates through (features, labels) as tuples. If lazy or parallel execution is desired, instead of directly linking to the data (providing a Python variable), we need to pass  a generator. Python generators have the same structure as a function, but use a  `yield` keyword in stead of `return`. Generators are great for large data, but I'm guessing, can also be used for data that is generated on the fly?

# Computational Graph

How to make TF2.0 code fast by building a computational graph using tf.function:
https://medium.com/towards-artificial-intelligence/learning-tensorflow-2-use-tf-function-and-forget-about-tf-session-67848eb33a2

# TF profiler

#todo

# Refs

On sequential model maniuplations: freezing-unfreezing (for transfer learning), extracting outputs of deep layers (for representation building), pre-training etc:
https://keras.io/guides/sequential_model/

Several reasonable tutorials: https://github.com/sayakpaul/TF-2.0-Hacks/blob/master/README.md


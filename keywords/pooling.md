# Pooling layer

#dl

Parents: [[dl]],  [[convnet]]
Related: [[softmax]]

**Maxpool**: Only retains the maximal value, and drops all the rest. Mostly used in convolutional networks, in which case it has dimensions, depth, and a stride. Has no learnable parameters. At backpropagation step, it only propagates to those neurons that contributed to the selected value (max value); all other neurons aren't updated. Good for downsampling feature maps, where the presence of a feature (quantified by different components of the depth-vector) is more important than the precise position of this feature.

Extreme version of this: **Global pooling** - entire tensor in one vector (equivalent to doing maxpooling with W and H of full image). A faster alternative to having a fully connected layer from a convolutional layer to a global feature vector.

**Average pooling**: as clear from the name, just averages all the inputs. Good for downsampling.

**How to backprop through a max-pooling layer?**

* RNN is usually a "Recurrent Neural Network" (not recall?). Also it's not usually an alternative for a convnet, unless you're talking about sound specifically.
* Why zero-padding around the image?
* Having no trainable parameters is not necessarily bad, no? I'm actually not sure they are used less. The latest paper this chapter referenced is from 2016… - We can check inception and see!
* Batch normalization - what is it and why is it used? (I'm not sure it is used any less honestly...)

# Refs

https://machinelearningmastery.com/pooling-layers-for-convolutional-neural-networks/
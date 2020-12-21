# Pooling layer

#dl

Parents: [[06_DL]],  [[convnet]]
Related: [[softmax]]

**Maxpool**: Only retains the maximal value, and drops all the rest. Mostly used in convolutional networks, in which case it has dimensions, depth, and a stride. Has no learnable parameters. At backpropagation step, it only propagates to those neurons that contributed to the selected value (max value); all other neurons aren't updated. Good for downsampling feature maps, where the presence of a feature (quantified by different components of the depth-vector) is more important than the precise position of this feature.

Extreme version of this: **Global pooling** - entire tensor in one vector (equivalent to doing maxpooling with W and H of full image). A faster alternative to having a fully connected layer from a convolutional layer to a global feature vector.

**Average pooling**: as clear from the name, just averages all the inputs. Good for downsampling.

# Refs

https://machinelearningmastery.com/pooling-layers-for-convolutional-neural-networks/
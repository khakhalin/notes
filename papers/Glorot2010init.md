# Glorot 2010 - the famous init paper

Glorot, X., & Bengio, Y. (2010, March). Understanding the difficulty of training deep feedforward neural networks. In Proceedings of the thirteenth international conference on artificial intelligence and statistics (pp. 249-256).

#dl #init

See also (an improment): [[He2015init]]

The first idea (?) to make the variance of weights in each layer related to the size of this layer, to preserve the variance of signal as it propagates through the layers. Shows that for a liniar case, $\text{sd}(w) \sim \sqrt{1/n}$, where n is the size of that layer.

The paper describes two different, but similarly scaled types of init:

* **Scaled normal distributions**: Traditionally called Xavier init (after this paper by Xavier Glorot)
* **Scaled uniform distributions**: Traditionally called Glorot init (after this same paper by same Xavier Glorot). Default init in [[tensorflow]] for conv layers.

Why first and last names of one person got their own independent lives  is a historical oddity, apparently.

# See also:

A list of inits in Keras that have both Xavier init and Glorot init:
https://keras.io/api/layers/initializers/

That Glorot Uniform is the default init for TF layers:
https://www.tensorflow.org/api_docs/python/tf/keras/layers/Conv2D
https://www.tensorflow.org/api_docs/python/tf/keras/layers/Dense
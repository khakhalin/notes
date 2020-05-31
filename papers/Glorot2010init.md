# Glorot 2010 - the famous init paper

Glorot, X., & Bengio, Y. (2010, March). Understanding the difficulty of training deep feedforward neural networks. In Proceedings of the thirteenth international conference on artificial intelligence and statistics (pp. 249-256).

#dl #init

See also (an improment): [[He2015init]]

The first idea (?) to make the variance of weights in each layer related to the size of this layer, to preserve the variance of signal as it propagates through the layers. Shows that for a liniar case, $\text{sd}(w) \sim \sqrt{1/n}$, where n is the size of that layer.
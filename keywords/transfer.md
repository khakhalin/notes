# Transfer learning

#transfer

Parents: [[06_DL]], [[08_Self_Supervised]]

See also: [[style_transfer]]

Papers:
* [[Zoph2020pretraining]] - on limited use of pre-training

For transfer learning, one would typically train a model on one set, then freeze the layers, and apply it to a different set. When mixing pre-trained layers with random ones, it's important to have the pre-trained layers frozen, or later random layers would generate huge random gradients, disrupting the pre-trained layers. It is possible however to **fine-tune** pre-trained layers later, by unfreezing them already after convergence, and probably with a very gentle learning rate. (ref: [keras tutorial](https://keras.io/guides/transfer_learning/))
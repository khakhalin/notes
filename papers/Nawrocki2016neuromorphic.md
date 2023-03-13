# A mini review of neuromorphic architectures 2016

Nawrocki, R. A., Voyles, R. M., & Shaheen, S. E. (2016). A mini review of neuromorphic architectures and implementations. IEEE Transactions on Electron Devices, 63 (10), 3819-3829.
https://www.academia.edu/download/55043930/IEEE_TED2016_Neuromorphic_Review.pdf 

Parent: [[neuromorphic]]

#neuromorphic #review


"Alan Touring" typo in the very first row!! Oufff!

Term coined by Mead in 1990. Power consumption contrast between supercomputers and human brain. Assume that it is because of the architectures, and not hardware?

**Information is stored in a distributed way**: stored and processed by same networks. Networks themselves may be spiking or non-spiking (with spiking approaches being older actually). Levels of biological realism also differ. Some people go with things that are simpler mathematically (e.g. some "tau-cell"); some try to use non-linear objects that are easiest to manufacture (like logic gates), or those that are most similar to ANNs (e.g. sigmoid activation).

**Silicon retinas** with parallel processing of information, one of the more successful products. Lower power consumption than conventional approaches.

Keyp players:
* **Neurogrid** (Standord): an attempt to create a universal programmable chip, modeled after mammalian cortex, and supposedly suitable for testing hypotheses about the cortex. "8 transistors per soma, integrate-and-fire neurons" (they call it "low biological realism", but low power consumption and very high complexity). **asynchronous handshaking protocol** to transmit events (???)
* ROLLS at Zürich
* SpiNNaker at Manchester University
* Neural Processing Unit (NPU) by Qualcomm
* DARPA worked with IBM on that
* Blue Brain (_can we now reliably say that it failed?_)

Another, alternative approch: looking for a **Memristor**: a "memory resistor", first theoretically postulated in 1970s. The idea is that its resistance depends on the history of current that flew through it, and is stored even when the system is off. A logical concept for which people are trying to find an ideal physical representation, NOT a physical phenomenon for which people are trying to find some use. https://en.wikipedia.org/wiki/Memristor

**CMOL**: CMOS nanowire MOLecular nanodevice, where CMOS stands for "complementary metal-oxide-semiconductor", and that's just a name for how modern silicon electronics is made. Different universities worked on that, and demonstrated [[stdp]], 1-layer perceptron, Hebbian learning etc. IBM had a neuromorphic project.

And then there are also alternative implementations, like organic semiconductors (??).

> They didn't mention optical techniques; perhaps they didn't really exist in 2016?

# Refs

Neurogrid:
* ] R. Silver, K. Boahen, S. Grillner, N. Kopell, and K. L. Olsen, “Neurotech for neuroscience: Unifying concepts, organizing principles, and emerging tools,” J. Neurosci., vol. 27, no. 44, pp. 11807–11819,
* P. Gao, B. V. Benjamin, and K. Boahen, “Dynamical system guided mapping of quantitative neuronal models onto neuromorphic hardware,” IEEE Trans. Circuits Syst. I, Reg. Papers, vol. 59, no. 10, pp. 2383–2394, Oct. 2012.

# JAX

#tools

Parents: [[tools]]

Main ref: https://jax.readthedocs.io/en/latest/jax-101/index.html

# Random generators

In JAX, random generator needs a **key** (something like a seed) at any use. as it is **stateless**. The seed isn't modified at use, so calling `normal(key)` repeatedly will always produce the same result. To get a new random something, you need to explicitly generate new key using `random.split`. It means that you always control the flow of randomness, which is critical for both reproducibility and parallelized calculations. _Not sure how exactly it is implement, but sounds right. You just split one key into many subkeys, then every thread uses its subkey. Not sure how it can support variable number of threads, but at least it's definitely more control than in a standard stateful random generator._

Footnotes:
* https://jax.readthedocs.io/en/latest/jax.random.html
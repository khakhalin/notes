# Lévy Walks

#behav #exploration

_Theory goes here_

# Among animals

Fruitflies, sharks, and albratrosses all use **Levy walk** as random exploration strategy. Levy walk is like a Brownian motion, but with occarional very far "jumps". For whale sharks this works even in 3D, not just 2D (Sims 2008).

When the target is large and slow (like, a collection of food), Levy walks are somehow probabilistically the optimal strategy of finding it (Bartumeus 2003)

### Refs

Levy walk simulation:
https://www.complexity-explorables.org/explorables/anomalous-itinerary/

Levy walks in different animals, overall summary:
https://www.quantamagazine.org/random-search-wired-into-animals-may-help-them-hunt-20200611/

* Fruitfly larva: Sims, D. W., Humphries, N. E., Hu, N., Medan, V., & Berni, J. (2019). Optimal searching behaviour generated intrinsically by the central pattern generator for locomotion. eLife, 8.
* Marine predators: Sims, D. W., Southall, E. J., Humphries, N. E., Hays, G. C., Bradshaw, C. J., Pitchford, J. W., ... & Morritt, D. (2008). Scaling laws of marine predator search behaviour. Nature, 451(7182), 1098-1102.
* Albatrosses: 
    * Original flawed paper (didn't account for nesting, and for olfaction): Viswanathan, G. M., Afanasyev, V., Buldyrev, S. V., Murphy, E. J., Prince, P. A., & Stanley, H. E. (1996). Lévy flight search patterns of wandering albatrosses. Nature, 381(6581), 413-415.
    * Newer cool paper showing that over deep water they actually do Levy walks: Humphries, N. E., Weimerskirch, H., Queiroz, N., Southall, E. J., & Sims, D. W. (2012). Foraging success of biological Lévy flights recorded in situ. Proceedings of the National Academy of Sciences, 109(19), 7169-7174.
* Sharks in 3D (or rather 1D, up and down, which is apparently also a Levy walk): Sims, D. W., Southall, E. J., Humphries, N. E., Hays, G. C., Bradshaw, C. J., Pitchford, J. W., ... & Morritt, D. (2008). Scaling laws of marine predator search behaviour. Nature, 451(7182), 1098-1102.

That Levy walks are optimal:
* Bartumeus, F., Peters, F., Pueyo, S., Marrasé, C., & Catalan, J. (2003). Helical Lévy walks: adjusting searching statistics to resource availability in microzooplankton. Proceedings of the National Academy of Sciences, 100(22), 12771-12775.
# CI = Continuous Integration

Path: [[devops]], [[coding]]
Related: [[microservice]], [[kubernetes]], [[design_patterns]], [[unit_test]]

#devops #systems


The idea is that it is hard to test and add features to a piece of code that is constantly running. So people came up with some tricks.

At least in its simplest form, CI seems the idea of feature branches, which is the dominent version control approach (see [[git]]). Typically, in CI the update you push starts to run in dev. But if you have 2 branches, which one will run in dev?
* One approach (the worst one, that we don't endorse!) is to build features in tiny increments (so that each of them can be tested in dev, until the entire feature reaches prod). Once something works on dev, push further in prod.
* Another approach is to use **feature toggles** that allow new features to be turned on an off, either by the user, or (more likely) via environmental variables that can be set up differently for dev and prod.
* Yet another approach is to just have different feature branches, and automatically deploy to dev whichever branch is active right now. Once it works, you merge it to master, and develop it to int and prod. This way you can only test one thing at a time, but you can develop many things concurrently, and you don't have to overcomplicate it with feature toggles. (Even tho this approach can also be combined with feature toggles).

# Refs

https://www.innoq.com/en/blog/continuous-integration-contradicts-feature-branches/
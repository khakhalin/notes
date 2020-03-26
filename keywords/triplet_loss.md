# Triplet Loss

#self-supervised

Described in a paper I haven't yet read:  Schroff2015facenet

Say, you want the model to learn that all objects in group A belong to class A, etc. If you just reward low Δ score for objects in A, model will map everything to const and achieve Δ = 0. So instead take 2 objects from A and 1 object from B, and make the model minimize $D_+$ = D(a1,a2) relative to $D_-$ = D(a1,b). 

Say, you can drive $max(0,D_+ + (M-D_-))$ down, where M is some characterist distance that we want to promote between objects from different classes. 

> In the paper, they apparently use some other tricks as well. 

Paper: FaceNet: A Unified Embedding for Face Recognition and Clustering Florian Schroff, Dmitry Kalenichenko, James Philbin
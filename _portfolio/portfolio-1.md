---
title: "Summer Internship 2018"
excerpt: "A Distributed Identification System on the Blockchain"
collection: projects
---

This summer, I had the opportunity to work with a nonprofit corporation called the [Kuwa Foundation](http://kuwa.org/), which aimed to deliver Universal Basic Income through the use of cryptocurrency-based systems. The main challenge in implementing such a system is to solve the problem of uniquely identifying people and prevent the same person from registering for multiple accounts(also known as Sybils). The motivation: multiple payments to some users(while the others receive less from the same system) are unfair, because it violates the "Universal" in Universal Basic Income.

My responsibilities included coding a system called the *Kuwa Registrar*, the entity which performs the Sybil Detection and marks a new registrant as either valid or invalid. 

For the Sybil Detection, I started with an extremely simple SHA-256 Hash on a video file that all new registrants must submit. However, this is obviously not enough to reliably differentiate people, because it does not consider the video as that of a person, but rather a chunk of bytes. Also, even if the same person uploads a second video(the system doesnt require it, but it may happen), the hash of the new video would be different from the initial one, and the system might label the person as a duplicate. The answer to reliable Sybil Detection was to use Machine Learning to recognize the face in the video and determine identity based on the faces.

To perform the above task, I used a deep neural network to generate face embeddings, described in detail [here](http://blog.dlib.net/2017/02/high-quality-face-recognition-with-deep.html). The network takes an image containg a face, and generates a vector(or a list) of numbers. This vector encodes properties of the face, and even simple measures like Euclidean Distance can be used to differentiate between faces.

The design was simple and intuitive enough, but the implementation posed a challenge. The best neural networks are written either in Python or C++. The rest of the project was a set of RESTful services, done almost entirely in NodeJS. So, I had to somehow use NodeJS for running a neural network written in one of the aforementioned languages. Finally, I used the [dlib](http://blog.dlib.net/2017/02/high-quality-face-recognition-with-deep.html) library(written in C++), with bindings to NodeJS provided by a library called [face-recognition.js](https://github.com/justadudewhohacks/face-recognition.js). Additionally, to read images and preprocess them for the neural network, I used another library called [opencv4nodejs](https://github.com/justadudewhohacks/opencv4nodejs).

You can check out the Kuwa repository on Github [here](https://github.com/jimflynn/kuwa-tcup). To learn more about the system, you can refer to this [white paper](https://jamespflynn.com/2018/03/01/kuwa-a-decentralized-pseudo-anonymous-and-sybil-resistant-individual-identification-system/) or this [pdf](http://kuwa.org/Kuwa-Driven_Basic_Income_Faucet.pdf).
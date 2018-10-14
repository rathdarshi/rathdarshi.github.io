---
title: 'Summer Internship'
date: 2018-10-14
permalink: /posts/2018/10/summer-internship-2018/
<!-- tags:
  - Internship
  - category1
  - category2 -->
---

This summer, I had the opportunity to work with a nonprofit corporation called the [Kuwa Foundation](http://kuwa.org/), which aimed to deliver Universal Basic Income(UBI) through the use of a cryptocurrency-based "faucet" using the [Ethereum](https://www.ethereum.org/) blockchain. The main challenge in implementing such a system is to solve the problem of uniquely identifying people and preventing the same person from registering for multiple accounts(also known as *Sybils*). Why are duplicates an issue? Because duplicates violate the theory behind UBI by hoarding a bigger share of the resources than provided to them.

Registration for the system works as follows: a person signs up by first selecting a password for their crypto wallet(which will receive their UBI). Once their wallet is ready, they are sent a passphrase which they must respond to, by taking a video of themselves and uploading it to the system. Based on the video, their identity is verified as either unique(*Valid*) or duplicate(*Invalid*). The system requires no other data from the user, making it minimally intrusive.

From among the various modules of the system(decribed [here](http://kuwa.org/Kuwa-Driven_Basic_Income_Faucet.pdf)), my responsibilities included implementing a module called the *Kuwa Registrar*, which performs Sybil Detection and marks a new registrant as either *Valid* or *Invalid*.

For the Sybil Detection, I started with an extremely simple SHA-256 Hash on a video that all new registrants must submit. However, this is obviously not enough to reliably differentiate people, because this method does not consider the video as a person with an identity, but rather a chunk of bytes. So, the answer to reliable Sybil Detection was to use Machine Learning to recognize faces in the videos uploaded to the system and determine people's identity based on that.

To perform the above task, I used a deep neural network to generate face embeddings, described in detail [here](http://blog.dlib.net/2017/02/high-quality-face-recognition-with-deep.html). The network takes an image containg a face, and generates a vector(or a list) of numbers. This vector encodes properties of the face, and even simple measures like Euclidean Distance can be used to differentiate between faces.

The design was simple and intuitive enough, but the implementation posed a challenge. The best neural networks out there are written either in Python or C++. The rest of the project was a set of RESTful services, done almost entirely in NodeJS. I had to somehow use NodeJS for running a neural network written in one of the aforementioned languages. Finally, I used the [dlib](http://blog.dlib.net/2017/02/high-quality-face-recognition-with-deep.html) library(written in C++), with bindings to NodeJS provided by a library called [face-recognition.js](https://github.com/justadudewhohacks/face-recognition.js). Additionally, to read images and preprocess them for the neural network, I used another library called [opencv4nodejs](https://github.com/justadudewhohacks/opencv4nodejs).

You can check out the Kuwa Foundation repository on Github [here](https://github.com/jimflynn/kuwa-tcup). To learn more about the system, you can refer to this [white paper](https://jamespflynn.com/2018/03/01/kuwa-a-decentralized-pseudo-anonymous-and-sybil-resistant-individual-identification-system/) or this [pdf](http://kuwa.org/Kuwa-Driven_Basic_Income_Faucet.pdf).
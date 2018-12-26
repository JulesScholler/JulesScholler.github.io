---
layout: page
title: "Ranking portrait pictures using AI"
subtitle: "Challenge Data 2017"
comments: true
gh-repo: JulesScholler/Regaind-ChallengesDATA
gh-badge: [star, fork, follow]
---

So this project was a lot of fun. It was proposed by Regaind on [Challenge Data](https://challengedata.ens.fr/en/home). I teamed up with [Leo Paillier](https://github.com/leo-p) and we ended up [winning](https://www.sciencesmaths-paris.fr/upload/Contenu/MathsInfos/MathsInfos37_web.pdf) this challenge. Our report is available [here](/pdf/Report_REGAIND.pdf).

## Purpose

The objective of this challenge was to predict an aesthetic score for portrait photos. This problem is complex for at least three reasons: (i) images belongs to a very high dimension space (several millions), (ii) what makes a picture beautiful can sometime be very subjective and finally (iii) portraits specificity where a minor change in the pan angle or a closed eye for instance can have a major impact on its perceived beauty.

## Methods

The first step was to read a lot about art, pure art. When we understood what makes a good photo we started to code functions that could capture the properties that we just read about in portraits. You might think that beauty is easy but try to explain why a given photo is beautiful, it's an hard task. We are able to rate it, at least decide if it's a good shot or a bad one. We quickly learnt that it is way easier to capture a bad portrait, because there is a problem with the lighting or there is blur and those features are easy to capture. The hard part of this project was to compare **beautiful** shots. We build for each portrait a feature vector and then we trained a SVM with a properly scaled Gaussian Kernel with these features.

Our code is available [here](https://github.com/JulesScholler/Regaind-ChallengesDATA).

---
layout: page
title: "Ranking portrait pictures using AI"
subtitle: "Winner Challenge Data 2017"
comments: true
gh-repo: JulesScholler/Regaind-ChallengesDATA
gh-badge: [star, fork, follow]
---

So this project was a lot of fun. It was proposed by Regaind on [Challenge Data](https://challengedata.ens.fr/en/home). I teamed up with [Leo Paillier](https://github.com/leo-p) and we ended up [winning](https://www.sciencesmaths-paris.fr/upload/Contenu/MathsInfos/MathsInfos37_web.pdf) this challenge without using Neural Networks. Our report is available [here](/pdf/Report_REGAIND.pdf).

## Purpose

In this challenge the goal was to develop an algorithm that could rate portrait pictures. At the time, such algorithms were working fine on landscapes but not for portraits. The problem with portraits is that a tiny change in the picture (e.g. an eye is closed, or a funny face) could ruin it and that these changes are hard to capture by an algorithm.

## Methods

The first step was to read a lot about art, pure art. When we understood what makes a good photo we started to code functions that could capture the properties that we just read about in portraits. You might think that beauty is easy but try to explain why a given photo is beautiful, it's an hard task. We are able to rate it, at least decide if it's a good shot or a bad one. We quickly learnt that it is way easier to capture a bad portrait, because there is a probleme with the lighting or there is blur and those features are easy to capture. The hard part of this project was to compare **beautiful** shots.

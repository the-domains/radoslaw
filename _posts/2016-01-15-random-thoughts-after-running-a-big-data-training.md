---
inFeed: true
hasPage: true
inNav: false
inLanguage: null
starred: false
keywords: []
description: ''
datePublished: '2016-01-18T20:20:19.297Z'
dateModified: '2016-01-18T20:20:15.787Z'
title: "Random thoughts after running a Big Data Training\_"
author: []
sourcePath: _posts/2016-01-15-random-thoughts-after-running-a-big-data-training.md
published: true
authors: []
publisher:
  name: null
  domain: null
  url: null
  favicon: null
url: random-thoughts-after-running-a-big-data-training/index.html
_type: Article

---
# Random thoughts after running a Big Data Training ![](https://s3-us-west-2.amazonaws.com/the-grid-img/p/203cbf25bbb191bb79b08223d2fd5f645cfda74f.jpg)

Recently I had a possibility to run a two day training for one of the software houses in Szczecin. 

## Agenda

Training was tailored for customer needs and consisted of 5 huge parts:

* Big Data Introduction
Hadoop introduction (architecture, commands, usage) 
* Map Reduce (paradigm, hadoop framework, MRUnit) 
* Pig (getting started guide, different running modes, PigUnit) 
* Hive (introduction, serdes, file formats, integrations) 
* Elastic Search & Kibana (introduction, search concept, integration with Hadoop Ecosystem, Kibana)

## Feedback

Training finished with very good notes, but surveys shown that there are places where I can do better and this post is about those places and many more I have found out by myself. 

Following thoughts are in random order, writing them down probably will help me in preparing next presentations or trainings :-) Someone maybe will find them helpful?

## Production experience

In every agenda point except Elastic Search I have production or PoC experience. For Elastic Search I didn't have problems in making training material (I have some experience), but I had trouble to find some answers about running it on production - scaling, clustering, administering. One of the funniest mistakes was when everyone started elastic search and by accident we have created huge cluster because Elastic Search instance finds other nodes by multicasting in subnet. 

## Elastic Search Query DSL

I find Elastic Search query language quite hard to learn and teach. There is no 3-5 page cheat sheet like in Pig or Hive - whole documentation is like very long tutorial. It is difficult to organize it into short guide.
I tried to teach by showing example queries which may be copied and applied to different datasets, but it lacks of reference, schema - they're still just examples.
[Sense (Beta)][0] Chrome extensions is very useful for writing queries. There is small context assist, it helps to find some misspells or missing bracket but it doesn't validate scheme'a like it possible with XSD on XML. 

## Workshop tasks

All participants were developers with good Java and Python experience but when facing new challenge (new paradigm, new language) the learning time vary. It may happen that some participants were finishing their tasks two times faster than others. What to do then?
When there is more than 10 participants it may be difficult to check progress of everyone. 

On my training few time it happened that people were waiting because I haven't noticed that they've finished. 
I'm thinking about some solutions for this case:
prepare some kind progress bar based on post-it tasks or find solution online.
prepare core tasks for everyone, and many additional tasks that can broaden participant knowledge, participants that make tasks slower can take additional tasks as homework 

## Breaks 

One of the keys of training success is good mood of participants. First day I was so focused on presentation that I forgot about some brakes so everyone was tired. Next day I made short break every hour. It was better. People could share experience, ask more questions etc. Next time I have to organize some kind of timer. 

## Material length

I was worried that my presentation and workshop material are too short. They weren't. They were too long. Probably I need to give more presentations to have a good timing experience. For workshop length there is good proportion: 1h of presentation should have 2-3 hours of workshop. 

## Elastic Search basics 

Is it possible to teach basics Elastic Search with Kibana in 3 hours? I don't think so.

## Keynote iOS app 

I could see notes, change slides, draw on them. It is not perfect app, from time to time it was disconnecting from my keynote app, probably Wi-Fi disconnected on iPhone, drawing was quite slow. For presentation I recommend trying to run it on personal hotspot so no-one interfere in quality of wifi network. If you present on Mac - Buy keynote iOS app. 

## Cheat sheets

I found Mortar's Pig and Hortonworks Hive cheat sheets very helpful but they lack of some basic functionalities. For example creating table in hive with custom delimiter. So when participants want to write something really basic and they search for reference I have to point them slides or website. Probably will have to write my own cheatsheets. 

## Programming Environment 

One of key points I wanted to proove in training was showing that most of the training material can be finished without use of cluster or Hadoop itself. For that I have created starter projects on github for Pig and Java Map Reduce that participants could download and fill with their code. They could write MRUnit tests, run Map Reduce Java job in local mode (without cluster or hdfs), write Pig scripts and test them with PigUnit. But I have faced some problems as well:

* For this training I told that Mac OS or Linux is required but two participants had Windows so they couldn't finish their tasks - For Windows systems it is required to have binaries of Hadoop. It has taken me about 4-5 hours to organise all dependencies and compile Hadoop 2.5 so it may be difficult to give it as a prerequisite for training. Next time probably I need to provide this as a downloadable package for participants (both versions - 32bit and 64bit)
* Pig Unit is not very well documented and there is not so much development around it - is it good idea to teach about it? 

## Sandbox

On the opposite to IDE only training, there are Hadoop distributions that one can download and install on laptop. Hortonworks and Cloudera have Hue which is very helpful when learning Pig or Hive. They help creating tables, importing flat files. Pig query page has a dropdown menu with all useful commands so there is no problem with syntax. 

Cons: 

* The main problem with Sandbox is speed. VM is eating 4GB of memory and even easiest job which should finish in 10 seconds starts more than one minute on Intel i5 with 8GB of ram
* VM is huge - there is always a participant that forgets to download an image and downloading 5GB during training may be difficult because. Always have it on pendrive

Pros: 

* When skipping workshop related to writing Java Map Reduce or UDF, you can run whole training on this VM 
* Hue helps a lot with learning Pig syntax in comparison to context help in Grunt 
* We could run interesting scenario - find when ORC (binary columnar file format) is faster than default file format in Hive for specific query

## Gist and Github

I found Github very useful to share starter mavenized projects. Most of developers already use github and they didn't have any problems with forking, downloading and importing the projects. 
Gist was very helpful when sharing simple command lines for downloading, extracting binaries, running applications etc.
I remember a slide full of command lines and asking participant to write commands which always finished with a lot of typing errors - with gist it won't happen anymore. 

By the way, many thanks to [Sages Sp z o.o.][1] for this opportunity :-)
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/a9ca3def-9d42-4bfe-8d73-0e2286c886e2.jpg)

[0]: https://chrome.google.com/webstore/detail/sense-beta/lhjgkmllcaadmopgmanpapmpjgmfcfig
[1]: http://www.sages.com.pl/
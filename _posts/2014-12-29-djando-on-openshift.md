---
layout: post
title: djando on Openshift
categories: [web-development, ]
tags: [django, openshift]
published: True

---

[Openshift](https://www.openshift.com/) is a new app hosting service from RedHat which offers a great free package. Till now I've used Heroku to host some apps and its great but it doesn't provide any free database (well it does but you'll have to enter your credit card details which in India few people have). Also the filesystem of heroku is such that it refreshes every 24hrs deleting every thing except the repo. So heroku is great for testing but hosting something with somesort of a database is not possible unless you pay.


In comes ***Openshift*** to the rescue. It provides a 1GB filesystem which doesnot refresh and a free database which is quiet generous. So right now I'm trying to host a django app on Openshift and I'll document the steps that I take here.


Steps:
======
1. ###  ###
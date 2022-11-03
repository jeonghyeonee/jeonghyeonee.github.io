---
layout: post
title: Singleton pattern
categories: Pattern
tags:  [Singleton, pattern, design]
---

## Singleton Pattern
### Intro
When I made Android Application, I used Database in Memory by making `Appdatabase` class. Becuase I want to very small data in Database, so I don't have to use `Shared Preferences` or `SQlite`. (Becuase sharedPreferences occur file I/O and I don't neeed to make tree(schema) to store the data(very small!!))
In this reason, I select to store data in Memory using HashMap or HashTable.
However, in this case, I have some trouble when I access to Database at the same time. If you update the data, but it is used another class.
*How to deal with **Concurrency** problem?*

### What is the Singleton Pattern

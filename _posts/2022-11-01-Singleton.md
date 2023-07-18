---
layout: post
title: Singleton pattern
categories: Pattern
lang: en
lang-ref: Singleton
tags:  [Singleton, pattern, design]
---

## Singleton Pattern
### Intro
When I made Android Application, I used Database in Memory by making `Appdatabase` class. Becuase I want to very small data in Database, so I don't have to use `Shared Preferences` or `SQlite`. (Becuase sharedPreferences occur file I/O and I don't neeed to make tree(schema) to store the data(very small!!))
In this reason, I select to store data in Memory using HashMap or HashTable.
However, in this case, I have some trouble when I access to Database at the same time. If you update the data, but it is used another class.
*How to deal with **Concurrency** problem?*

### What is the Singleton Pattern



##### References
- [Singleton Pattern](https://www.javatpoint.com/singleton-design-pattern-in-java)
- [Design Pattern - Singleton Pattern](https://www.tutorialspoint.com/design_pattern/singleton_pattern.htm)
- [싱글톤 패턴(Singleton pattern)](https://gyoogle.dev/blog/design-pattern/Singleton%20Pattern.html)
- [싱글턴 패턴(wiki)](https://ko.wikipedia.org/wiki/%EC%8B%B1%EA%B8%80%ED%84%B4_%ED%8C%A8%ED%84%B4)
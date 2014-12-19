---
layout: post_page
title: "Scope and Classes in C++"
date: 2014-12-08 18:00:33
categories: Programming, C++
---

I hope you read my previous post before looking at this one if you don't know much about OOP in C++. The previous post explains various things like naming of stuff in C++ which is different from naming conventions in other languages.

This one is about explaining scope in C++ when we have inheritance and non-default constructors as well as compositions.

The piont of this post is to establish:

1. What order are constructors called in case we create an object of a derived class.
2. What order are destructors called in case we create an object of a derived class.
3. In the case of compositions (objects as data members) which constructors are called first. The one of the composing class or the object?


Anyway let's get to it. We'll picture a


### Base classes:
- **Cars**
- **Guns**


### Subclasses:

**Customer**
  
- Current Account
- Fixed Deposit Account
- Debit Account
- Savings Account


**Employee**
  
- Management
- Tellers
- Other bank peeps

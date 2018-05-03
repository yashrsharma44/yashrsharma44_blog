---
layout: post
title: Details of my project
---

This blogpost covers the details of my project, Async Await support in **Scrapy**. With this post, I would explain my project which I would be working on during this summers.

## Details of my project : Async Await support in Scrapy



> How did I get to know about the organization ?

_**Scrapy**_ is a web scraper, which I have used to scrape websites, and get a lot of data to use them, in _Data Analytics_, and _Machine Learning_. Data is an integral part of these fields, and we need to have data, in order to progress in them. So using _scrapy_ made me realise the philosophy of the software, while open sourced nature would mean that the users can customize them accordingly.

> How did I choose the project ?

Choosing the right project is important, because it is neccessary that the goal you would be working, should align with your interests and philosophy. The most interesting part of the project that fascinated me was _**Asynchronous Programming**_ paradigm. Now the ones who have programmed in _Javascript_, they know about that; for the others, I would use an analogy.
Suppose, we have to serve 3 different dishes ordered by 3 different people, in some specific order. Synchronous way would be analogous to serve the 3 dishes in the order of sequence. While this wastes a lot of time, if the third order was completed first, but we have to serve that at last, in order to comply with the rule.

For _Asynchronous_ paradigm, the goal is simple : serve the order which finishes first, in spite of the sequence of the request. In programming terms, it is like serving the requests in order of their completion.

> How does _Scrapy_ uses the asynchronous paradigm of programming ?
 
As _**Scrapy**_ is a web scraper, it makes sense to use async programming, because web pages may not be available at time of requests, and it makes sense to use async paradigm to approach the task.
Currently, before _**Python 3.3**_, people used asynchronous paradigm, using _**generators**_. _**Scrapy**_ uses a framework, named _**Twisted**_ for the above. _**Generators**_ are iterators, which can be used to stop the iterator as and when wished, and to **resume** them, whenever we want.
Using generators, we can stop the code flow, as we can ask _**Python**_ to wait till we get the response of the request, and proceed accordingly. A relevant blog post for using generators is : **[Generators](https://jeffknupp.com/blog/2013/04/07/improve-your-python-yield-and-generators-explained/)**.

> How async await syntactic sugar relevant ?

For people who have programmed using _asynchronous paradigm_, they know the drawbacks of it, especially the _**sphagetti code**_ and _**callback hell**_. While new paradigms for asycnhronous programming has been used in _Python_( i.e generators), the main problem starts, when we want to use generators for both as tool for asynchronous programming, and as an iterator. It becomes difficult to differentiate between the two, and people contributing to these projects, might not be able to make out between them.
In order to differentiate between the two, Python has introduced two primitives, for them, _**async and await**_. These are generators under the hood, but with difference that they have their own primitive, called _**'coroutines'**_. Using the above, it becomes easy to write the asynchronous code in a synchronous manner, while mantaining the async nature of code.

> What would be my goal in this project ?
 
My objective in the project would be to support _**async / await idioms**_, so that users can write the project with the new syntactic sugar, while maintaining the backwards compatibilty of the project. At the end, we can write scraping code using the new paradigm, and frame our logic of scraping, without needing the overhead of _callbacks_.

## Links to resources
* [Generators](https://jeffknupp.com/blog/2013/04/07/improve-your-python-yield-and-generators-explained/)
* [Twisted](https://twisted.readthedocs.io)
* [Async Await syntaxes in Python](https://www.python.org/dev/peps/pep-0492/)
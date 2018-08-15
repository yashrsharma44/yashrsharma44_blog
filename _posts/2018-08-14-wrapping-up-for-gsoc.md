---
layout: post
title: Wrapping up for GSOC
---


As I discussed in the last post, about the trials with the asyncio reactor, concluding the previous week, I made lot of progress. I completed most of the requirements of the project, and in the end, made some review along with my mentor, for the quality of code. I would discuss this now –

* **Supporting Asyncio frameworks** – The previous blog discussed as to how I tested the asyncio reactor. This week, I installed asyncio reactor, and tried testing on it.  I did not write any test case, but I tried using asyncio based frameworks, such as aioredis, and aiohttp. Both were running smoothly, and based on some discussion with my mentor, I tried using specific cases for the use of asyncio framework.
* **Fixing the spider-idle bug** – Last week, I was plagued with the spider idle bug, where on running the spider, the spider used to close quite prematurely. So I thought about various scenarios, as to when the spider should be closed. After designing the implementation, I got a hold on the above problem, and the bug is now solved.

I also discussed with my mentor, for the prospect of merging the PR. While he expressed his thought that the work was good, there were some requirements, before it can be merged into the Scrapy codebase –

# Requirements before it is merged into scrapy code base
While I have made most of the API required for using async / await syntax, there remain to be some of tasks left, before it can be merged into Scrapy’s code-base.

* **Writing test suites** — While I intended to complete writing the test suites, as it was proposed in the proposal, and I did write some of them, but the main project rather got too cumbersome at the end ; So while the new API achieves all the expectations in the proposal, but a new API cannot be rolled out, until and unless the code is well tested.
* **Writing documentation** — While the new API supposedly adds a few hooks on scrapy, and providing the async/await support as an additional feature, but still writing documentation and some test examples, is mandatory for the common user to use them.
* **Supporting Python2.7** — The Scrapy codebase is backwards compatible, and most of the code is backwards compatible ; but there are some hooks and additional features which are only possible in Python ≥3.7. It should check the version of Python, and use the appropriate methods accordingly.
* **Twisted** — While this is a reason that is completely out of my hand, but Twisted has a bug, in which it has some variables defined as async. Starting from Python 3.7, we have async/await as a reserved keyword. This bug is resolved in Twisted’s github page, but before the corrected release, one has to wait in order to try them. You can clone them in your local computer from here.

# What does it means for the Scrapy users ?
After this new API is merged into Scrapy, users would be able to take advantage of new syntaxes async/await and run libraries requiring asyncio. The new API also provides the users to get the response in the same line, rather than using a method for receiving the response as a callback.

# Learning through the project
The project itself has been quite challenging, but that is the real beauty of Google Summer of Code. I have been quite used to with the frameworks that Scrapy has used, namely Twisted but regarding Asyncio, it has been quite new, and the evolving nature of asyncio would mean that the developers would certainly have a hard time catching with it.

Right at the start of applying for the project, I learnt Django and Flask, but going through Scrapy made me look at it with awe, as concurrency along with single threaded nature of Twisted certainly pushed me to learn this framework. Regarding Asyncio, I was playing with it through, and after reading through quite a lot of blogs, I certainly understood that it would take a fair bit of time before asyncio gets used as a mainstream event driven network framework.

There have been moments, where I chalked out the flag posts for the project, but there have been few moments where I certainly went away from the deadline. But I had allotted time considering the complexity of the project, so I did cover up most of the project within the stipulated time.

I also covered a lot of practical knowledge of generators and asynchronous generators in Python, and after a fair bit of use cases of them, I feel quite content in using them in future requirements.

# Important Links
* Asyncio support in Scrapy — [The PR for the project](https://github.com/scrapy/scrapy/pull/3362)
* My Blogpost — (https://yashrsharma44.github.io/)
* Medium page for the Blogs — (https://medium.com/@yashrsharma44/list-of-blogs-for-google-summer-of-code-18-674ddf91a34d)
---
layout: post
title: Completion of second task
---
This Blogpost deals with the progress during the couple of weeks, and the near completion of the first task, that was discussed with the mentors.

## Supporting `response = await scrapy.Request(...)`

While in the previous few weeks, I made a lot of progress, I was able to support inline-callbacks in the scrapy spiders. This meant that we could yield the response in the same line, without needing the use of a callback, so it was a good start.

> Starting with the idea for the same using native coroutines

I had discussed with my mentors, about the possible loopholes, and problems, that I might face, so it was good that I discussed the probable solutions there itself.

Supporting `response = await scrapy.Request(..)` required the scrapy Request object to be scheduled, which it does not have in the architecture, so scheduling it would require either - 1. Designing out the reference of Request object, which would be linked with the crawler object and, 2. Using context variables, which would provide a link to the Request object in a particular context.
Both of them turned out to be quite extravagant, considering the fact that designing out the reference would require another refactoring of scrapy codebase, which would not be backwards compatible, and the other requires `context-variable` which is actually supported in Python 3.7
So an alternative was clearly required in this case to support the above

> Supporting `response = yield scrapy.Request`

This was another alternative, which was discussed, so I started to implement the above.
This required the scrapy codebase to support asynchronous generators, as we would support the same paradigm, using async/await syntax. I had to refactor `scrapy.scraper`, `scrapy.engine`, in order to extend the support, that had been done earlier.

I had to read a lot of articles, particularly [Coroutines](https://mdk.fr/blog/python-coroutines-with-async-and-await.html) and [Twisted Cooperator](https://jcalderone.livejournal.com/24285.html).

While the first article served as a refresher for the native coroutines, the second one was useful, after I was stuck in refactoring a method in `scrapy.misc` named `parallel`. This method used synchronous iterator, while I was required to support asynchronous iteration. Thus, it took me a lot of time to get through the task, so it was fairly challenging as well.

> Working prototype

It took me few days to understand and then implement the code, so it was fairly challenging as well for me. The code now supports the `async def parse`, so it is safe to say that our implementation has started. It has been one month, and we have lots of ground to cover, but a progress like above does not harm us in any manner.

I would post another blog post which would implement as to how to use the new syntax (though it can safely be considered in its beta stage), and the utility, so stay tuned for updates.
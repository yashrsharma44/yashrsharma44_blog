---
layout: post
title: Using Asyncio in Twisted
---
This Blogpost deals with supporting Asyncio in Twisted.

## Update of the work

While I made it clear,that I would be working on wrting tests and supporting asyncio based frameworks, I managed to write tests for the API supporting `response = yield scrapy.Request(url)`. For the tests of native coroutines, I am still working on it, as there are not a lot of libraries available to support writing tests using `async/await`.

I also worked on learning, how to support asyncio in twisted, and the remaining blogpost deals with that.

> **Asyncio and Twisted**

Right from the start, I was excited in using asyncio,and rendering support to scrapy. Indeed, it is an exciting framework by Python itself, and with the advent of native coroutines, it is a different experience in using asyncio for supporting asynchronous programming. While, there have been other frameworks too to help us for async-programming, asyncio has been quite new, to be processed and rendered as a rugged framework to be used. Twisted, on the other hand has been quite an experienced ordeal, and it is a framework, which has been used a lot, so we find a lot of support with any sorts of problem, that we might face.

On a personal note, I had faced a lot of problems, in using asyncio, not because it is complex, but the support for different problems, that we might commonly face haven't been discussed yet, so one has to work on the problem, before asking for the community yet.

> **Using Twisted on asyncio**

There are two ways, in which asyncio can be used in Twisted. One is running Twisted on top of Asyncio,and other is running asyncio on top of Twisted. While, theoretically, both are possible, only one has been implemented uptil yet. Twisted is run over asyncio, so the current blog deals with that.

> **How I supported asyncio code in Twisted**

While, I went through a quite a bit of resources for supporting asyncio in Twisted. I even lurked in the irc of Twisted, and asked them various questions, sometimes, going through the answers previously discussed.

### Using Python3 and Twisted

Normally, Twisted had support for native coroutines(`async/await`), and we can write a coroutine, and then wrap them in `defer.ensureDeferred` to be used in Twisted.

```
from twisted.internet.task import react
from twisted.internet.defer import ensureDeferred
# our "real" main
async def _main(reactor):
    await some_deferred_returning_function()
# a wrapper that calls ensureDeferred
def main():
    return react(
        lambda reactor: ensureDeferred(
            _main(reactor)
        )
    )
if __name__ == '__main__':
    main()
```

Using this feature, we can write and await deferred returning functions. The drawback, is that we cannot await any awaitable coroutines ; only deferred can be awaited.

### Supporting asyncio frameworks in Twisted

Till now we discussed, that Twisted supports native coroutines, but all are useful only if we are dealing with deferreds. Till now, the native coroutines serve as a syntactic sugar, for writing methods dealing with deferreds.

But what happens, if we want to `await` other asyncio frameworks, or technically `asyncio future` ? 
Twisted has a solution for this, and we obtain by running twisted on top of asyncio. All the semantics remain the same, we just install twisted reactor on top asyncio. 

Make sure that you install `asyncioreactor` as early as possible, so that other reactor does not gets installed by default.

```
import asyncio
from twisted.internet import asyncioreactor
asyncioreactor.install(asyncio.get_event_loop())
```

### The trick of using asyncio frameworks in Twisted

The trick to use future objects in Twisted, is to wrap them in deferreds, and then use them as deferreds. We can also use the loop implementation of asyncio, and when we get the future result, we can wrap them into deferred. Twisted also allows interconversion of future and deferreds, so one can easily use them as native coroutines, without worrying about the interconversion between the two.

```
from twisted.inernet.defer.Deferred import asFuture, fromFuture

def as_future(d):
    return d.asFuture(asyncio.get_event_loop())
def as_deferred(f):
    return Deferred.fromFuture(asyncio.ensure_future(f))
```

> **What it means for Scrapy**

Using asyncio in scrapy remains the big target, as I am working to support the asyncio frameworks in Scrapy. Learning this new API from Twisted was fairly challenging, but more than that, I am happy that Twisted supports in utilising asyncio, otherwise it would have been as easily a tough job in supporting asyncio. Fairly, this is one of the most challenging phase of Gsoc for me uptil now, as this implementation hasn't been carried out anywhere, as far as I know( though I would love to get ideas about projects facing the same hurdle). I have completed a part of this, running up and supporting asyncioreactor, and phasing out (or rather) decoupling parts where it requires the implementation of asyncio. I have progressed fairly well, but I am quite behind the actual result. I am hopeful that in few weeks, this might be up and running.

## Links to some resources
 * [Using asyncio in Twisted](https://meejah.ca/blog/python3-twisted-and-asyncio)
 * [Asyncio reactor](https://twistedmatrix.com/documents/current/api/twisted.internet.asyncioreactor.html)
 * [Deferred to Futures](https://twistedmatrix.com/documents/current/api/twisted.internet.defer.Deferred.html#asFuture)



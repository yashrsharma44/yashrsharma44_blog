---
layout: post
title: Trials with asyncioreactor
---

This Blogpost deals with using asyncioreactor in Scrapy.

In the earlier post, I discussed in using asyncio in scrapy. While I have installed asyncio in scrapy, it is working well, and I am covering some problems that are occuring. I am working out day in and day out to cover the problems up, so when some trivial problems are covered, we would have time to polish up the project for public version.

## Using asyncioreactor in Scrapy

While I have discussed in using asyncioreactor in scrapy, this part would discuss my experiences in using asyncioreactor in scrapy. While Twisted supports in running asyncio, I have used the above support in Scrapy. We are running the asynchronous generators, in which each `asend` objects are awaited, and wrapped in `asyncio.ensure_future()`, and scheduled to run. This makes running and using asyncio frameworks, and we can support them as well.
Regarding the working, there was an issue that I was facing. The ones who use `scrapy` they know that the spider closes, if it is found idle. While this is normal, considering the fact that I want to await some awaitable. It is expected that the lag time would be covered up by other task, but if we have only one awaitable, then this issue is created. I am currently working on it, and hope that this issue is resolved in a few days of time.

## Some snippets in using the new framework

While I discussed that there is one recurring issue that is plaguing the progress, the new support is quite ready to be used. Below is a spider snippet that can be used - 

```
import scrapy

class QuotesSpider(scrapy.Spider):
    name = "quotes"

    async def start_requests(self):
        urls = [
            'http://quotes.toscrape.com/page/1/',
            'http://quotes.toscrape.com/page/2/',
        ]

        for url in urls:
            yield scrapy.Request(url=url, callback=self.parse)

    async def parse(self, response):
        print("--------------------------IN PARSE-------------------------------------------------------")
        
        links = [(response.xpath('//@href').extract()[-1])]
        links.append(response.xpath('//@href').extract()[-2])
       for h1 in response.xpath('//h1').extract():
            yield {"title": h1}
        for link in links:
            res = yield scrapy.Request(url=link)#can use scrapy.Request(.., callback=self.parse2), so supports both the syntax
            await asyncio.sleep(3)# Sleeps for 3 seconds, but point being that we can await any awaitable, also using asyncio loop
            print("___RESPONSE_____________________________________________________________{!r}".format(res))
        print("---------------------------END OF PARSE------------------------------------------------")
    
    async def parse2(self, response):
        page = response.url.split("/")[-2]
        print("------------------------IN PARSE 2----------------------------")
        filename = 'File-%s.html' % page
        with open(filename, 'wb') as f:
            f.write(response.body)
        self.log('Saved file %s' % filename)
        yield
        print("----END OF PARSE2 ------------")
        
```
The spider would work, but it needs to be shut down manually, as I have switched off the closing of spider when it remains idle.

## Future tasks and goals

Considering the fact that after supporting asyncioreactor, we can support asyncio based frameworks. After this, I would discuss with my mentor, if we are looking forward to small enhancements, or work on polishing the remaining work. 
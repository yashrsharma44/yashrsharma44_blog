---
layout : post
title : Documentation for the project
---

This blog lists up the use case and details of using asyncio support in Scrapy

# Using new asyncio support in Scrapy

## Requirements of the installation
1. Use `Python3.7`.
2. Install asyncio,using `pip install asyncio`.
3. If you plan to use any asyncio framework, then install them also. For example, for using `aiohttp`,install `pip install aiohttp`.

## Installing the new Scrapy.
1. First clone the new repository from this [github repo](https://github.com/yashrsharma44/scrapy/tree/devel_parse).
2. Then create a virtual environment, to install the new version of scrapy locally `virtualenv --python=python3.7 venv`.
3. Run the command, `. venv/bin/activate`, to start the virtual environment.
4. Run the command to install Scrapy `python3 setup.py install`.
5. After installing Scrapy, you are good to go, using asyncio based Scrapy.

## Brief History of Scrapy
Scrapy has used callback based programming, so this GSOC project has aimed to support async/await based programming. This helps users in getting the response in the same line, or `await` Requests, and get the response in the same line, rather than assigning a callback for dealing with the response.
The project has ensured that users are still able to use callback based programming, so now Scrapy supports both the syntax.

## Using async/await in Scrapy
In Scrapy, we create a Spider class, where we start our scraping by initialising the urls in spider method `start_requests`. We create a new definition, `async def start_request(self)`, and in Python terms, it is an asynchronous generator. Right in previous versions of Python, we had generators which generated urls, this start_request is an asynchronous generator. We can have `awaitable` list of urls, from an external sources, but for simple case we have simple iteration of predefined urls, and yield them through `scrapy.Request(url, callback=self.parse)` . Here we can yield the `scrapy.Request`, and get the response there itself, but we also support callbacks, so we assigned one.
We can create a callback method, in this case `async def parse(self, response)`. This accepts the response, and we can deal with it accordingly.

## Using callbacks and `awaitable` Requests
Scrapy provides three ways to make a request, and receive the response. 
1. The old callback based request is supported, so one can assign a callback and receive a response.
2. Yielding `scrapy.Request` in a `async def` method. One can simply yield the result of request like, `response = yield scrapy.Request(...)`. Note that this is supported only in methods using async/await syntax.
3. Awaiting `scrapy.Fetch.Fetch` method. This method accepts a request, and awaits the response as and when it is available. One can write, `response = Fetch(scrapy.Request(...))`.

## Using awaitable methods
You can create an awaitable method, and create an object and await them. You can refer to Python [documentation](https://docs.python.org/3/whatsnew/3.5.html#pep-492-coroutines-with-async-and-await-syntax) for more details.

## Using asyncio frameworks
Users can easily use asyncio based frameworks, in Scrapy spider.
The following example demonstrates the above - 
```
import scrapy

from scrapy.Fetch import Fetch
import asyncio
import aiohttp

class QuotesSpider(scrapy.Spider):
	name = "quotes1"

	async def start_requests(self):
		urls=['http://quotes.toscrape.com/page/1/',
		'http://quotes.toscrape.com/page/2/']

		for url in urls:
			yield scrapy.Request(url=url, callback=self.parse)


	async def parse(self, response):
		
    		links = [(response.xpath('//@href').extract()[-1]),(response.xpath('//@href').extract()[-2])]

		for link in links:
			request = scrapy.Request(url=link)
			res = await Fetch(request) # Can use `yield request`
			await asyncio.sleep(2)
			
		print("Started the aiohttp module!!")
		conn = aiohttp.TCPConnector(verify_ssl=False)
		async with aiohttp.ClientSession(connector=conn) as session:
			html = await self.fetch(session,'https://en.wikipedia.com')
	    		print(html)
		print("Completed the aiohttp module!!")

	async def fetch(self, session, url):
		async with session.get(url) as response:
			return await response.text()

```
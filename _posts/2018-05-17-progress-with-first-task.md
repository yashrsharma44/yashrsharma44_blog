---
layout: post
title: Completion of first task
---
This Blogpost would describe my account for the Community Bonding Period and the completion of first small task for Gsoc.

## Community Bonding Period

During the **Community Bonding Period**, a lot of time went into planning, getting used to the codebase, and optimising the requirements, of the Proposal.
This post would describe the details regarding it.

> Going through the codebase

I have previously contributed into codebase, so contribution to a open source project, would be rather straightforward : Make changes to your patch, submit a PR, make changes as required by the reviewer, and voila, we have made a contribution! But coding for a serious proposal, requires fair bit of responsibility, from the developer's side, so getting used to with the codebase is a must.
I had earlier went through small portions of codebase, before community bonding, so I made a few notes, regarding the 'gotchas' of the project. The notes did came handy though, as I went through the codebase, figuring out the flow of code, and noting down the utility of methods and classes, and the high-level interface associated to it.
Going through the codebase becomes highly monotonous, so I undertook a task, that was discussed with my mentor during the week. So I went through the relevant methods, classes which would require refactoring, and it made my objective of understanding the codebase, quite interesting.

> Discussing the goals, with the mentor

I had a fairly good discussion with my mentor, about the short term objectives and goals, as well as the long term objective. The best part of Gsoc, is to divide our work into small manageable parts, so completion of each part, actually helps us to gauge the progress of the project. So we discussed the requirements, and decided that for the first short term requirement, I would have to support async/await idioms in Scrapy's inbuilt `parse` method. This was a fairly good chalenging work, so I discussed all the points, doubts with the mentor, and finalised the objective of goal.

> Going about the writing code

As I spent a lot of time, in planning and discussing the goals of the project, I did not write that much of a code, as I intended. However, I did work upon enabling asynchronous generator support in Scrapy. This is also backwards compatible, so we can use it with Python 2.7( though Python 2.7 does not support Asyncio). This PR, will be used in introducing async/await idioms in parse method of Scrapy. So I think, that this kicks off my community bonding period. I am hopeful, that I can code through much faster, though coding with planning, requires time.

> Tasks that I am currently working upon

I am currently working on supporting `response = await scrapy.Request(..)`, as this is the next task I planned with my mentor. I am currently looking through a similar Python package, `inline_requests`, and would be looking forward to discuss the above with my mentor. However, I am quite hopeful that I might make good progress this week, so looking forward to it.

## Links to the resources
* [PR that I submitted](https://github.com/yashrsharma44/scrapy/commits/develmaster)
* [inline_requests](https://scrapy-inline-requests.readthedocs.io/en/stable/)
---
layout: api
title:  "The Camdram API"
date:   2019-04-07 14:47:00 +0000
author: "George Bateman"
permalink: /api
---

Camdram supports an API so that other websites and applications can access our data. This allows societies to see the history of their shows, venues to get cast and crew lists, and so on.  This article, which is a work in progress, aims to document what's available to applications.

In short, Camdram offers machine-accessible endpoints, in JSON, XML and iCal format. Your application requests an API key and can then access these pages. Some are available without an API key, but this is only for testing purposes and manual viewing.

Historically Camdram's API has been undocumented, but this site aims to make the API documented and well-defined. It is currently a work in progress, but if you spot any issues or find a page unclear you are welcome to [file a bug](https://github.com/camdram/camdram.github.io/issues/new) against the camdram/camdram.github.io repository and we will try to improve it.

## Objects

* [Diary](/api/diary)
* [Show](/api/show)
* [Societies and venues](/api/organizations)
* [Performances](/api/performance)

## Other information

* [Camdram Markdown](/api/markdown)


## Available client libraries
You can access the Camdram API directly, but the following library is available for Ruby:

- **Ruby**: https://github.com/CHTJonas/camdram-ruby

_If you write a library yourself and would like it to appear here, let us know!_

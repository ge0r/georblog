---
title: "Find where an openQA module is running"
date: 2021-06-14T12:07:57+02:00
---

*There is never a bad excuse to write a web scraper in go.* <!--more-->

You might have heard of a tool called [openQA](http://open.qa/).  
If not, the best way to get how it works is with some hands on experience. I recommend [get started with openqa development](https://kalikiana.gitlab.io/post/2021-02-16-get-started-with-openqa-development/), it will throw you right in.  

Ok, now that we know what openQA does, let's hear a major complain for the tool, from the people who use it daily.  
> "There is no good way to find all the places where an openQA module is running."  
> ~ *some openQA test developer*

Yes, this is an important issue and I will explain why:
* A module is the smallest testing block in openQA, and it is where the code that does the testing resides.
* Lots of modules running together in sequence form a job.
* Anyone with access in an openQA instance can schedule a job. In the case of [openqa.opensuse.org](openqa.opensuse.org/) for example, this is anyone with an opensuse account. 

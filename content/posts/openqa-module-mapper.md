---
title: "Find where an openQA module is running"
date: 2021-06-14T12:07:57+02:00
---

*There is never a bad excuse to write a web scraper in go.* <!--more-->
---
## The problem

You might have heard of a tool called [openQA](http://open.qa/).  
If not, the best way to understand how it works is with some hands on experience. I recommend following [get started with openqa development](https://kalikiana.gitlab.io/post/2021-02-16-get-started-with-openqa-development/), it will throw you right in.  

Ok, now that we know what openQA does, let's hear a major hurdle from the people who use it daily:  
> "There is no good way to view all the places where an openQA module is running."  
> ~ *some openQA test developer*

This lack of visibility can turn into quite an issue. To really understand what the deal is, let's clarify some stuff first:
* A module is the smallest testing block in openQA, and it is where the code that does the testing resides.
* Modules running together in sequence form a job.
* Anyone with access to an openQA instance can schedule a job. In the case of [openqa.opensuse.org](openqa.opensuse.org/) for example, this is anyone with an opensuse account. 

This means that when an openQA test developer changes the code of a module, this could affect an undefined number of jobs, each of which could correspond to a different System Under Test. Some of these jobs might be running on a regular basis, and each could be spawning through a different not-so-obvious mechanism.  
So if the developer wants to verify that the newly changed module  

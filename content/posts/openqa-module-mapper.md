---
title: "Find where an openQA module is running"
date: 2021-06-14T12:07:57+02:00
---

*There is never a bad excuse to write a web scraper in Go.* <!--more-->
---
## The problem

You might have heard of a tool called [openQA](http://open.qa/).  
If not, the best way to understand how it works is with some hands on experience. I recommend following [get started with openqa development](https://kalikiana.gitlab.io/post/2021-02-16-get-started-with-openqa-development/), it will throw you right in.  

Ok, now that we know what openQA does, let's hear a major hurdle from the people who use it daily:  
> "There is no good way to view all the places where an openQA module has been scheduled."  
> ~ *some openQA test developer*

This lack of visibility can turn into quite an issue. To really understand what the deal is, let's clarify some stuff first:
* A module is the smallest testing block in openQA, and it is where the code that does the testing resides.
* Modules running together in sequence form a job.
* Anyone with access to an openQA instance can schedule a job. In the case of [openqa.opensuse.org](openqa.opensuse.org/) for example, this is anyone with an opensuse account. 

This means that when an openQA test developer changes the code of a module, this could affect an undefined number of jobs, that could be testing different systems. The developer's code might break one or more of these jobs, which can be running on a regular basis, being scheduled by a not-so-visible mechanism.  

All well-intentioned test developers will try to be proactive about potential issues caused by their code changes. This means that they will need to find, for a specific openQA instance, all the jobs that contain the module(s) affected by their code changes, and verify that no side effects arise.  
However, as you might have guessed, there is no good way to list all jobs that contain a specific module.  

---
## The solution

Enter the [openqa-module-mapper](https://github.com/ge0r/openQA-module-mapper).  
*Shameless plug*: It is a web scrapper written in Go by [Anna Minou](https://github.com/punkioudi) and [George Gkioulis](https://github.com/ge0r) for SUSE Hack Week 2021.

The scraper parses daily [openqa.opensuse.org](openqa.opensuse.org) (abbreviated *o3*), visiting links that a user would, and mapping the jobs included in the last build of selected Job Groups. This allows the test developer to have an aggregated daily overview of which modules run in which jobs and whether they failed or not.  
Today's results can be seen in the o3 [output log](https://raw.githubusercontent.com/ge0r/openQA-module-mapper/o3/o3.log).

Right now usage is pretty basic.  
Let's say you want to change the `tests/containers/docker_runc` module:
* First step is to search in today's [o3 output log](https://raw.githubusercontent.com/ge0r/openQA-module-mapper/o3/o3.log) for that module. You now know which jobs you might need to clone for your verification runs.
* You get an extensive overview of which products and flavors your test module is running against. 
* You have visibility on which `YAML_SCHEDULE` file is used for each job, in case you wish to modify the schedule (and want to know which jobs will be affected).

---
## The implementation

The [project repository](https://github.com/ge0r/openQA-module-mapper) is open for everyone interested, PRs and feature requests are welcome!  
We wrote *openqa-module-mapper* solely in Go, using [goquery](https://github.com/PuerkitoBio/goquery), which brings jQuery functionality to Go. 
The project is in essence a web scrapper that gathers job and module information from a given openQA instance.  

Currently the o3 scraper runs once every night based on this [github actions workflow](https://github.com/ge0r/openQA-module-mapper/blob/o3/.github/workflows/o3-schedule.yaml).  
As mentioned, the o3 daily logs can be found in the o3 branch, [here](https://raw.githubusercontent.com/ge0r/openQA-module-mapper/o3/o3.log).  
One can also `git diff` the o3.log between different dates in order to see how the results change, what was scheduled and what was unscheduled.

---
## Future improvements

The following are in our todo list:
* Store the required data in a time series database instead of versioning the logs in git.
* If the latest build still has unfinished jobs, move to the next build. Right now this is mitigated by the scraper's timetable.
* Implement a CLI interface for the *openqa-module-mapper* tool.
* Pass a configuration file to specify which Job Groups to whitelist or blacklist, and other user defined specifications.

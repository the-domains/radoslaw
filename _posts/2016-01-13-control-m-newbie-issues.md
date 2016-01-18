---
inFeed: true
hasPage: true
inNav: false
inLanguage: null
starred: false
keywords: []
description: Issues with Control M I have faced during automatization of Hadoop cluster
datePublished: '2016-01-18T11:06:48.798Z'
dateModified: '2016-01-18T11:06:48.626Z'
title: Control M newbie issues
author: []
sourcePath: _posts/2016-01-13-control-m-newbie-issues.md
published: true
authors: []
publisher:
  name: null
  domain: null
  url: null
  favicon: null
url: control-m-newbie-issues/index.html
_type: Article

---
# Control M newbie issues
![](https://the-grid-user-content.s3-us-west-2.amazonaws.com/ffac3a4e-1383-4ea5-801b-85b0af655de7.png)

I'm lazy person - I try to automate tasks as much as possible.

## Why

For a long time some of the ETL workflows were written in Oozie. In new cluster I decided to move automation from Oozie into BMC Control-M. Reason to that is Hadoop is just a small part in our company and maintaining many scheduling systems is a huge issue. 

Control-M offers the pretty good tool which helps with Oozie workflow/coordinator import but still there is a lot of work to do after that. 

As I didn't have prior knowledge about Control-M I used 'F1 help' a lot and I faced few beginner issues.

#### 

## System variables and their format 

I've configured a job that run reports with parameters set to yesterday's date. Control-M supports that by system variables like: %%ODATE which has a value of current order date. The problem is that this variable has yymmdd format. My task preferred yyyymmdd format. I've found out that there is**%%$ODATE** which has yyyymmdd format. Great!

## Yesterday's date - whitespaces are important!

The second issue is to calculate a date. There is **%%CALCDATE** function which supports yymmdd format, and, of course, there is **%%$CALCDATE** for full year format.

There is the small catch, whitespaces. I wasn't aware why "**%%$CALCDATE %%$ODATE - 1**" returns "**CTMERR 20150901 - 1**". Searching and reading docs couldn't give any useful hint, but one thing was common - minus was very close to 1\. Long story short - "**%%$CALCDATE %%$ODATE -1**" is the proper one. 

Documentation specifies specifies format: 
> 
> result=%%CALCDATE date +|-quantity.

#### 

## Sqoop

Sqoop configuration is a huge pain, especially when password and jdbc uri has special characters. It may happen that Control-m fail on testing configuration but will later work properly when importing data.
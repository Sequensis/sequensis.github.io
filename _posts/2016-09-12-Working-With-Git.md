---
layout: post
title:  "Working With Git"
date:   2016-09-12 11:26:48 +0100
categories: Source-Control
---

## The Master Branching

The Master branch always contains production ready code. 

Before a feature branch is mastered:

* A pull request should be submitted and approved by other devs. The number of approves required is down to the authors discression.
* The branch should be tested on the Dev environment. The author should not test their own code.
* If possible the feature should be demonstrated to the product owner and approved by them.


## Branching

When beginning work on a new task will create a branch from master in the repository of the component we are working in. The branch will be named after the JIRA code for the task being worked on. 
Here is an example of a new branch being created for task ```CEX-123```

![branching](/images/branching.gif)

## Branching off branches

We try our best not to branch off branches, however sometimes it is unavoidable. If you end up with a lot of merge conflicts it is suggested to pair with someone else while you resolve them.
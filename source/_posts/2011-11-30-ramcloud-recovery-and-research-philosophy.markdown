---
layout: post
title: "RAMCloud Recovery and Research Philosophy"
date: 2011-11-30 22:58
comments: true
categories: 
 - academic
 - storage
 - research
---

[rc]: https://ramcloud.stanford.edu/wiki/display/ramcloud/Home

In the YouTube video below John Ousterhout talks at LinkedIn about the
RAMCloud project and the recovery mechanism that it uses. The
[RAMCloud project][rc] is building a scalable, durable, entirely in-memory data store.
Data in RAMCloud is served from a single in-memory copy. Since only one copy
exists in memory (stable storage holds backups) a server crash means there is
no opporunitiy for fail-over. To avoid unavailability of data for long
periods, the goal is to recover fast using many spindles within the cluster,
aiming for 1-2 second recovery times that appear only as a delay to any
application.  Recovery in RAMCloud is fascinating!

<iframe width="560" height="315"
src="http://www.youtube.com/embed/lcUvU3b5co8" frameborder="0"
allowfullscreen></iframe>

I was also thrilled to hear that the RAMCloud research group Ousterhout is
running at Stanford is aiming to produce production quality code. In the video
John explains that it is an awful waste of time to produce code for research
papers, only to toss everything to the side afterwards. I agree entirely, and
would even make a stronger statement that aiming for high-quality open code
the way RAMCloud is produces better research and better students. If I ever
find myself running a lab in the future, I know code quality and open review
will be integral part of our research workflow.

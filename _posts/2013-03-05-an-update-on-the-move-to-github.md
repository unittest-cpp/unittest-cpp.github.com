---
layout: post
title: "An Update on the Move to GitHub"
description: ""
category: ""
tags: [github-migration, v1.4, v1.5]
---

### First, an introduction

My name is Patrick Johnmeyer, and I have been a user of UnitTest++ in my day job and side projects for the past 5 years. You may have seen some activity from me on the SourceForge mailing list or the SourceForge / Google Code project pages in the past. I first discovered UnitTest++ when my team was evaluating C++ test frameworks in 2007, and we chose it for its ease of use, and its speed at both build-time and run-time. It was with great pleasure, then, that I accepted a role as maintainer in December 2012.

But enough about me, let's get to the meat of things.

### What is behind us

If you go to the [milestone 1.5.0 issue list](https://github.com/unittest-cpp/unittest-cpp/issues?labels=&milestone=1&page=1), you can see what has been finished and what is still open. The goal of milestone 1.5.0 is to produce a _nearly_ drop-in experience for those updating from 1.4 or from Google Code, while bringing the two code-bases together. I say nearly because the two development lines are incompatible in one regard, namely the locations and names of certain files.

There are a handful of long-standing bugs, and a couple of enhancements that I felt it was high time to incorporate in the main code-base; these are mostly taken care of. The CMake and autobuild support you will find here are unique to the GitHub edition, and were contributed by @paleozogt and @qdii respectively.

### What is coming up

The primary tasks remaining before I am prepared to call the GitHub project "stable" include completing the rehosting and updating of all documentation to GitHub, and the testing of all project-supported environments. This is not to say that you cannot use UnitTest++ now -- far from it. All it really means is that a few things may still end up shifting around in the near future. When this is all done, we will tag a version 1.5.0.

Also on the short-list is determining the right method for discussing UnitTest++. As of now the SourceForge mailing list or the GitHub issues remain the place to have discussion, but I would expect the mailing list to be deprecated at some point.

### What is around the bend

Once 1.5.0 is lying flat, our near-term goals will be as follows:

* **Create an add-on model**: Now that UnitTest++ is an 'organization' on GitHub, there exists a nice central landing place for additional repositories that expand on the UnitTest++ core. Users have been asking for a 'contrib' area for a long while, and now we have it. Obviously people are free to publish their extras whereever they like, but if we do this right this will be where they _want_ to publish them.

* **Gather the sheep back to the fold**: There are several "forks" of UnitTest++ all about the internet, including on GitHub (not forked from the now-official GitHub repo, mind you) and BitBucket. It is my intent to reach out to as many of these people as I can track down to see what we can do to meet their needs, and get their contributions back into the main project (or an add-on).

* **StackOverflow sweep**: With StackOverflow being [the source of half of development documentation according to one study](https://twitter.com/codinghorror/status/308797620988567552), it stands to reason that double-checking the accuracy of content regarding UnitTest++ is of value.

### What *isn't* around the bend

Lest you be be concerned, rest assured that the core functionality of UnitTest++ will always remain light-weight and simple (as long as I have any say in the matter). The goals of simplicity, portability, speed, and small footprint remain the charter of this test framework.

`~-- pj --~`
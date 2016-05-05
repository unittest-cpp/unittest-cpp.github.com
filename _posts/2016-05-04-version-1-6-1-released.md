---
layout: post
title: "Version 1.6.1 Released"
description: ""
category:
tags: [v1.6]
---
{% include JB/setup %}

The April* release of UnitTest++ is complete. The only changes were to CMake and autotools
files, so there's not much to write about. The full list of [issues and pull requests are
here] [1]. Thanks to everybody who contributed.

I'm still working out a few kinks in a changeset that will hopefully further improve the build
and release process, and I look forward to posting that as a PR for feedback soon. For this
release, I manually uploaded the output of `make dist` as a release artifact, which you can
[download here] [2].

<sub>* And yes, I am aware it is May.</sub>


`~-- pj --~`

[1]: https://github.com/unittest-cpp/unittest-cpp/issues?q=milestone%3A1.6.1 "v1.6.1 Issues and Pulls"
[2]: https://github.com/unittest-cpp/unittest-cpp/releases/download/v1.6.1/unittest-cpp-1.6.1.tar.gz
---
layout: post
title: "Version 1.5 and Beyond"
description: ""
category: 
tags: [v1.5]
---
{% include JB/setup %}

This site has gone a long, long time without an update -- longer than I even thought. As
promised in [the announcement of version 1.5] [announcement], though, I'd like to publish a
quick project update.

## First, Version 1.5.0

Version 1.5.0 was a long time coming. It probably could have been made a long time ago.
Regardless, for the first time in years we now have a numbered release people can work against.
The library has stayed mostly the same, but it is now a amalgam of the SourceForge and Google
Code versions of the project. Some of the most significant changes are more about supporting
the project -- especially the CMake migration and the Appveyor / TravisCI configurations. There
is a lot more than this, of course, so check the [issue list for v.1.5.0] [issues] for a full
list. Thanks to everybody who contributed!

## Second, Version 1.5.1

Version 1.5.1 will be released in the next few days, incorporating anything that is done to this
point. There isn't much at the time of this writing, but that's okay, because starting in 2016
UnitTest++ will be released **at least once per month** if there are any changes to release.
Along those lines, the project will be adopting [Semantic Versioning] [semver] to hopefully
prevent any confusion regarding the more frequent releases.

## Third, What's To Come

A few months ago I gave a presentation to a local C++ user group about UnitTest++, and in it
I highlighted ten things that fall into my 'future plans' for UnitTest++. Half of these ideas
are technical changes, some of which are already in progress; the other half are more
'spiritual' improvements. I'll go into more detail on these in future posts, but for now keep
your eyes on the [issue tracker for discussions] [discussions] on these upcoming items.

`~-- pj --~`

[announcement]: https://github.com/unittest-cpp/unittest-cpp/issues/80 "v1.5.0 Announcement"
[issues]: https://github.com/unittest-cpp/unittest-cpp/issues?q=is%3Aissue+is%3Aclosed+milestone%3A1.5.0 "v1.5.0 Issues"
[semver]: http://semver.org/spec/v2.0.0.html "Semantic Versioning 2.0.0"
[discussions]: https://github.com/unittest-cpp/unittest-cpp/issues?q=is%3Aopen+is%3Aissue+label%3Adiscussion "Open Issues tagged 'Discussion'"

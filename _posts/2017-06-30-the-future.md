---
layout: post
title: "Beyond UnitTest++ 2.0"
description: ""
category:
tags: v2.0 v3.0
---

Back in January when I [released UnitTest++ 2.0] [1], I mentioned that I would
be putting thought into what I'd like to accomplish with UnitTest++ in 2017.
This period of ponderance has led to a lack of activity on the project, for
reasons which will become obvious if you choose to continue reading. If you
want the short version, rest assured: **UnitTest++ is here to stay**. If you
want the long version, buckle in:

## Is there still a need for UnitTest++?
There are many, many options for unit testing C++ code. As of this writing,
there are [76 options listed on Wikipedia] [2], and I have to imagine there
are dozens if not hundreds of others out there, lurking in some undiscovered
repository, or locked behind closed doors at some proprietary shop. Google
Test is clearly very popular, as is the new kid on the block, Catch. Boost
Test, CppUnit, CppUTest -- the list goes on and on. Clearly, _need_ is a
strong word to apply to any of these at this point. If UnitTest++ somehow
magically disappeared overnight, it would not generally be that hard to pick
another library and switch.

But, UnitTest++ clearly has an audience. With 284 stars on GitHub and 1,920
unique clones in the last two weeks, there is obviously a user-base of some
magnitude. Microsoft has used it in at least [one open source project] [3],
and [has contributed back] [4]. Given my personal first and second-hand
knowledge of corporate use, I suspect there are a lot more people using it
than are active online about it. Beyond this, there are things about
UnitTest++ that, in my experience, differentiate it from the others.

## What differentiates UnitTest++?
### Small API
When you are in test-writing mode, there is very little to think about on the
UnitTest++ side of the equation. The vast majority of the time, it is as
simple as opening up a `TEST` block, or maybe a `TEST_FIXTURE` block if you
use fixtures -- then calling your code and choosing from a _small_ set of
`CHECK` macros to assert the expected behavior. Small can be interpreted as
"feature-poor", but it can also be interpreted as "minimalist". When I am
coding, I want to be focusing the majority of my mental energy on the
production code, not which of the twenty-or-so assertions is the "right" one.

UnitTest++ takes up a smaller portion of the experienced user's mental equity,
and presents a less daunting set of choices to the new user. It is my goal to
keep it this way.

### Fast to compile, fast to run
[As previously mentioned] [5], in late 2015 I gave a presentation to a local
C++ user group about UnitTest++. As part of this presentation, I did some
performance measurements as compared to three other popular C++ unit testing
frameworks. Now, these numbers were gathered under a very specific set of
circumstances, and are now almost two years old, so I hesitate to draw too
great a conclusion from them. That said, as the number of test cases grew,
UnitTest++ continually outperformed the others in initial build time a
incremental build + run time. This latter one is the most important to me, as
it represents the length of the feedback loop. I would not know where to start
to link to the vast array of literature discussing the importance of short
feedback loops in software development.

Based on my semi-scientific observations, UnitTest++ is one of if not _the_
fastest unit testing framework for C++. I would like to quantify this by
creating a performance testing suite that compares it to other frameworks more
thoroughly, and on an ongoing basis, but I will admit that may be too large of
an effort to undertake by myself.

### Compatibility with aging/odd compilers
Most C++ developers that have been in the industry for any significant amount
of time have had the misfortune of dealing with old, non-conformant, or just
plain broken compilers. It simply is part of the business. Maybe you're
supporting some legacy software the company can't justify scrapping, but also
can't justify spending months migrating to a newer compiler. Maybe you're
stuck with an oddball compiler specific to one particular target platform.
Maybe you're just like me, trying to support as many aging compilers as
possible and need a testing framework that supports them as well. Or, maybe
you're just a masochist and enjoy using Microsoft Visual Studio 6.

None of these things -- well, except maybe that last one -- should exclude you
from writing unit tests if you so require. (And yes, I use Microsoft Visual
Studio 6's C++ compiler as one of my test targets.)

## What challenges face UnitTest++ going forward?
### Single maintainer
For the past 5+ years, I have been the primary maintainer of UnitTest++. For
the most part, this is fine -- UnitTest++ doesn't _need_ constant care and
feeding to work for people. Over half of the merged PRs since the project
moved to GitHub are from contributors other than myself. I am in no way
bearing all of the load. But, it is a load, and it competes with all of the
other things I like to do with my time.

Past attempts to find additional maintainers have not been successful.
Submission guidelines and additional automation would be helpful to reduce the
load, but it all of course takes time.

### Compatibility with aging/odd compilers
Ah, the classic "pro that is also a con". Simply put, this aspect of
UnitTest++ makes it much harder to maintain and extend. There are only so many
compilers I can test on -- be that automatically or manually -- to ensure
maximum compatibility. Sure, I can generally rely on the community to provide
pull requests for the Screwball C++ compiler, but I have no way to prevent
that from regressing a prior patch for the OddDuck compiler.

For new features, especially those supporting modern C++ features, things
become doubly complicated. Reading through and reasoning about code is harder
when complicated by pre-processor blocks, CMake and/or autotools compiler
feature detection, and the like. Not to mention, these things _aren't fun_ to
spend time on.

## So, what now?
Right now, I am primarily weighing two long-term options for the project.

1. Continue to maintain UnitTest++ 2.0 and beyond as before, striving for as
much backward compatibility with as many compilers as possible. This means
accepting that proper support of modern C++ constructs will continue to be
more challenging and time-consuming, and development / maintenance will remain
complicated.
2. Immediately place UnitTest++ 2.0 into maintenance mode and start work on a
revamped 3.x code-base. Bug-fix pull requests will still be accepted on v2,
but new feature development will be done only on 3.x and beyond. Compilers
will be required to have a certain as-yet-undetermined level of modern C++
feature support.

Before writing this piece, I found option two the most appealing. While it
does eliminate one of the differentiators for UnitTest++, it also eliminates
one of the challenges for ongoing maintenance. At the same time, v2 won't be
going anywhere, so it will still be there for those who need it. Focusing on
"simple and fast" gives the project a clear charter and measure of success for
the future. Reading it now, though, with my thoughts thoroughly organized and
documented, I am less sure.

Before I can make a decision, though, I need to do some research. So, in the
near term, I will:

* Resume periodic review and merge of _bug, compiler compatibility, and
documentation fixes_ to UnitTest++. New features are officially on hold for
now.
* Begin an in-depth analysis of the C++ unit testing landscape, to determine
if a modernized UnitTest++ has a place in the world.

I will avoid over-promising and say I _hope_ to have sufficient time and
energy to really get into this latter bullet point, and to post my findings
here.

If you have any feedback, questions, or comments on this blog post, please [post them here] [8] (requires GitHub login).

`~-- pj --~`

[1]: {% post_url 2017-01-13-version-2-0-0-released %} "UnitTest++ 2.0 Released"
[2]: https://en.wikipedia.org/w/index.php?title=List_of_unit_testing_frameworks&oldid=786872499#C.2B.2B "List of Unit Testing Frameworks -- C++"
[3]: https://github.com/Microsoft/cpprestsdk "Microsoft C++ REST SDK"
[4]: https://github.com/unittest-cpp/unittest-cpp/pull/78
[5]: {% post_url 2016-01-28-version-1-5-and-beyond %} "Version 1.5 & Beyond"
[6]: https://github.com/unittest-cpp/unittest-cpp/issues?utf8=%E2%9C%93&q=is%3Aissue%20label%3Adiscussion%20
[7]: https://stackoverflow.com/questions/tagged/unittest%2b%2b "UnitTest++ on StackOverflow"
[8]: https://github.com/unittest-cpp/unittest-cpp/issues/156 "Discuss this blog post"

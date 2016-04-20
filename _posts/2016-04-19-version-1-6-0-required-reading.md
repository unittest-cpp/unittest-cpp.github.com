---
layout: post
title: "Version 1.6.0 : REQUIRE'd Reading"
description: ""
category:
tags: [v1.6]
---
{% include JB/setup %}

At the end of February, I tagged version 1.6.0 of UnitTest++. You can see
[everything this release included here] [1]. I also posted my intent to create a blog post
"later in the week". One of these days I'm going to learn not to put timelines on these things.

_Sigh._

That said, let's dive in and see what version 1.6 has to offer.

## New feature: The `REQUIRE` Macro

The biggest change in this latest version, and the reason for the minor version change, is the
new `REQUIRE` macro. The purpose of this feature is to allow short-circuit or "early-exit" test
failures.

Other C++ testing frameworks have analagous functionality. Google Test achieves this by defining
an `ASSERT` macro for every `EXPECT` macro (21 of them at the time of this writing). Boost Test
also does this, defining a `BOOST_REQUIRE` macro for every `BOOST_CHECK` macro (18 of them in
version 1.60). Catch, too, mimics this approach, although its interface is thankfully much
smaller.

UnitTest++ implements this feature for all existing `CHECK` macros with a single new helper:
`REQUIRE`.

The documentation is [on the wiki] [2], but let's discuss further.

By default, UnitTest++ runs all tests to completion, even after a check fails. Some test
libraries in some languages stop on the first failure for each test, but sometimes -- especially
if you consider a large suite of tests running on a build server -- the additional information
from multiple failure messages can be very helpful in quickly ascertaining the issue.

Let's say, for example, that you're using UnitTest++ to test a function that converts a vector
of strings from a data file into a vector of polygon objects. Now, you may start with some
simple cases that test each individual possible shape, one at a time, but you're probably going
to want at least one test that verifies a larger data set. We could test using `type_info` or
`dynamic_cast` to ensure the right types are coming back, but I'd rather use a real property of
the objects, say the number of sides.

    TEST(MultilineInput)
    {
       std::vector<std::string> input = {"rectangle", "rectangel", "rectangle", "triangle",
          "octogon", "pentagon", "rectangle"};

       // Please forgive my bare pointers
       std::vector<Polygon*> output = stringsToPolygons(input);

       CHECK_EQUAL(4, output[0]->sides());
       CHECK_EQUAL(4, output[1]->sides());
       CHECK_EQUAL(4, output[2]->sides());
       CHECK_EQUAL(3, output[3]->sides());
       CHECK_EQUAL(8, output[4]->sides());
       CHECK_EQUAL(5, output[5]->sides());
       CHECK_EQUAL(4, output[6]->sides());
    }

You run the test and you get a lot of output:

    /path/to/tests/PolygonReader.cpp:96:1: error: Failure in MultilineInput: Expected 4 but was 3
    /path/to/tests/PolygonReader.cpp:97:1: error: Failure in MultilineInput: Expected 3 but was 8
    /path/to/tests/PolygonReader.cpp:98:1: error: Failure in MultilineInput: Expected 8 but was 5
    /path/to/tests/PolygonReader.cpp:99:1: error: Failure in MultilineInput: Expected 5 but was 4
    /path/to/tests/PolygonReader.cpp:84:1: error: Failure in MultilineInput: Unhandled exception: test crashed

So you do a little more poking around, maybe step through the test in the debugger, and
eventually realize that `output[6]` is out of bounds because you typed `rectangel` for the
second input. (Apparently this function is ignoring invalid inputs -- that may not be the best
implementation, but you're okay with that for now.)

You fix your typo and the test passes, but you're a kind and generous developer and want to make
sure this test reports a better error message if it ever again fails to produce enough output.
So you put the typo back temporarily, and start by putting a size check before the value checks.

    CHECK(input.size() == output.size());

That helps clarify what is going on, but if a similar problem were to recur you'd now have _six_
errors instead of five, and that might be more than a person needs to know in this situation.

### Before REQUIRE

I've seen developers tackle this in several ways using UnitTest++ before. One is to replace the
CHECK with preconditions and throw:

    if(input.size() != output.size())
    {
       throw std::runtime_error("Input size does not match output size, aborting");
    }

    CHECK_EQUAL(4, output[0]->sides());
    // ...

This works well when there's only one precondition in your test, and generates good output. If
there are multiple conditions, however, this can quickly become tedious. Another variant is
similar, breaking the case into two code paths instead:

    if(input.size() != output.size())
    {
       CHECK(input.size() == output.size()); // or sometimes just CHECK(false)
    }
    else
    {
       CHECK_EQUAL(4, output[0]->sides());
       // ...
    }

Most people find this unintuitive. Why would I branch on what I'm checking in a test case? Why
am I repeating the if condition in the body as a `CHECK`?

Finally, I've seen more than one project define their own `REQUIRE` or `ASSERT` macros.

### After REQUIRE

The `REQUIRE` macro cleans up these arguably less pleasant solutions by allowing you to mark one
or more existing `CHECK` statements as test-ending failures. For our example, we go back to
checking the size of the input and output, but represent it as either a one-liner:

    REQUIRE CHECK(input.size() == output.size());

    CHECK_EQUAL(4, output[0]->sides());
    // ...

or in a scope:

    REQUIRE
    {
       CHECK(input.size() == output.size());
       // could have more checks inside this scope, they'll be required as well
    }

    CHECK_EQUAL(4, output[0]->sides());
    // ...

If the size check fails, the test will terminate immediately, reporting only that one error
before moving on to the next test.

## Miscellany

Version 1.6.0 also sees us upping our game in terms of compiler support on AppVeyor. Visual
Studio 2008 and up are all included in the build config. It is always my goal to support as many
compilers as possible; not everybody out there in the corporate world gets to use the latest
and greatest whenever they like (I've been there), and it doesn't seem to me they should have
to go hunting for a specific version, or miss out on new developments.

This is also part of shoring up our concept of "releases", which has admittedly been a bit
lacking. Thanks to your contributions, there are already a few improvements to CMake and
autotools support incorporated on master that will be released in 1.6.1 later this month.
Ideally we'll get a few more things worked out in the months ahead.

`~-- pj --~`

[1]: https://github.com/unittest-cpp/unittest-cpp/issues?q=milestone%3A1.6.0 "v1.6.0 Issues and Pulls"
[2]: https://github.com/unittest-cpp/unittest-cpp/wiki/Macro-Reference#miscellaneous

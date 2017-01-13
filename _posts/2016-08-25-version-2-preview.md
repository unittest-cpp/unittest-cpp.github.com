---
layout: post
title: "UnitTest++ 2.0 Preview"
description: ""
category:
tags: v2.0 site-update
---

> As you can likely see, I've freshened up the site a bit. The versions of Jekyll and Jekyll
> Bootstrap I had been using since 2013 were a bit long in the tooth, so it was time for a
> clean(ish) start. I won't bore you with the details, but I hope you find it more pleasant. If you
> notice any problems, feel free to [file an issue] [1].

[Back in January] [2] I announced version 1.5 of UnitTest++, and with that the adoption of Semantic
Versioning. Since then, there have been three more releases, and while this falls a bit short of the
"once per month" rate I had planned, I'm happy that we've had four releases in 2016, soon to be
five. This fifth release, however, introduces our first potentially breaking changes, so I wanted to
post a bit more about this in advance -- especially since the breakage is rather subtle.

## CHECK 1, 2

The `CHECK` macro is the most basic of checks, asserting simple "truthiness" of the provided
expression. Ever since the migration to GitHub, however, there has been [an open issue] [3] to
change the signature of this method from pass-by-value to pass-by-reference. While the obvious
impetus -- avoiding unnecessary copies -- has always been there, this was not a strong motivation
for change. However, [a recent issue] [4] pointed out the real problem with this -- copies with
side-effects can trigger an unexpected failure.

Now, I for one do my best to avoid side-effects in copy constructors, and in the case of [this
particular code] [5] the object probably shouldn't even _be_ copyable. However,
a test library should not be making assumptions of, or attempting to enforce particular practices in,
the code under test. Rather, this was an excellent driver to finally make the breaking change.

The simplest example I could come up with to expose the issue looks like this:

{% highlight cpp %}

    class TruthyUnlessCopied
    {
    public:
       TruthyUnlessCopied()
       : truthy_(true) { }

       TruthyUnlessCopied(const TruthyUnlessCopied&)
       : truthy_(false) { }

       operator bool() const { return truthy_; }

    private:
       bool truthy_;
    };

    TEST(CheckProperlyDealsWithOperatorBoolOverrides)
    {
       TruthyUnlessCopied objectThatShouldBeTruthy;
       CHECK(objectThatShouldBeTruthy);
    }

{% endhighlight %}
[ [source] ] [6]

The prior implementation of `CHECK` would fail here because the default-constructed,
truthy object passed to `UnitTest::Check` would be copied; the new implementation does not exhibit
this behavior. However, if anybody out there has tests that are reliant on this behavior --
intentionally or not -- this could cause failures where none existed.

The changes are available in master for you to test against today.

## Updates to macro names

[Pull 114] [7] attempts to introduce some consistency and flexibility to the macro names that make
up the majority of the UnitTest++ public interface. However, since it was never clearly stated
which macros _make up_ the public interface, I consider this a breaking change -- although it
should not break most users.

Starting with version 2.0, all macros that are considered part of the public interface are of the
form `UNITTEST_<SHORTNAME>`, and those that are not are of the form `UNITTEST_IMPL_<NAME>`. The
`<SHORTNAME>` forms of the public interface of course remain, but they can now be shut off to
hopefully avoid name collisions with other macros in your project. See [the pull request] [7] for
full details of the changes.

These changes are also available in master for you to test against.

`~-- pj --~`

[1]: https://github.com/unittest-cpp/unittest-cpp.github.com/issues/new "Report site issue"
[2]: {% post_url 2016-01-28-version-1-5-and-beyond %} "Version 1.5 and Beyond"
[3]: https://github.com/unittest-cpp/unittest-cpp/issues/7 "UnitTest++ Issue 7"
[4]: https://github.com/unittest-cpp/unittest-cpp/issues/119 "UnitTest++ Issue 119"
[5]: http://stackoverflow.com/questions/38330348/continuous-error-21-library-routine-called-out-of-sequence
[6]: https://github.com/unittest-cpp/unittest-cpp/blob/de915185f6f40a3c25bef51eecddab22c40d045d/tests/TestChecks.cpp#L318-L338 "Example"
[7]: https://github.com/unittest-cpp/unittest-cpp/pull/114

# Developer Guide

## Building, Testing, and Running

You can build .NET Core either via the command line or by using Visual Studio.
We currently only support building and running on Windows. Other platforms will
come later.

The command line build is invoked via

```
build.cmd
```

In order to build successfully, you must have Visual Studio 2013 or higher
installed.

### Running Tests

We use the OSS testing framework [xUnit.net][xunit].

Running the tests from the command line is as simple as invoking `build.cmd`.

You can also run the tests from within Visual Studio. See [this page][xunit-runner]
for more details on how to install the required test runner and how you can
invoke the tests using Test Explorer.

[xunit]: http://xunit.github.io/
[xunit-runner]: https://xunit.codeplex.com/wikipage?title=HowToUseVs2012
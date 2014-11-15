## General .NET Contributing Guide
General information on contributing to dotnet projects is in the [Contributing Guide]. 

This document contains information about coding styles, source structure, making
pull requests, and more.

[Contributing Guide]: https://github.com/Microsoft/dotnet/blob/master/CONTRIBUTING.md

## Additional Guidelines for contributions to dotnet/corefx

### Issues
You do not need to file an issue for trivial changes (e.g. typo fixes). Just send us
a PR if it's tiny.

If an issue is complex, consider filing it and giving us time to respond before sending a
corresponding PR. If you want to work on the issue, just let it be known on the issue thread. Giving
us a chance to review the issue will help save time. For example, we might let you know why existing
behavior can't be changed or about particular implementation constraints you need to keep in mind, etc.

Don't feel obliged to match every issue with a PR. Simply filing issues for problems you
encounter is a great way to contribute too! 

### Style
We are striving to bring dotnet/corefx in to full conformance with the style guidelines
described above.We have built some automation for that and will work to bring everything
in line. In the meantime, please

* **DON'T** send PRs for style changes. We'd rather handle them holistically and they are tough
to review and merge.

* **DO** give priority to the current style of the project or file you're changing even if it
diverges from the general guidelines.

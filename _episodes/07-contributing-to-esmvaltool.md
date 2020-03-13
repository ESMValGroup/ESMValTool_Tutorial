---
title: "Contributing to ESMValTool"
teaching: 0
exercises: 0
questions:
- "How do you contribute to ESMValTool"
objectives:
- "First learning objective. (FIXME)"
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---


## Contribution guide

The first place to look for instructions on how to contribute to ESMValTool
is in the [CONTRIBUTING.md](https://github.com/ESMValGroup/ESMValTool/blob/master/CONTRIBUTING.md)
file. It is recommended that you read the entirety of that document before
attempting to contribute to ESMValTool. This section is only a summary of those
instructions.


## Contributing steps

Firstly, you will need to have installed ESMValTool in development mode.
Step by step instructions are in 4 [CONTRIBUTING.md](https://github.com/ESMValGroup/ESMValTool/blob/master/CONTRIBUTING.md).

Secondly, you will need to work in your own branch.

```bash
git branch mybranch
git checkout mybranch
```
You should make your changes in that branch, then add, commit and push your
branch back to the ESMValTool github repository in the usual way:
```bash
git add myfile
git commit -m 'Added my file'
git push
```


## Code Standards

You code should adhere to the code standards required by ESMValTool,
and should not cause any of the unit or integration tests to fail in an
unexpected way.
There are more details for each language in the
[CONTRIBUTING.md](https://github.com/ESMValGroup/ESMValTool/blob/master/CONTRIBUTING.md)
file. In addition, this file also describes the process by which you
can check whether you code passes the code standards and tests.

To run the test, the command is:
```bash
python setup.py test
```

# New Issues and Pull  request

When you have a version that you are happy with and that passes all code standards
and units tests, please create an [new issue](https://github.com/ESMValGroup/ESMValTool/issues)
to describe your branch and a [new pull request](https://github.com/ESMValGroup/ESMValTool/pulls)  (PR)
on the [ESMValTool gihub page ](https://github.com/ESMValGroup/ESMValTool/).

Your PR will then be reviewed by an ESMValTool expert and they will decide what changes
(if any) are needed to your code before it is suitable to be merged into the
main ESMValTool master branch.

The ESMValCore repository follows a similar methodology for contributing in
the [ESMValCore CONTRIBUTING.md](https://github.com/ESMValGroup/ESMValCore/blob/master/CONTRIBUTING.md) guide.

{% include links.md %}

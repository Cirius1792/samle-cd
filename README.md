# Pre-Commit Example for Maven Project 
One of the most common causes of a pipeline failure is a Unit Test failure during the build stage. Noting strange, it happens, but it can be quite annoying for other colleagues if they just checked out the code while the build is broken.

It is always a good practice to be sure that the code builds and passes the Unit Tests before check-in, but hurry, tiredness or just forgetfulness can happen.

One easy solution can be to leverage a git hook to trigger build and tests before the commit. From the doc: "Git has a way to fire off custom scripts when certain important actions occur" [git documentation](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks)

But we can do much more thanks to [pre-commit](https://pre-commit.com/#intro). Pre-commit makes it easy to configure git hooks across different projects and platforms, supporting different kinds of projects in many programming languages, with out-of-the-box hooks that just need to be added to its config file.

Here we will see an example of pre-commit in action, configuring a maven hook that will trigger the test of our sample app before each commit.

## Configuration
Install pre-commit:
```
pip install pre-commit
```
(more info in the official documentation)

create a sample configuration file
```
pre-commit sample-config
```
Configure the maven hook:
```-   repo: https://github.com/ejba/pre-commit-maven
    rev: v0.3.3
    hooks:
    -   id: maven
        args: ['clean compile test -f ./example-reactive/pom.xml']
```
in this example, we added the -f options to point at the source folder, as they are not in the root.

After configuring pre-commit we can manually trigger all the hooks to test our configuration:
```
pre-commit run --all-files
```
From now on, at every commit attempt, our hook will trigger a maven build to catch any issue before they get into the trunk and prevent our pipeline to fail

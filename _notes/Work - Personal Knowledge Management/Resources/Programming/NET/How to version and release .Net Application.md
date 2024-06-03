---
title: How to version and release .Net Application
feed: show
---

[https://medium.com/@fran6_ca/how-to-version-and-release-net-application-e7b5811dfe4b](https://medium.com/@fran6_ca/how-to-version-and-release-net-application-e7b5811dfe4b)

Recently, a co-worker posed a question on how we can improve the versioning and release process of our dotnet applications. Currently, we version our applications by looking at the git commit logs and guessing the correct version number. Then we do a release doing a manual git tag, manual git branch, manual git pull requests, and GitHub actions. The whole process can be tedious, slow, and prone to human error.

I’ve been brainstorming about multiple ways to solve these problems. With over a decade experience in software development, i’ve seen so many ways of solving this. I want a solution that runs end-to-end, establish a good practice, automated and documented. This is what i come up with.

TL;DR:

1. How do we automate the process of **manual** git tag, git branch from main, git pull request, git merge, git tag again etc_Solution: use_ [_versionize_](https://github.com/versionize/versionize) _with_ [_github actions_](https://docs.github.com/en/actions)_. Versionize look at your git commit message to determine the version and automatically creates a tag for you_
2. Ok, so it looks at commit messages, how do we ensure developers use correct git commit messages ?_Solution: Use_ [_husky.net_](https://github.com/alirezanet/Husky.Net/tags) _to establish_ [_use conventional commits_](https://www.conventionalcommits.org/en/v1.0.0/) _and_ [_github actions_](https://docs.github.com/en/actions)
3. how does this work in practice?_Solution: read this article_

## T**echnical introduction**

[Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/): This is a practice where we generate a version of our application based on the commit message. e.g. a commit message starting with BREAKING CHANGE will result in a new major version being created for the application.

[Versionize](https://github.com/versionize/versionize): This is a library that uses conventional commits. It does the actual work of checking the commit messages, creating a new version, and updating the CHANGELOG.md

[Husky.net](https://github.com/alirezanet/Husky.Net/tags): This is a library that helps you enforce the convention of the commit message with the git repo. With this library, you can define the format, rules and behaviour of each commit message.

Github action: A declarative workflow script that allow you specify a series of actions and steps to complete a task. Usually used for CI/CD and validation.

## Walkthrough

Let’s quickly use these libraries and tools with a .net application.

1. We need a project that is already has a git repository. If you don’t have one, login to [github](https://github.com/new), create a new repository and clone it.

    Otherwise, navigate to the application folder using the terminal and skip to step 3.

2. Create a sample .net application using the terminal.

    Navigate to a folder or create a new one to hold this application code.

3. Open the csproj file and add version tag as shown below to the first property group.

    ```Plain
    <Project Sdk="Microsoft.NET.Sdk.Web">
      <PropertyGroup>
    <Version>1.0.0</Version>      <TargetFramework>net6.0</TargetFramework>

    </PropertyGroup>
    .
    .
    .
    ```

4. Install versionize on your machine to try it out

    ```Plain
    > dotnet tool install --global Versionize
    ```

5. At the root of your application where the .git folder exists, run the versionize command

```Plain
> versionize
```

You should be prompted to add and commit your changes. This is where conventional commits will come in but we will walkthrough that later

```Plain
> git add .
> git commit -m "added version"
```

Run the versionize again.

This will version the application, create a tag for the release and generate a changelog in a file CHANGELOG.md

Now let’s publish our changes

```Plain
git push --follow-tags origin main
OR
git push --follow-tags origin master <depends on your default branch name>
```

You will notice that this does not create a release for us. This is where github action come into play.

## Auto version and release via github action

We will make use of [github script](https://github.com/actions/github-script) in the next couple of steps to automatically create a release via github action. To do that, we need to put all the manual actions that we did earlier into a github action script.

Create a new folder called _**.github**_ under the source code directory. Make sure it is in the directory as _.git_ folder.

```Plain
> mkdir .github
> cd .github
> mkdir workflows
>cd workflows
> touch version-and-release.yml
```

Open up the version-and-release.yml and add this code

```Plain
name: Version and Release

on:
  push:
    branches: [ "main" ]
```

This specifies the name of our github action and its trigger. We only want to version and release when there is a push to the main branch.

Now let’s the task that will do versioning and release

```Plain
on:
  push:
    branches: [ "main" ]jobs:
  build:
    runs-on: ubuntu-latest
```

We are telling the git runtime to run our task(job) on ubuntu operating system using the latest version.

Now we have a system, let’s checkout our git repository.

```Plain
on:
  push:
    branches: [ "main" ]jobs:
  build:
    runs-on: ubuntu-latest

 steps:
    - uses: actions/checkout@v3
      with:
        fetch-depth: 0
```

We use fetch-depth of zero so that all our tags are check out as well.

Next, let’s install .Net on the ubuntu agent that will be running our github action.

```Plain
on:
  push:
    branches: [ "main" ]jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v3
      with:
        fetch-depth: 0

- name: Setup .NET
      uses: actions/setup-dotnet@v3
      with:
        dotnet-version: 6.0.x
```

Let’s build our application to make sure it compiles before releasing it.

```Plain
on:
  push:
    branches: [ "main" ]jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v3
      with:
        fetch-depth: 0

    - name: Setup .NET
      uses: actions/setup-dotnet@v3
      with:
        dotnet-version: 6.0.x

  - name: Build
      run: dotnet build
```

Now let’s install versionize

```Plain
on:
  push:
    branches: [ "main" ]jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v3
      with:
        fetch-depth: 0

    - name: Setup .NET
      uses: actions/setup-dotnet@v3
      with:
        dotnet-version: 6.0.x

    - name: Build
      run: dotnet build

  - name: Install Versionize
      run: dotnet tool install --global Versionize
```

Time to run it. Add the commands below

```Plain
on:
  push:
    branches: [ "main" ]jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v3
      with:
        fetch-depth: 0

    - name: Setup .NET
      uses: actions/setup-dotnet@v3
      with:
        dotnet-version: 6.0.x

    - name: Build
      run: dotnet build

    - name: Install Versionize
      run: dotnet tool install --global Versionize

 - name: Versioning
      id: versionize
      run: versionize --changelog-all --skip-dirty
      continue-on-error: true
```

The _-changelog-all_ flag ensures that all commit messages are included in the logs.

The versionize command returns a status result which indicates a success or failure. We willl check that before doing a release.

```Plain
on:
  push:
    branches: [ "main" ]jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v3
      with:
        fetch-depth: 0

    - name: Setup .NET
      uses: actions/setup-dotnet@v3
      with:
        dotnet-version: 6.0.x

    - name: Build
      run: dotnet build

    - name: Install Versionize
      run: dotnet tool install --global Versionize

    - name: Versioning
      id: versionize
      run: versionize --changelog-all --exit-insignificant-commits
      continue-on-error: true

 - name: No release required
      if: steps.versionize.outcome != 'success'
      run: echo "Skipping Release. No release required."
```

If successful, push the new version and changelog to github

```Plain
on:
  push:
    branches: [ "main" ]jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v3
      with:
        fetch-depth: 0

    - name: Setup .NET
      uses: actions/setup-dotnet@v3
      with:
        dotnet-version: 6.0.x

    - name: Build
      run: dotnet build

    - name: Install Versionize
      run: dotnet tool install --global Versionize

    - name: Versioning
      id: versionize
      run: versionize --changelog-all --exit-insignificant-commits
      continue-on-error: true

    - name: No release required
      if: steps.versionize.outcome != 'success'
      run: echo "Skipping Release. No release required."

- name: Setup git
      run: |
        git config --local user.email "<user@email.com>"
        git config --local user.name "<username>"

    - name: Push changes to GitHub
      if: steps.versionize.outcome == 'success'
      uses: ad-m/github-push-action@master
      with:
        github_token: ${{ secrets.GITHUB_TOKEN }}
        branch: ${{ github.ref }}
        tags: true
```

Make sure to replace <user@email.com> with a valid email and < username > with your a valid username. This is just for logging purposes.

Now it’s time to release. We will use the [github-script](https://github.com/actions/github-script) here to automatically create a release.

```Plain
on:
  push:
    branches: [ "main" ]jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v3
      with:
        fetch-depth: 0

    - name: Setup .NET
      uses: actions/setup-dotnet@v3
      with:
        dotnet-version: 6.0.x

    - name: Build
      run: dotnet build

    - name: Install Versionize
      run: dotnet tool install --global Versionize

    - name: Versioning
      id: versionize
      run: versionize --changelog-all --exit-insignificant-commits
      continue-on-error: true

    - name: No release required
      if: steps.versionize.outcome != 'success'
      run: echo "Skipping Release. No release required."

    - name: Setup git
      run: |
        git config --local user.email "<user@email.com>"
        git config --local user.name "<username>"

    - name: Push changes to GitHub
      if: steps.versionize.outcome == 'success'
      uses: ad-m/github-push-action@master
      with:
        github_token: ${{ secrets.GITHUB_TOKEN }}
        branch: ${{ github.ref }}
        tags: true

- name: "Create release"
      if: steps.versionize.outcome == 'success'
      uses: "actions/github-script@v5"
      with:
        github-token: "${{ secrets.GITHUB_TOKEN }}"
        script: |
          try {
            const tags_url = context.payload.repository.tags_url + "?per_page=1"
            const result = await github.request(tags_url)
            const current_tag = result.data[0].name
            await github.rest.repos.createRelease({
              draft: false,
              generate_release_notes: true,
              name: current_tag,
              owner: context.repo.owner,
              prerelease: false,
              repo: context.repo.repo,
              tag_name: current_tag,
            });
          } catch (error) {
            core.setFailed(error.message);
          }
```

Running this should create a release in github. Complete file can be found in my [github repo](https://github.com/francis04j/SampleDotNetVersioning/tree/main/.github/workflows)

## How do you control the version number?

Versionize uses [conventional commits](https://www.conventionalcommits.org/en/v1.0.0/) to determine the version of your release.

This is a runthrough of how it works

```Plain
> git add .
> git commit -m "fix: trim email addresses"
> versionize
> git push --follow-tags origin main
```

When you do a push, versionize will create a tag and update the application patch version. So, from 1.0.0 to 1.0.1.

Now let’s add a new major change, a breaking change

```Plain
> git add .
> git commit -m "feat!: trim email addresses"
> versionize
> git push --follow-tags origin main
```

When you do a push, versionize will create a tag and update the application patch version. So, from 1.0.1 to 2.0.0.

Now if you want to add a normal feature, you can just do

```Plain
> git add .
> git commit -m "feat: added email queueing"
> versionize
> git push --follow-tags origin main
```

When you do a push, versionize will create a tag and update the application patch version. So, from 2.0.0 to 2.0.1.

It is recommended that you commit working code to avoid the versioning a ill prepared code.

## How do you ensure commit messages follow the correct format?

Basically, how do we avoid commit messages like this.

This is where [husky.](https://github.com/alirezanet/Husky.Net)[net](https://github.com/alirezanet/Husky.Net) comes in. With husky, you can add [Git hooks](https://githooks.com/) that will run when a developer commit, push or pull from the project’s git repository.

Husky flow

Let’s see how Husky works. The following instructions use and referenced [husky.net tutorial](https://alirezanet.github.io/Husky.Net/guide/csharp-script.html#using-c-code-in-your-git-hooks). Please go to that [husky site](https://alirezanet.github.io/Husky.Net/) if any of these instruction is out-of-date or unclear.

## Setup husky for your project

```Plain
> cd <Your project root directory>
> dotnet new tool-manifest
> dotnet tool install Husky
> dotnet husky install
```

## Add your first hook

```Plain
dotnet husky add pre-commit -c "echo 'Welcome to our project'"
git add .husky/pre-commit
```

Now when you make a commit, you should see a message like below

```Plain
Welcome to our project
[main 91afb16] Testing husky.net
 1 file changed, 22 insertions(+)
 create mode 100644 .husky/pre-commit
```

Let’s extend this pre-commit hook to make sure developers follow the conventional commits pattern.

```Plain
> dotnet husky add commit-msg
```

This will add a **commit msg** hook to your project. You can find the hook in the `.husky` folder. In the `commit-msg` file, add the following commands to run before the commit is made.

update your `commit-msg` to match this

```Plain
echo
echo Great work!
echo
```

This will add a [commit message linter](https://alirezanet.github.io/Husky.Net/guide/csharp-script.html#simple-commit-message-linter) to lint the commit message. Husky.Net will run the commit message linter before the commit is made.

Create a new folder under the `.husky` folder and name it `csx` (csx stands for C# script). In the `csx` folder, create a new file and name it `commit-lint.csx`. In the `commit-lint.csx` file, add the following code.

```Plain
private var pattern = @"^(?=.{1,90}$)(?:feat|chore|docs|fix|perf|refactor|revert|style|test|wip)(?:\(.+\))*(?::).{4,}(?:#\d+)*(?<![\.\s])$";
```

Notice that we check for the prefix of _feat_, _chore_ and _fix,_ etc. You can edit this script to include more checks. This code is copied from husky.net tutorial [here](https://alirezanet.github.io/Husky.Net/guide/csharp-script.html#simple-commit-message-linter).

**Make sure you remove** _csx/_ **from the .gitignore file and push**

A common check to add is minimum message length to force developers to be as descriptive as possible in their commit message.

## Update husky task runner

Now we have our hooks ready. We need to update the task runner to run the hooks. Open the `task-runner.json` file located under the _.husky/_ folder and add the following tasks.

```Plain
{
   "tasks": [
     {
       "name": "commit-message-linter",
       "command": "husky",
       "args": [
         "exec",
         ".husky/csx/commit-lint.csx",
         "--args",
         "${args}"
       ]
     },
     {
       "name": "dotnet-format",
       "group": "pre-commit",
       "command": "dotnet-format",
       "args": ["--include", "${staged}"],
       "include": ["**/*.cs"]
     }
   ]
}
```

Commit and push your changes. You can find the commit message linter in action.

Husky warning for invalid commit message

Correct the commit message to use feat:, fix:, test: prefix

Working git hook

## Conclusion

In this article, we’ve seen how to automate the versioning and releasing of a dotnet application using versionize and GitHub actions. Using husky.net, we were able to establish a version number via conventional commits right from the developer machine.

I’m sure there are alternatives to doing this. A colleague mentioned [semantic release](https://github.com/semantic-release/semantic-release). I plan to try that out and blog about my experience.

If you have any questions or suggestions, please leave a comment below.

Have a good day!

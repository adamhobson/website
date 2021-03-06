---
layout: post
title:  "Command Spotlight: volta run"
date:   2020-11-25 08:00:00 -0700
categories: [spotlight, commands]
author: Chuck Pierce
excerpt_separator: <!--more-->
---

Volta is designed to be invisible: You type a command like `npm start`, and Volta automatically detects and runs the right versions of npm and Node. While that works great most of the time, occasionally you need direct control over the tool versions that are used. Maybe you are trying to reproduce a reported bug or testing how your project works with a different npm version. Whatever the reason, `volta run` lets you **take explicit control of tool selection** right from the command line.
<!--more-->

Let's check out some examples of how you can use `volta run`:

```
volta run --node lts --npm 7 npm start
```

This will run the command `npm start` using the most recent LTS Node version and major version 7 of npm.

```
volta run --yarn 1.22.1 yarn add typescript
```

This will run `yarn add typescript` using Yarn 1.22.1 and whichever Node version Volta would normally find (probably from your `package.json` file).

```
volta run --node latest --bundled-npm --env NPM_CONFIG_LOGLEVEL=silly npm install
```

This will run `npm install` using the latest Node, the npm version bundled with that Node, and with the environment variable `NPM_CONFIG_LOGLEVEL` set to `silly`.

You can even use `volta run` inside your CI configuration to test against multiple tool versions. For example, the following GitHub Actions configuration for a job will run `npm test` using every combination of Node 10, 12, & 14 with npm 5, 6, and 7:

```
strategy:
  matrix:
    node: [10,12,14]
    npm: [5,6,7]
steps:
  - run: volta run --node {% raw %}${{ matrix.node }}{% endraw %} --npm {% raw %}${{ matrix.npm }}{% endraw %} npm test
```

As with any Volta command, if one of the versions you set at the command line hasn't been seen before, Volta will automatically download it and make it available. No need to install it separately, just tell Volta which version to use and it will make it happen! For more details about the available command-line flags, visit the [documentation](https://docs.volta.sh/reference/run).

While most of the time Volta's automatic version detection means you don't have to worry about which version of Node or npm is needed, sometimes you need to take over and have full control. In those situations, `volta run` is a powerful tool that lets you tell Volta directly which versions to use for a one-off command.

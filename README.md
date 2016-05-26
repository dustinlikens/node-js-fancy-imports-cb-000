Fancy Imports
---

![fancy](http://i.giphy.com/sEueensqIsLpm.gif)

## Overview

We've learned so far about `require`ing modules from the local filesystem and importing them from [npm](http://npmjs.org/). But `npm` (the command-line tool) allows us to be much more general — we can just use [urls as dependencies](https://docs.npmjs.com/files/package.json#urls-as-dependencies).

## Objectives

1. Explain why you might want to use a URL to specify a dependency
2. Demonstrate how to specify a dependency from GitHub
3. Explain drawbacks of specifying dependencies directly

## Why?

### Bugs

Sometimes, we write bugs. Even the most experienced programmers (except maybe [Donald Knuth](https://en.wikipedia.org/wiki/Donald_Knuth)) write a bug from time to time. And sometimes, these bugs make it into production — or, in the case of npm packages, into the official repository.

You could wait for the package maintainer to fix the package, but that might take a while. You could search for a replacement, but the replacement's API might differ from the library you've been using, and it might come with its own set of bugs.

![bugs](http://i.giphy.com/oSUtmrhRz5te0.gif)

But we're developers! Why not fix this problem ourselves? We can fork the repository, push a fix (and even open a pull request, but this too might take a while to merge), and — well, our dependency is still pointing to npm. How do we change that?

It's simple. Say you have an entry in your `package.json` that requires the [Slack](https://www.npmjs.com/package/slack) package:

```javascript
"slack": "^6.1.0"
```

But we've found a bug. (We don't mean to imply that there is a bug in this package. This is just for example.) So we fork the [repository](https://github.com/smallwins/slack), check in a fix, and tell `npm install` to look at our fixed fork:

```javascript
"slack": "your_name/slack"
```

That's just shorthand for

```javascript
"slack": "git://github.com/your_name/slack.git"
```

### Enhance!

But say you need to add a feature for a new project that you're working on. One thing that's great about the official npm libraries is that you can version your dependencies using [semantic versioning](https://github.com/npm/node-semver). Can we replicate this functionality using GitHub based dependencies?

![you betcha](http://i.imgur.com/zxPlMFR.gif)

We can append a "[commit-ish](https://docs.npmjs.com/files/package.json#git-urls-as-dependencies)" to our dependency. As the official docs note, "The **commit-ish** can be any tag, sha, or branch which can be supplied as an argument to **git checkout**. The default is **master**."

So we can do something like

```javascript
"slack": "git://github.com/your_name/slack.git#next"
```

To point to the `next` branch. Or

```javascript
"slack": "git://github.com/your_name/slack.git#{SHA1}"
```

to point to the commit with the given `SHA1`. So even when we're outside of official npm land, we can lock in the versions of our dependencies.

Note that you require the package the exact same way: `require('slack')`, in this case.

## Why else?

We might not be fixing a bug when we use a GitHub-based npm package. We might just be adding a feature that we need (but that might not be useful or could be confusing for other users); we might be reviving a dead package, or we might be trying out a new package that we've written that we aren't yet ready to publish to npm. These are all great reasons to use URL-based dependencies.

We might also be using a private library that we _can't_ publish to npm. In thiscase, we'll need to configure our server so that it can authenticate with, e.g., GitHub to access the private repository; but once that's done, it's just a matter of pointing to the URL — `npm install` will take care of the rest.

## Why not?

For now, if you can, it's best to use the official packages published on [npm](https://npmjs.org). These packages are maintained by some great folks, and you'll be benefiting from the collective brilliance of a pretty cool community.

We say "for now," because it could be that a better package management solution emerges. And the flexibility of `npm install` lets us make use of those packages with ease — so if and when it comes time to switch, we'll just need to update a few lines in our `package.json`s and we'll be good to go.

## Resources

- [package.json documentation](https://docs.npmjs.com/files/package.json#urls-as-dependencies): https://docs.npmjs.com/files/package.json#urls-as-dependencies

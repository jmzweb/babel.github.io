---
layout: docs
title: FAQ
description: Frequently Asked Questions and Answers
permalink: /docs/faq/
---

## Why is the output of `for...of` so verbose and ugly?

This is necessary in order to comply with the spec as an iterators `return` method must be called on
errors. An alternative is to enable [loose mode](/docs/plugins/transform-es2015-for-of/#options-loose) but please note
that there are **many** caveats to be aware of if you enable loose mode and that you're willingly choosing
to be spec incompliant.

Please see [google/traceur-compiler#1773](https://github.com/google/traceur-compiler/issues/1773) and
[babel/babel#838](https://github.com/babel/babel/issues/838) for more information.

## Why are `this` and `arguments` being remapped in arrow functions?

Arrow functions **are not** synonymous with normal functions. `arguments` and `this` inside arrow functions
reference their *outer function* for example:

```javascript
const user = {
  firstName: "Sebastian",
  lastName: "McKenzie",
  getFullName: () => {
    // whoops! `this` doesn't actually reference `user` here
    return this.firstName + " " + this.lastName;
  },
  // use the method shorthand in objects
  getFullName2() {
    return this.firstName + " " + this.lastName;
  }
};
```

Please see [babel/babel#842](https://github.com/babel/babel/issues/842), [babel/babel#814](https://github.com/babel/babel/issues/814),
[babel/babel#733](https://github.com/babel/babel/issues/733) and [babel/babel#730](https://github.com/babel/babel/issues/730) for
more information.

## Why is `this` being remapped to `undefined`?

Babel assumes that all input code is an ES2015 module. ES2015 modules are implicitly strict mode so this means
that top-level `this` is not `window` in the browser nor is it `exports` in node.

If you don't want this behaviour then you have the option of disabling `strict` in the [es2015-modules-transform](http://babeljs.io/docs/plugins/transform-es2015-modules-commonjs/#usage).

**PLEASE NOTE:** If you do this you're willingly deviating from the spec and this may cause future
interop issues.

## Help?! I just want to use Babel like it was in 5.x! Everything is too complicated now!

We hear you! Babel 6 requires a tiny bit of configuration in order to get going.
[We think this is for the best](/blog/2015/10/29/6.0.0) and have added
[presets](/docs/plugins#presets) to ease this transition.

## Upgrading from Babel 5.x to Babel 6

At the heart of Babel 6 are [plugins](/docs/plugins). What plugins you need completely
depends on your specific configuration but just add the following `.babelrc` file to
get all the same transforms that were in Babel 5:

```json
{
  "presets": ["es2015", "react", "stage-2"]
}
```

```sh
npm install babel-preset-es2015 babel-preset-react babel-preset-stage-2 --save-dev
```

Also check out our [Setting up Babel 6](http://babeljs.io/blog/2015/10/31/setting-up-babel-6) blog post.

## Where did all the docs go?!

Babel 6 removes a lot of the options in favor of <a href="/docs/plugins">plugins</a> so a
lot of the docs are no longer applicable.

For every removed option there should be a plugin for it. It's possible we may have missed
something, if you think this is the case, please
<a href="https://github.com/babel/babel/issues">open an issue</a>!

Babel is an open source project and we appreciate any and all contributions we can get.
Please help out with documentation if you can by submitting a pull request to the
[babel.github.io](https://github.com/babel/babel.github.io) repo.

## How do I build babel from source?

See [build instructions](https://github.com/babel/babel/blob/master/CONTRIBUTING.md#developing).

## How do I contribute to Babel?

See [contributing](https://github.com/babel/babel/blob/master/CONTRIBUTING.md).

## Why am I getting a Syntax Error/Unexpected Token?

It's most likely the case that you didn't include a plugin/preset that supports that feature. (It's also possible it's a bug in the parser, or that it actually is a syntax error).

## Why isn't a certain babel-x package updated?

We currently use [Lerna's fixed versioning](https://github.com/lerna/lerna#fixedlocked-mode-default) system.

We have a global version for all packages. When we do a release, the only packages that get updated are the packages that
actually had changes (we do a `git diff` on that folder).

If we only update `babel-plugin-transform-exponentiation-operator` to 6.x.x, currently we don't publish a new version for all packages since the other dependencies are using `^`.

For example, the Babel [v6.6.0 release](https://github.com/babel/babel/releases/tag/v6.6.0) doesn't mean all packages are now 6.6.0.

> To make sure you are using the latest package versions, you may need to remove `node_modules` and `npm install` again.

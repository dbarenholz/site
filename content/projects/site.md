---
title: Personal site
description: "My personal site, created using Zola with custom made theme."
date: 2026-05-01
taxonomies:
    tags: ["code", "markdown", "html", "css"]
---

You're looking at this project right now.
It's a personal website built with [Zola](https://www.getzola.org) and a custom theme.
I wrote a blogpost about hosting it [here](@/notes/site-hosting.md).

> The git repository of the site itself: [here](https://github.com/dbarenholz/site).
>
> The git repository of the theme it uses: [halcyon-zola](https://github.com/dbarenholz/halcyon-zola).

Because Zola is a rust based project, and thus shups as single binary, it's (architecturally) nontrivial to extend it with plugins.
As such, I've had to write some Javascript to add functionality.

## theme toggle

Part of the theme is allowing users to choose dark or light mode.
By default, the theme uses your system preferences.
But sometimes you want the other one, or your system is misconfigured and gives you a flashbang at 2AM.
Toggling is implemented by storing the current theme in [localStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage), and modifying it on click of the toggle.
It's _also_ modified if the user changes their system preferences mid-browse.
The icon of the theme toggle is [Lucide](https://lucide.dev/)'s under the [ISC License](https://lucide.dev/license).

## mathjax

While prerendering math should absolutely be feasible with a custom build script or forking Zola, I don't want to maintain either.
A quick client-side [mathjax](https://docs.mathjax.org/en/latest/basic/mathjax.html) script does the job... if we convince Zola to let it.
The setup used is that on `pageReady` we call `fixCollapsedNewlines()`, which uses a [`TreeWalker`](https://developer.mozilla.org/en-US/docs/Web/API/TreeWalker) to walk the DOM, find text nodes that include `$$` (the open delimiter of display math), and then it `normalizeDisplayMathText()` for each of those nodes.
The reason this is necessary is because Zola decides that `\\` within a math block is an escaped backslash, and thus gets replaced with a single `\`.
I'd like to specifically point out arguably the worst regex I've used so far in my life (I tend to try and avoid them):

```js
return /(^|[^\\])\\\s*$/.test(line)
    ? line.replace(/\\(\s*)$/, String.raw`\\$1`)
    : line;
```

Pure art.

## more features

I have a dedicated site for the theme and all of its features.
Check [some features](https://dbarenholz.github.io/halcyon-zola/#features) and even [more features](https://dbarenholz.github.io/halcyon-zola/features/) on it. You'll find information on diagrams with mermaid, exceedingly hacky `[TOC]` support, and more.

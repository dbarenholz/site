---
title: Omiyage
description: "A Spring+Svelte online wishlist application. Vibe coding experiment."
date: 2026-06-16
taxonomies:
    tags: ["code", "java", "svelte", "docker"]
---

Omiyage is a wishlist application. It's publically available [here](https://omiyage.dbarenholz.com/).

> The git repository of omiyage: [here](https://github.com/dbarenholz/omiyage).
>
> Look at CI for omiyage [here](https://ci.dbarenholz.com/repos/2).

A selfhosted wishlist application, created largely by vibe coding[^1] as an experiment[^2].
Choice of technology is my own, namely [Spring Boot](https://spring.io/projects/spring-boot) for the backend and [SvelteKit](https://svelte.dev/docs/kit/introduction) for the frontend.
This because I'm relatively familiar with them, so that I can fix hallucinations.

You can find a lot of information in the README file on github, but roughly speaking, here's why omiyage is cool:

- **passwordless authentication**: instead of an e-mail/password combination, users get a 16-digit account number upon signup. This way there's no potential for leaks.
- **sharing**: you can share wishlists ala google drive. People are generally familiar to get such "shared link" and know how to use it.
- **wish claiming**: logged-in users can claim and unclaim wishes to avoid duplicate gifts, while the list owner can never see claim status.
- **the basics**: it's got multiple lists and wishes and tags that can be used to sort and filter and also there's multiple supported currencies. But it's a wishlist. This much makes sense.

In any case, it's intended to be selfhosted[^3] with docker. Give it a go on my instance, or deploy your own by reading the readme on github.

[^1]: Note that while the project is created by vibe coding, this page doesn't contain any generated content.
[^2]: I plan to rewrite it from scratch without LLMs, so that the code is actually maintainable.
[^3]: It uses CORS, and is a pain to get working in its current vibed state.

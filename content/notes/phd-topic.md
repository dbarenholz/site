---
title: "Interplay between data and processes"
description: "Brief and approachable introduction to my PhD topic."
date: "2023-05-06"
taxonomies:
  tags: ["research"]
---

Consider the following: you want to buy a new pair of `pants`, and a `shirt` at your favourite online store.
To buy your new wearables, you have to:

1. Visit your favourite online store's site.
2. Find `pants`, either by searching for it, or by browsing through the categories.
3. Add `pants to your cart.
4. Repeat steps 2 and 3 for `shirt`.
5. Go to the checkout page.
6. Pay for your items (somehow).

This is a **process** -- a sequence of steps (events) that are taken over time.
In processes, there can be multiple paths to the same outcome (e.g. you could have found `pants` by searching for it, or by browsing through the site).
This example looks simple, right?
Let's imagine we are the store instead.
Each of our customers has their own way of doing things (their own _trace_ through the process).
As store we have to make sure that the right products (= data) go to the carts of the appropriate customers (= data), producing the right orders/invoices (= data), and more.
No longer simple, and this is only to make sure things happen _correctly_.

> Usually, we're not just interested in correct execution of a process, but we want to gain insight into the process itself.
> The following questions are common:
> 
> 1. How many different paths are there through the process? How do they differ? How long do they take? Why?
> 2. Which products are being bought? Are there patterns we can discover?
> 3. Can we somehow improve the process?

As hinted at in above paragraph, products, customers, orders, and invoices are all examples of "data objects".
What I look into in my PhD is how we can work with these data objects using a process-aware hat.
In particular, I look at:

- How do we **model** (interacting) multi-process systems?
- How do we **design** (interacting) multi-process systems?
- How do we **analyze** (interacting) multi-process systems?
- How do we **build** (interacting) multi-process systems?
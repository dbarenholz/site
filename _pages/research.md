---
permalink: /research/
title: Research
toc: false
classes: wide
---

## Publications

See [google scholar](https://scholar.google.com/citations?user=bFXkeeQAAAAJ&hl=en&inst=7240083048524121927&oi=ao) for a more up-to-date version.

1. [On the Reconstructability and Rediscoverability of Typed Jackson Nets]({{ site.url }}/notes/research/on-the-reconstructability-and-rediscoverability-of-typed-jackson-nets).


## Research Topic

I am currently doing a PhD at [Utrecht University](https://www.uu.nl/staff/dbarenholz) on _the interplay between processes and data_.
To explain what this means, consider following example.
Suppose that you want to buy a two products, a pair of pants and a t-shirt, online at your favourite store. 
Imagine that these products can be identified by _p1_ for the pants, and _p2_ for the t-shirt.

To buy these products, you as customer have to perform certain actions:
1. Visit the site of your store.
2. Search for _p1_ (or use the sites' navigation to directly go to the product).
3. When on the product page of _p1_, click a button to add it to the cart.
4. Repeat steps 2 and 3 for product _p2_.
5. Go to your cart, and checkout.

This is a **process**. As illustrated at step 2, there may be multiple _paths_ towards an end-goal. 
Simple, right? Now imagine you are the store, and you have a million customers. 
Each customer has their own way (their own _path_) to reach the checkout-step.
As the store, you need to make sure that the right products (= data) go into the carts of the right customers (= data), producing the right orders (= data), allowing for a correct execution of your business process. 

> Usually you are not only interested in a correct execution, but you also want to gain insights into your processes.
> The following questions are not uncommon:
> * How many different paths are there? How long do they take? Why?
> * Which products are bought? Are there patterns we can discover?
> * Can we improve the process? How?

As hinted at by the parentheses, products, customers, and orders in this scenario are "data objects". 
What I study during my PhD is how we can work with these "data objects" using a process-aware hat.
In particular, in the coming years I hope to find some answers to, amongst others, the following:
* How do we model multi-process systems?
* How do we design multi-process systems?
* How do we analyze multi-process systems?
* How do we build multi-process systems?

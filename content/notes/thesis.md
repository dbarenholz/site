---
title: "Knowledge Graphs for Improving Robot Operations in Logistics"
description: "Master thesis, done at Vanderlande."
date: "2022-10-17"
taxonomies:
    tags: ["thesis", "neo4j", "python"]
---

> Find the complete thesis [here](/docs/thesis/thesis.pdf).
> Find this work on [google scholar](https://scholar.google.com/scholar?oi=bibs&hl=en&cluster=14011193794507456609), or on [research.tue.nl](https://research.tue.nl/en/studentTheses/knowledge-graphs-for-improving-robot-operations-in-logistics/).

## Abstract

[Vanderlande](https://www.vanderlande.com/) is a company providing future-proof logistic process automation in, amongst others, warehousing for the food segment.
This segment requires high availability and diversity of products with a limited workforce.
To combat aforementioned problems within the food segment, Vanderlande has developed the STOREPICK evolution: a robotised, end-to-end Automated Case-Picking (ACP) warehousing solution, consisting of various modules, each with their own dataset(s).
This thesis is the first data-driven approach to making Vanderlande’s ACP solution more robust against errors.
Part of the STOREPICK evolution is a palletizer cell, where a robot arm grabs and places cases on top of a pallet.
We call this process (automated) palletisation.
Notice how the palletisation process occurs in a physical setting.
We implement a proof of concept data integration pipeline to construct a knowledge graph describing the physical palletisation process from the various available datasets, and evaluate which questions (about the palletisation process) can be answered reliably, either by querying it or using visual analytics.
During this exploration on the usecase of knowledge graphs for modelling both a physical setting in tandem with its process for the purpose of detecting machine faults, we find that there is a critical data quality issue with respect to the recorded Z axis values of cases on pallets.
We discuss the consequences of the data quality issue, and provide insights into other potential usecases of the graph as data model, comparing it to a more traditional tabular data format.

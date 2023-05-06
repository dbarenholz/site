---
title: "Probabilistic models in process discovery"
excerpt: "Results of an internship at the Process Analytics group in Eindhoven."
tag: research
---

Read the complete report [here]({{ site.url }}/_assets/docs/internship_pa/report.pdf).
<br />
Find this item on [google scholar](https://scholar.google.com/citations?view_op=view_citation&hl=en&user=bFXkeeQAAAAJ&citation_for_view=bFXkeeQAAAAJ:u5HHmVD_uO8C).

## Main Idea

Process discovery (PD) in process mining is the task of transforming an event log into an interpretable model, usually a Petri net.
Traditionally this transformation is achieved in an algorithmic sense, for instance using the Alpha algorithm.
The event log, which is the input data to the algorithm that solves PD, is noisy by definition and it stems from some unknown probability distribution $$p^*$$.
The problem with current algorithmic approaches is that they have a fundamental short-coming: there are a priori assumptions on said data, for instance that infrequent behaviour is irrelevant to the overall process.
Using a machine learning based approach attempts to generalize away from such assumptions, as done in the thesis by D. Sommers where Graph Convolutional Networks (GCNs) were used to solve the PD problem.
GCNs, however, do not consider learning $$p^*$$, even though the probability distribution can be used to optimally solve classification and prediction tasks.
As such, in this internship we want to investigate if using probabilistic models, which learn the distribution of the data, is useful in solving the process discovery problem.
Ideally we want to have an explainable model, but we will settle with less explainable if in return we get superior performance.

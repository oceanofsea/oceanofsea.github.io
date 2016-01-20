---
layout: post
title:  "2016-ICDE-learning to query"
date:   2016-01-15
categories: Query
tags: ICDE query 2016 
excerpt:
---
* content
{:toc}

## Problem
intelligently select queries so that we can
harvest pages, via a search engine, focused on an entity aspect
of interest.

## Contribution
First, a target entity in a given domain has many
peers. We leverage these peer entities to become domain aware.

Second, a candidate query may “overlap” with the past queries
that have already been fired. We account for these past queries to
become context aware.

we developed models for domain and contextaware
L2Q. We **started with a basic utility inference model**
for queries, which was then enhanced with the domain and
context. **For domain awareness**, we devised templates to address
entity variations; **for context awareness**, we formulated
collective utilities to manage multiple queries.

## Insight
Useful query -> Useful page

Useful page -> Useful query

Reinforcement graph

## Comment
这篇paper挺有意思的，如何learn to find good queries to get the good results. And the good results can contribute to good queries.

---
layout: default
title: Searching and Sorting I
parent: Data Structures and Algorithms I
nav_order: 1
---

# Searching and Sorting I
We consider the following motivating example for this entire section. Alice is a marine biologist who has harvested a large number of waterborne bacteria and sequenced their DNA. She has obtained one billion (1,000,000,000) unique 30 bp DNA sequences. Alice would like to publish the data she has collected as a public database for other scientists to use, so that they can search for their sequences of interest. How can Alice best store her data?

## Storing the Data 'as-is' is Inefficient
The simplest option for Alice is store the data 'as-is'. That is, she just needs to upload her gigantic `.fasta` file and let the other scientists search for their sequences on their own. Unfortunately, this is really slow. In fact, the average number of sequences checked will be 
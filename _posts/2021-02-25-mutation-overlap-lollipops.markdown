---
layout: post
title: "Mutation Lollipops"
subtitle: >
    A pictogram of a gene's genomic variants present in a cohort of tumour samples showing both the number of samples
    each mutation appears in as well as how many of the mutations co-occur with larger-scale copy number alterations.
plot:
  src: /assets/img/plots/lollipop_fig.svg
  alt: website picture
thumb:
  src: /assets/img/plots/lollipop_thumb.svg
  alt: website picture
---

We wanted to create a representation of a gene's mutation landscape in a particular cohort of tumour samples, similar to what sites like cBioPortal [offer](https://bit.ly/3ommyvS). These types of plots are commonly referred to as "lollipop plots", and they allow us to view at a glance the most frequently mutated loci, and where these loci are located on the genome relative to one another and to other structures present in the genome such as protein binding domains.

For our most recent project, we are studying how the presence of overlapping copy number alterations affects classifiers trained to predict the presence of a gene's mutations. We thus wanted to show not only the smaller-scale "hotspot" variants usually depicted in a lollipop plot, but also how frequently larger-scale gains or deletions of entire regions of the genome co-occur with these mutations in a cohort. Although it would be difficult to show this information for every single loci without cluttering the plot, we found a solution to this problem by only showing overlap with gains and losses in the cases of mutations that appear in at least ten samples. We also added a pie chart legend to the plot illustrating the degree of overlap with such alterations across all mutations of the given gene.


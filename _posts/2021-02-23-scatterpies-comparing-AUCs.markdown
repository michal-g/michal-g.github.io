---
layout: post
title: "Scatterpies Comparing AUCs"
subtitle: >
    To compare the AUCs of pairs of related tasks, we created a scatterplot where each point is
    a pie chart annotating the relationship between the two tasks.
plot:
  src: /assets/img/plots/scatterpie_fig.svg
  alt: website picture
thumb:
  src: /assets/img/plots/scatterpie_thumb.svg
  alt: website picture
---

The crux of [our recent publication in BMC Bioinformatics](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-021-04147-y) was the claim that models trained to predict the presence of cancer mutations from expression data could be improved by taking into account the diversity of mutations within each gene. Previous work in this domain tended to treat all mutations within a gene identically; we showed that models often performed better when instructed to predict the presence of a subset of mutations within the gene (the "subgrouping" task) as opposed to predicting the presence of any mutation in the gene (the "gene-wide" task).

To illustrate this improvement for a single tumour cohort in which we searched for superior subgroupings we created a scatterplot with each point representing a gene in which we tested subsets of mutations. The position of a gene's point thus corresponds to the gene-wide AUC (x-axis) and the AUC of its best found subgrouping task (y-axis). However, instead of plotting points, we plotted pie charts showing the proportion of a gene's mutations that belonged to this best subgrouping. For cases where the best subgrouping task was significantly better than the gene-wide task we also added labels giving a condensed description of the subgrouping.

The main difficulty when implementing this design was figuring out how to use the `inset_axes` function from `mpl_toolkits.axes_grid1.inset_locator` in the matplotlib API. This is an experimental function, and as such its documentation is fairly opaque; nevertheless, it is very useful for creating plots which combine multiple types of data visualization. In particular, we create a separate `axes` object for each pie chart which occupies a small portion of the space within the main `axes` object spanning the entire plot. See the `plot_sub_comparisons` function in our [python module](https://github.com/ohsu-comp-bio/dryads-research/blob/master/experiments/subgrouping_test/plot_aucs.py) for AUC comparison analyses related to this paper for further details.

For this plot I also created an [algorithm](https://github.com/ohsu-comp-bio/dryads-research/blob/master/experiments/utilities/label_placement.py) for automatically annotating points in a scatterplot such that these labels don't collide with one another or the plotted points. This algorithm first looks for points whose labels can be placed directly beside the point, such as ARID1A and GATA3 in the above example. For the remaining points, it randomly chooses a location in the proximity of the point using an exponential distribution and then checks if the location is suitable for placing the label. If it isn't, another location is randomly chosen, with the "radius" of the distribution growing a small amount after each iteration to allow for a larger search space surrounding the point. Although relatively simple, this greedy algorithm proved to be quite useful in a number of other plots in which individual points needed to be annotated with a descriptive label.

The [data portal](https://osf.io/28vxb/) for this publication contains many more examples of this type of plot - see for example the same analysis repeated for all of the tumour cohorts used in our study (S11 - Subgrouping AUC Comparisons by Cohort) and the same analysis broken down by mutated gene instead of cohort (S12 - Subgrouping AUC Comparisons by Gene).


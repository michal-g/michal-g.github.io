---
layout: post
title: "Coefficient Clustermaps"
subtitle: >
    This is a typical heatmap with hierarchically clustered rows and columns which we augmented with additional
    information about the regression models whose coefficients are shown. 
plot:
  src: /assets/img/plots/clustmap_fig.svg
  alt: website picture
thumb:
  src: /assets/img/plots/clustmap_thumb.svg
  alt: website picture
---

Although the performance of a model is important for judging its usefulness, we must also consider its interpretability &#151; that is, can we use the properties of the model to reach meaningful conclusions about the underlying processes it is trying to quantitatively represent? In our case, we apply classification models to expression data to predict the presence of a mutation in a tumour cohort, so these models' performance (as measured by AUC) tells us whether or not the classifier is finding an expression signature strongly associated with the mutated state. However, to demonstrate that these classifiers are biologically meaningful, and not picking up a random signal in the expression data that just happens to be correlated with the presence of a particular mutation, we examine the genes whose expression features play the largest role in helping the classifiers make their prediction.

In our [recent paper](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-021-04147-y) we showed that for many genes we can find better-performing classifiers for genes often mutated in cancer contexts by using subsets of mutations within these genes instead of treating all of a gene's mutations as a uniform whole. To augment this finding we also wanted to show that classifiers trained using these subgroupings of mutations were divergent not just in performance, but also in the genes whose expression they used to predict the presence of particular subgroupings. We can thus show that the downstream biological footprint of genes' mutations is not homogeneous, but can instead vary just as the effect of one gene's mutations varies from that of another gene.

We use logistic regression classifiers, so our models consist of a linear combination of the expression features in which each gene feature is assigned a weight. A positive weight indicates that higher expression of the gene is associated with the presence of the mutation, and a negative weight indicates that its higher expression is associated with the absence of the mutation. We can thus compare these weights across all of the subgrouping models we tested for a given mutated cancer gene to find which other genes' expression is most strongly impacted by different subsets of the gene's mutations.

The above heatmap is our visualization of these weights for a selection of subgrouping models for a particular gene. Each row corresponds to a single model, with a column for each gene whose model expression weights were used to make mutation predictions. Both rows and columns are hierarchically clustered so that subgroupings and genes with similar weights are plotted closer together. We also show a partition of the subgroupings into four clusters to illustrate the overall structure of similarities across our models.

We took several steps to try and maximize the amount of information presented while keeping the plot easily readable. As for many genes we tested dozens if not hundreds of subgroupings (in this case, 34) we had to prune the number of models we presented using an AUC cutoff of 0.7 to only show the reasonably well-performing cases. Within each of our four clusters, we also pruned subgroupings which had other very similar subgroupings (as measured by Jaccard index of mutation labels in the cohort) in the same cluster that also had higher AUCs. Likewise, with 13018 gene expression features used in each model, we had to employ a strategy to only show the most "interesting" genes. In particular, we only plotted model coefficients for genes which appeared in the top five according to absolute model weight in at least one subgrouping passing the aforementioned filters.

To help the reader better understand the biological significance of each cluster, we annotated each row with a short description of the associated with subgrouping, along with information on its mutations' prevalence in the cohort and its classification performance. We also added asterisks to the subgrouping AUCs in cases where they were significantly higher than that of the gene-wide (i.e. any point mutation) task, and bolded the best performing subgrouping in each cluster along with the gene-wide task.

This visualization is similar to the clustermaps available through the [seaborn Python package](https://seaborn.pydata.org/generated/seaborn.clustermap.html), although we instead used `seaborn.heatmap` to have greater control over plotting aesthetics. More examples of this type of plot can be found at the [data portal](https://osf.io/28vxb/) for our publication under <i>S3 - Gene Coefficient Heatmaps</i>. The code used to create these plots can be found at `plot_auto_heatmap()` in our [python module](https://github.com/ohsu-comp-bio/dryads-research/blob/master/experiments/subgrouping_test/plot_models.py) for creating model coefficient heatmaps for use in our paper.


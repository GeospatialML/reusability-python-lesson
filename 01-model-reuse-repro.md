---
title: "Modelling and model reusability and reproduciblity in the context of a case study"
teaching: 0 # teaching time in minutes
exercises: 0 # exercise time in minutes
---

:::::::::::::::::::::::::::::::::::::: questions 

- Why is model reusability and reproducibility important? 
- How can you facilitate your model reuse and reproducibility?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Describe the main phases of data-driven geospatial modelling
- Explain the importance of geospatial model reusability and reproduciblity
- Discuss the main challenges of geospatial model reusability and reproduciblity 

::::::::::::::::::::::::::::::::::::::::::::::::

## Introduction

In this lesson, you will learn why reusability and reproducibility matter in geospatial modelling.

We begin by introducing the main phases of data-driven geospatial modelling. Then, we move on to model reusability and reproducibility.

We close by discussing the main challenges of reusability and reproducibility in geospatial modelling.



## Geospatial modelling process - general workflow

Data-driven approaches, meaning that models are built with parameters learned from observations’ data and aim to simulate new data minimally different from the “ground truth” under the same set of descriptive features. Among the standards guiding the implementation of data-driven model applications in general, CRISP-DM (Cross-Industry Standard Process for Data Mining) remains the most widely adopted reference framework. CRISP-DM structures a modelling project into a series of iterative phases including problem and data understanding, data collection and preparation, model selection, model training and hyperparameters optimization, model evalution, deployment and inference. Rather than following a stricly linear approach, CRISP-DM adopts a cyclical one allowing insights and lessons learned in later stages to inform (feedback) and refined decisions made in earlier phases. 

In this episode we focus on data-driven approaches which mean that models learn their parameters from observations. The figure below illustrate the CRISP-DM adapted to geospatial data as envisoned by [1]. 


![Modelling workflow. Adapted from [1]](modelling_workflow.png){alt='Modelling workflow'} 

1. Spatial data collection


Spatial data collection involves getting geospatial observations relevant to the study area, such as satellite imagery (e.g. Sentinel-2), aerial photography, LiDAR point clouds, or in-situ field measurements. This stage must account for spatial and temporal coverage, sensor resolution, and coordinate reference systems. Metadata documentation at this stage, including acquisition dates and preprocessing history, is essential for later reproducibility.

2. Spatial data analisys

Spatial data analysis explores the collected data to understand its structure, quality, and underlying spatial patterns before the modelling process. This includes data quality assessement such as handling missing or corrupted pixels, assessing spatial autocorrelation, inspecting class or value distributions, and visualizing the study area to identify potential confounders such as clouds, water bodies, or shadow. Exploratory analysis at this stage often reveals data-quality issues and spatial patterns.

3. Model building

Model building covers the selection and training of one or more algorithms, ranging from classical machine learning methods such as Random Forest or LightGBM to deep learning architectures, on patches or samples derived from the study area. Feature engineering, hyperparameter selection, and the choice of loss function all take place here according to the nature of the task, i.e., regression, classification, or clustering. Experiment tracking tools such as MLflow are particularly valuable at this stage to log model versions, parameters, and training features.

4. Validation

Validation assesses whether the trained model generalizes beyond the data it was trained on. Metrics should be chosen to reflect the spatial nature of the task such as error metrics for regression, or robust accuracy metrics for classification. Results should be interpreted alongside spatial error maps.

5. Model deployment

Model deployment moves a validated model into operational use. This stage requires monitoring for performance drift as new imagery or conditions diverge from the training distribution, along with clear versioning so that predictions can always be traced back to a specific model and dataset version.


## Model reusability and reproducibility

1.2	Importance of geospatial model reusability and reproducibility

Training machine learning models require a vast amount of data; deep learning models in particular are data-hungry so re-using them is crucial. Moreover, fine-tuning deep learning models, where you re-used a previously train model, is a common practice. Reusing models let scientist can understands what work and what does not, and enables fair comparison. 
As new data become available, it is crucial that previously trained models can be reused

Reproducibility is the ability to reproduce a ML experiment and obtain the same exact result [2]. It is essential for scientific rigour and for fair comparison between models, for example, when benchmarking different approaches to a specific problem. 


### Pre-trained models



### Foundational models

## Challenges of reusability and reproducibility in geospatial modelling


	
    Reproducibility across OS / Interoperability (awareness level)
	Metadata and model documentation
	Data standards in the geosciences
	Geospatial data and model repositories 
	Specific libraries xarray and geopandas – Community standards/preferences and compatibility with MLOps
	Open models/weights for deep learning models 
	Knowledge safety. Sensitive data. 




::: discussion
Why is reproducibility important?

::: hint
this is my hint
:::

:::

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: instructor

Inline instructor notes can help inform instructors of timing challenges
associated with the lessons. They appear in the "Instructor View"

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: challenge 

## Challenge 1: Can you do it?

What is the output of this command?

```r
paste("This", "new", "lesson", "looks", "good")
```

:::::::::::::::::::::::: solution 

## Output
 
```output
[1] "This new lesson looks good"
```

:::::::::::::::::::::::::::::::::


## Challenge 2: how do you nest solutions within challenge blocks?

:::::::::::::::::::::::: solution 

You can add a line with at least three colons and a `solution` tag.

:::::::::::::::::::::::::::::::::
::::::::::::::::::::::::::::::::::::::::::::::::

## Figures

You can use standard markdown for static figures with the following syntax:

`![optional caption that appears below the figure](figure url){alt='alt text for
accessibility purposes'}`

![You belong in The Carpentries!](https://raw.githubusercontent.com/carpentries/logo/master/Badge_Carpentries.svg){alt='Blue Carpentries hex person logo with no text.'}

::::::::::::::::::::::::::::::::::::: callout

Callout sections can highlight information.

They are sometimes used to emphasise particularly important points
but are also used in some lessons to present "asides": 
content that is not central to the narrative of the lesson,
e.g. by providing the answer to a commonly-asked question.

::::::::::::::::::::::::::::::::::::::::::::::::


## Math

One of our episodes contains $\LaTeX$ equations when describing how to create
dynamic reports with {knitr}, so we now use mathjax to describe this:

`$\alpha = \dfrac{1}{(1 - \beta)^2}$` becomes: $\alpha = \dfrac{1}{(1 - \beta)^2}$

Cool, right?

::::::::::::::::::::::::::::::::::::: keypoints 

- Use `.md` files for episodes when you want static content
- Use `.Rmd` files for episodes when you need to generate output
- Run `sandpaper::check_lesson()` to identify any issues with your lesson
- Run `sandpaper::build_lesson()` to preview your lesson locally

::::::::::::::::::::::::::::::::::::::::::::::::

## References

[1] Koldasbayeva, D., Tregubova, P., Gasanov, M. et al. Challenges in data-driven geospatial modeling for environmental research and practice. Nat Commun 15, 10700 (2024). https://doi.org/10.1038/s41467-024-55240-8

[2] D. Kreuzberger, N. Kühl and S. Hirschl, "Machine Learning Operations (MLOps): Overview, Definition, and Architecture," in IEEE Access, vol. 11, pp. 31866-31879, 2023, doi: 10.1109/ACCESS.2023.3262138. keywords: {Interviews;Machine learning;Training;Collaboration;Bibliographies;Automation;Codes;CI/CD;DevOps;machine learning;MLOps;operations;workflow orchestration},

[r-markdown]: https://rmarkdown.rstudio.com/

---
title: "Modelling and model reusability and reproduciblity"
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

## 1.1. Introduction

In this lesson, you will learn why reusability and reproducibility matter in geospatial modelling.

We begin by introducing the main phases of data-driven geospatial modelling. Then, we move on to model reusability and reproducibility.

We close by discussing the main challenges of reusability and reproducibility in geospatial modelling.

## 1.2. Geospatial modelling process - general workflow

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

## 1.3. Importance of geospatialModel reusability and reproducibility

Training machine learning models require a vast amount of data; deep learning models in particular are data-hungry so re-using them is crucial. Moreover, fine-tuning deep learning models, where you re-used a previously train model, is a common practice. Reusing models let scientist can understands what work and what does not, and enables fair comparison. 
As new data/EO mission become available, it is crucial that previously trained models can be reused, either for inference or through fine-tuning. 

Reproducibility is the ability to reproduce a ML experiment and obtain the same exact result [2]. It is essential for scientific rigour and for fair comparison between models, for example, when benchmarking different approaches to a specific problem. 

A minimum level of reproducibility is reached when you, as the author, can reproduce yourself your own experimental results which already requires deliberate effort. A higher level is reached when peers can reproduce your results which can increase trust, interest, and citations [3].

### Simple rules for reproducible computational research
Several recommendations exist for addressing reproducibility in geospatial research [4]. Below, we present ten well-known rules for reproducible computational research [3], which have been contextualized to geospatial modelling workflows. These rules aim to facilitate the reproducibility of research outputs, including data, models and derived artifacts such as performance metrics and figures.

1. For every result, keep track of how it was produced
In geospatial ML/DL, a single result, such as a segmentation map or an accuracy score comes out of a (long) chain of steps enclosed in phases as illustrated in modelling workflow above. Each step involves parameter choices that affect the outcome, for example, CRS, spatial resolution, tile size and overlap and data splitting strategy. Therefore, record the full provenance of every result. This means, in practice, keeping the computational workflow in executable scripts or pipelines and storing the configuration, e.g., a YAML file together with each experiment. A YAML (Yet Another Markup Language) file is a human-readable format for representing structured data in a manner that us both understandable by humans and interpretable by machines [5].

2. Avoid manual data manipulation steps
Geospatial workflows often involve interactive steps in desktop GIS software, such as label digitising, raster clipping to a study area or editing attribute tables. These steps are difficult to document and nearly impossible to reproduce exactly. As possible, they should be replaced by scripts. Some manual work maight not be avoided, for example, expert dataset annotation. In suc case, the resulting dataset should be versioned and archived. The annotation protocol should also be documented to make the process traceable.

3. Archive the exact versions of all external programs used
Geospatial modelling workflows heavily depends on specialized software, for example, geospatial libraries such as GDAL, PROJ, rasterio, (Geo)Pandas and ML/DL frameworks such as scikit-learn, PyTorch and TensorFlow. Version changes in any of these components may alter reported results. At a minimum, the names and versions of all software packages should be recorded. Ideally the exact environment configuration should be archived, for example as a container image (e.g., Docker). This is particularly useful in HPC or cloud environments, where software is shared and changes over time.

4. Version control all custom scripts
Manage all custom scripts with a version control system such as Git. Include preprocessing routines, model architectures, custom loss functions, training and analysis notebooks. Each reported result should be backtracking to a specific state of the workflow. 
Large files such as satellite imagery, model weights and predicted rasters are better handled with tools such as Data Version Control - DVC, or archived separately in data repositories and referenced from the code.

5. Record all intermediate results, when possible in standardized formats
Geospatial ML/DL pipelines often produce intermediate products, e.g., pre-processed image tiles, clipped rasters, rasterized masks, dataset splits and prediction maps. Store those intermediate results that allow individual stages to be checked without running the full pipeline, which is particularly important when model training is computationally expensive. Intermediate results may reveal bugs or faulty assumptions that are not apparent in the final results. They can also provide insight into the influence of parameter choices and allow critical examination of the process when not all executables are available or when running them is impractical, e.g., due to computational cost or hardware requirements.

6. For analysis that include randomness, note underlying random seeds
Randomness influence ML/DL workflows in many steps, e.g., data spliting, cross-validation, weight initialisation, data augmentation, random sampling and training patches. Recording the random seeds used by all relevant libraries (e.g., NumPy, scikit-learn, PyTorch) in every relevant step increases the reproducibility of a workflow. Minor differences may nevertheless occur because of non-deterministic GPU operations or differences in hardware and operating systems. To minimise such discrepancies, use deterministic operations wherever possible. When several runs are conducted, record and report every used seed.

**Example**:
The following example presents a function to set all seeds involved in a DL workflow for reproducibility [6].

~~~

from lightning.pytorch import seed_everything
def set_all_seeds(seed=42):
    """
    Set all random seeds for reproducibility

    Parameters:
    ----------
    seed : int
        The seed value
    """
    np.random.seed(seed)
    torch.manual_seed(seed)
    random.seed(seed)
    torch.cuda.manual_seed_all(seed)
    seed_everything(seed, workers=True)
~~~
{: .language-python}

7. Always store raw data behind plots
Store figures such as learning curves, confusion matrices, accuracy maps together with the underline data. Keeping the underline data allows to recreate figures without re-running the whole workflow. When code and data are stored, figures can also be adjusted or re-analyzed by modifying only a portion of the pipeline.

8. Generate hierarchical analysis output, allowing layers of increasing detail to be inspected
Metrics such as *overall accuracy*, F1-score, or intersection over union (IoU) present summarized information. Keep, together with the metrics, the detailed data used to generate them, for example, predicted maps, reference data, and false positive and false negative maps.

9. Connect textual statements to underlying results

Consistent logging of experiments with their corresponding metadata, e.g., model hyperparameters, input features, version, and seeds, facilitates the backtracking of a manuscript's claims to the specific experiment, configuration and output that produced them. Literate-programming approaches such as Jupyter notebooks can further strengthen the link between the narrative, i.e., interpretation or conclusions, and the computational workflow. 

10. Provide public access to scripts, runs and results
The reproducibility of geospatial studies might be hindered by data licensing constraints and privacy concerns. In such cases, provide derived products and sufficiently anonymized results together with precisely documented steps (e.g., scripts) for data gathering and pre-processing. Ideally, in addition to using platforms such as Github, archive the code, configuration files, model weights, and outputs in publicly available repositories such as Zenodo or 4TU.ResearchData that assign a digital object identifier (DOI). 
Ideally the code can be inspected for reproducibility. For example, open science initiatives like CODECHECK offer a space to get an artcle's code verified for reproducibility[7]

### 1.4. Pre-trained models

Geospatial modelling workflows using DL often use pre-trained models, i.e., weights of a previously trained model, because they accelerate the training process and can yield better performance while helping to overcome potential data scarcity constraints. To reproduce a workflow using pre-trained models, it is necessary to provide a link to the models' metadata and their corresponding weights. Metadata often describe model version, expected input data, tile size, features, and the number and order of channels, for example, how many Sentinel-2 bands are used, in which order, and at which resolution. 

A well-known source of pretrained weights is the [Hugging Face Hub](https://huggingface.co/). [Pytorch (torchvision)](https://docs.pytorch.org/vision/main/models.html) offers general-purpose pre-trained weights whereas ([TorchGeo](https://docs.torchgeo.org/en/stable/api/models.html)) provides a wide range of weights pre-trained specifically on satellite imagery. 

For correctness, it is crucial to assess whether pre-trained models are suitable for the computational workflow, i.e., the application-specific task at hand.

Foundational models are large-scale and highly versatile AI models that are pre-trained on massive datasets and can be readily adapted to diverse end tasks across domains.
**Geospatial foundational models** are a specialized class of are foundation models pretrained on heterogeneous spatiotemporal data, often from different missions or modalities, to support diverse downstream geospatial tasks. They can be used for various downstream tasks either directly, through zero-shot inference, or after task-specific fine-tuning. 

-- here XXXX

### 1.5. Challenges of reusability and reproducibility in geospatial modelling

*Reproducibility across OS

Different architectures and operative systems  

	
    Reproducibility across OS / Interoperability (awareness level)
	Metadata and model documentation
	Data standards in the geosciences
	Geospatial data and model repositories 
	Specific libraries xarray and geopandas - Community standards/preferences and compatibility with MLOps
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

[3]Sandve GK, Nekrutenko A, Taylor J, Hovig E (2013) Ten Simple Rules for Reproducible Computational Research. PLOS Computational Biology 9(10): e1003285. https://doi.org/10.1371/journal.pcbi.1003285

[4] Konkol, M., Kray, C., & Pfeiffer, M. (2019). Computational reproducibility in geoscientific papers: Insights from a series of studies with geoscientists and a reproduction study. International Journal of Geographical Information Science, 33(2), 408–429. https://doi.org/10.1080/13658816.2018.1508687

[5] [YAML](https://www.ibm.com/think/topics/yaml)

[6] [Public Urban Green Spaces (PUGS) Detection Workflow](https://github.com/rosaguilar/pugs-detection/blob/main/pugs_detection/utils.py)
[r-markdown]: https://rmarkdown.rstudio.com/

[7] Eglen S, Nüst D. CODECHECK: an open-science initiative to facilitate sharing of computer programs and results presented in scientific publications. Septentrio Conf Series. 2019;1. [(https://doi.org/10.7557/5.4910](https://doi.org/10.7557/5.4910).

---
site: sandpaper::sandpaper_site
---

This lesson focuses on documenting the creative process behind each model, which serves as a good starting point for a set of guidelines to promote FAIR standards within the geospatial ML modeling community.

## Learning Objectives
After following this lesson, learners will be able to:
 
* Design a minimal MLOps workflow/pipeline using *MLflow* to track modelling experiments
* Apply MLOps tools correctly on a *use case* to reproduce an experiment what for?: #)
* Manage data versions with DVC (Data Version Control - [DVC](https://dvc.org/))

::: prereq

* This lesson assumes you have a working knowledge of Python and some previous exposure to software version control (e.g., Git), whether from formal training or self-taught.
If you need to build these skills first, or a refresher, the following materials may be helpful:

* Completing a Python training, e.g., [Software Carpentry Programming with Python](https://swcarpentry.github.io/python-novice-inflammation/) 
* Completing a Version control lesson, e.g., [Version Control with Git](https://swcarpentry.github.io/git-novice/)
* Have completed the first lesson/unit on ML - core concepts or possess equivalent knowledge. Material of this lesson is available at: [Introduction to machine learning for geospatial data](https://geospatialml.github.io/geo-ml-intro-lesson/)
* Be familiar with geospatial modelling and design of experiments/scenarios
* Completing the lesson on spatial ML ([Introduction to data-driven modelling with geospatial data](https://geospatialml.github.io/geo-ml-modelling-lesson/)) is recommended but not required  
* A basic understanding of computational resources, e.g., HPC clusters, cloud platforms) and related concepts (e.g., CPUs, GPUs, data types)
* Familiarity with [FAIR data principles](https://www.go-fair.org/fair-principles/) and [FAIR software principles](https://zenodo.org/records/6623556)

:::

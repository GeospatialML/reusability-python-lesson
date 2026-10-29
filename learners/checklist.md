---
title: "Reproducibility Checklist for Geospatial Machine/Deep Learning Studies"
---

The following checklist helps you self-assess the key components that affect the reproducibility of a geospatial workflow

## A. Experimental design and reporting

### A.1 Study area and data provenance
- [ ] Study area extent reported (bounding box or boundary file) and time period covered
- [ ] Data sources, sensors, and collection/product versions reported (e.g., Sentinel-2 L2A, Landsat Collection 2)
- [ ] Access platform reported (e.g., Copernicus Data Space, Planetary Computer, Google Earth Engine) with access date
- [ ] Scene/STAC item IDs, acquisition dates, and query parameters archived
- [ ] Restricted/commercial data: scene IDs, footprints, and derived labels shared instead of imagery

### A.2 Preprocessing
- [ ] Coordinate reference system (CRS) and any reprojection reported
- [ ] Spatial resolution and resampling method reported
- [ ] Radiometric processing reported (product level, processing baseline, offsets/scaling, e.g., Sentinel-2 baseline ≥ 04.00)
- [ ] Cloud/shadow masking method and thresholds reported
- [ ] Temporal compositing or time-series construction described
- [ ] Tiling/patch size, overlap, and handling of no-data and edges described

### A.3 Labels and reference data
- [ ] Label source and acquisition method described (field survey, photo-interpretation, existing product)
- [ ] Annotation protocol, class definitions, and minimum mapping unit reported
- [ ] Label quality assessed (e.g., inter-annotator agreement, known errors)
- [ ] Temporal match between labels and imagery reported
- [ ] Class distribution reported

### A.4 Data splits and evaluation
- [ ] Data split (train/validation/test) reported, spatially and temporally if relevant
- [ ] Relevant geometries (e.g., GeoJSON/GeoPackage) available in an open repository if possible 
- [ ] Metrics precisely defined (e.g., macro vs. micro F1, background handling in IoU)

---

## B. Computational environment

### B.1 Software and hardware
- [ ] Programming language and key library versions reported, e.g., config.yml, requirements.txt
- [ ] Geospatial library versions reported (e.g., GDAL, PROJ, rasterio, TorchGeo); these can affect reprojection and I/O results
- [ ] Container or virtual environment provided (e.g., Docker, Apptainer)
- [ ] Hardware reported (CPU/GPU model, memory)

### B.3 Randomness and determinism
- [ ] Random seeds fixed and reported
- [ ] Known sources of nondeterminism (e.g., GPU operations, data-loader ordering) documented

### B.4 Code and workflow
- [ ] Code released in GitHub or GitLab
- [ ] Code released in a public repository with a persistent identifier (e.g., Zenodo DOI)
- [ ] Configuration files for all reported experiments released
- [ ] Data download and preprocessing scripts released
- [ ] Instructions to reproduce the main results provided, e.g., README
- [ ] Trained model weights and outputs (e.g., prediction maps) released where licensing allows

---

## C. Pre-trained models

### C.1 Model identification
- [ ] Model name, variant/size, and architecture reported
- [ ] Exact checkpoint identified, e.g., repository commit / Hugging Face revision
- [ ] Pretraining objective (e.g., masked reconstruction, contrastive learning) and pretraining dataset (sensors, coverage, timeframe) reported
- [ ] Weights license checked; redistribution terms documented
- [ ] For API-based models or precomputed embedding products: version and access date reported

### C.2 Input compatibility
- [ ] Band selection and band order match pretraining
- [ ] Normalization statistics stated (pretraining statistics vs. dataset statistics)
- [ ] Input resolution (GSD) and patch size relative to pretraining reported
- [ ] Handling of missing or extra bands/modalities described
- [ ] Spatial and temporal overlap between pretraining data and test set checked and reported

### C.4 Adaptation
- [ ] Adaptation strategy stated (zero-shot, frozen encoder + decoder, full fine-tuning)
- [ ] Decoder/head architecture reported
- [ ] Hyperparameters and early-stopping criteria reported
- [ ] Number of labeled training samples for fine-tuning reported
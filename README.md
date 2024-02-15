### Transform Seurat object to zarr store in R

Load required packages
```
library(Seurat)
library(Matrix)
library(vitessceR)
library(vitessceAnalysisR)
```

Transform to zarr store
```
adata_path <- file.path("data", "example", "example.h5ad.zarr")
vitessceAnalysisR::seurat_to_anndata_zarr(so, adatapath)
```

### Create Vitessce config

More examples available in the VitessceR tutorials

```
vc <- VitessceConfig$new(schema_version = "1.0.16", name = "My config")

dataset <- vc$add_dataset("My dataset")$add_object(AnnDataWrapper$new(
  adata_path=adata_path,
  obs_set_paths = c('obs/Phases', 'obs/pseudotime_bin', 'obs/Phase'),
  obs_set_names = c('Maturation phase', 'Pseudotime bins', "Cell cycle phase"),
  obs_embedding_paths = c("obsm/X_umap"),
  obs_embedding_names = c('UMAP'),
  obs_feature_matrix_path = "X",
))
```
```
# Add views
scatterplot <- vc$add_view(dataset, Component$SCATTERPLOT, mapping = "PCA")
scatterplot_UMAP <- vc$add_view(dataset, Component$SCATTERPLOT, mapping = 'UMAP')
status <- vc$add_view(dataset, Component$STATUS)
desc <- vc$add_view(dataset, Component$DESCRIPTION)
desc <- desc$set_props(description = "Visulasation based on")
genes <- vc$add_view(dataset, Component$FEATURE_LIST)
heatmap <- vc$add_view(dataset, Component$HEATMAP)
cell_sets <- vc$add_view(dataset,Component$OBS_SETS)
```
```
# Link views
vc$link_views(
  list(heatmap, scatterplot_UMAP),
  list(CoordinationType$FEATURE_VALUE_COLORMAP_RANGE),
  list(c(0.15, 0.2))
)
```
```
# Layout
vc$layout(vconcat(
  hconcat(heatmap, scatterplot_UMAP),
  hconcat(cell_sets, genes), 
  hconcat(desc)
))
```
```
# Render the Vitessce widget
vc$widget(theme = "dark")
```

### Create the GitHub repository
For where the data will be hosted

### Export Vitessce Config
```
vc$export (with_config=TRUE, base_url= '<GitHub_repository_url>', out_dir = './export_example
```

### Optional
Edit the exported config.JSON to make changes without having to export from R again. Can be tested on the Vitessce App if the .h5ad.zarr is hosted locally on an http-server.

### Clone to Github
Clone the export folder to the previously created repository. Ensure that th uploaded config.JSON has the URL that points to the .h5ad.zarr file in the repository. 

### Vitessce URL
A shareable Vitessce.io URL can now be generated

* http://vitessce.io/?url=<GitHub_reposiory_url_to_config.JSON>




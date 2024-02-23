# VitessceR Config construction

As an alternative to the Vitessce App, the config can also be created in R (or in Python) and the widgets can be launched in the environment directly. This does not have many benefits appart from the fact that it does not require the data to be served.
The steps are very similar to the JSON format:

### Configure the vitessce config:

```r
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

### Add views:

```r
scatterplot <- vc$add_view(dataset, Component$SCATTERPLOT, mapping = "PCA")
scatterplot_UMAP <- vc$add_view(dataset, Component$SCATTERPLOT, mapping = 'UMAP')
status <- vc$add_view(dataset, Component$STATUS)
desc <- vc$add_view(dataset, Component$DESCRIPTION)
desc <- desc$set_props(description = "Visulasation based on")
genes <- vc$add_view(dataset, Component$FEATURE_LIST)
heatmap <- vc$add_view(dataset, Component$HEATMAP)
cell_sets <- vc$add_view(dataset,Component$OBS_SETS)

```

### Link views:

```r
vc$link_views(
  list(heatmap, scatterplot_UMAP),
  list(CoordinationType$FEATURE_VALUE_COLORMAP_RANGE),
  list(c(0.15, 0.2))
)

```

### Configure Layout:

```r
vc$layout(vconcat(
  hconcat(heatmap, scatterplot_UMAP),
  hconcat(cell_sets, genes),
  hconcat(desc)
))

```

### Render the Vitessce widgets direclty in R:

```r
vc$widget(theme = "dark")

```

Or the config can also be exported, which then can be uploaded or copied to the Vitessce App directly. Optionally: The exported config.JSON can be edited after the export without having to export from R again.

```r
vc$export (with_config=TRUE, base_url= '<GitHub_repository_url>', out_dir = './export_zfish
```

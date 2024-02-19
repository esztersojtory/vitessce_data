# Transform Seurat object to zarr store in R

## Installation

Installation requires R 4.0.0 or greater.

```r
install.packages("devtools")
devtools::install_github("vitessce/vitessceR")
devtools::install_github("vitessce/vitessceAnalysisR")

```

## Usage

Load required packages:

```r
library(Seurat)
library(Matrix)
library(vitessceR)
library(vitessceAnalysisR)
```

Transform to zarr store:

```r
adata_path <- file.path("data", "example", "example.h5ad.zarr")
vitessceAnalysisR::seurat_to_anndata_zarr(so, adatapath)
```

## Create the GitHub repository

For where the data will be hosted.

## Clone to Github

Clone the export folder (.h5ad.zarr) to the previously created repository. 

## Setup GitHub Pages

Add a blank ```.nojekyll``` file to the root of the repository and set up GitHub Pages, from the Settings tab of the repository, from the  branch of the repository where the data was uploaded to. 

# Create Vitessce config

The config.json can be created from scratch, or by editing from a previous template/example. The Vitessce configuration will specify:

- The dataset that should be visualised
- The visualisation types of interest
- linking of visualisation parameters across views
- other design choices (colours, layout etc)

The following steps walk through the construction of the config, but it often might be easier to just edit a preexisting one. 

### Header

```
{
  "version": "1.0.16",
  "name": "Zebrafish Neutrophil Figure 3, Kirchberger and Shoeb et al. 2024",
  "initStrategy": "auto",
```

### Linking the Datasets

The url should point to the example.h5ad.zarr folder, whether it is served locally (through an http-server) or online (Ex: through GitHub Pages). 

```
  "datasets": [
    {
      "uid": "A",
      "name": "My dataset",
      "files": [
        {
          "fileType": "anndata.zarr",
          "url": "<GitHub_repository_url>/example.h5ad.zarr",
```

The options part of the file definition above is specific to the anndata-cells.zarr file type. The value for "path" must be pointing to an array in the Zarr store. 
The embeddingType coordination object is a coordination type because it allows us to coordinate which scatterplots should be displaying the same dimensionality reduction (PCA or UMAP).
The value for "dims" must be a tuple that specifies which dimensions of the dimensionality reduction array to use for the [X, Y] axes of the scatterplot.

```
          "options": {
            "obsEmbedding": [
              {
                "path": "obsm/X_umap",
                "embeddingType": "UMAP",
                "dims": [
                  0,
                  1
                ]
              }
            ],
```
"obsSets" in the options defines the metadata that we want to display on the plots. These will be listed under the "Cell Sets" widget if we add one later on. 

```
            "obsSets": [
              {
                "path": "obs/citrin_state",
                "name": "Neutrophil sorts (citrin state)"
              },
              {
                "path": "obs/ref1_neut_sub",
                "name": "Labels from Tang et al, 2017"
              },
              {
                "path": "obs/ref2_neut_sub",
                "name": "Labels from Athanasiadis et al, 2017"
              },
              {
                "path": "obs/Phase",
                "name": "Cell Cycle phase"
              }
            ],
```

The Feature matrix is automatically exported to the "X" folder from the Seurat object

```
            "obsFeatureMatrix": {
              "path": "X"
            }
          }
        }
      ]
    }
  ],
```

### Coordination space

The "coordinationSpace" is the container for all coordination objects in a visualization system. 
The "embeddingType" defines the type for the scatterplot.
The featureValueColormapRange defines the expression range for the heatmap component.
The obsSetColor allows the addition of custom colours to each obsSet variable. At the time of writing, if one colour is specified, all others will be automatically set to grey, so if one needs to be customised then all the others have to be as well. The makers of Vitessce have announced that Colour Maps are coming, where the individual colours will no longer have to be specified with an RGB value.

```
  "coordinationSpace": {
    "dataset": {
      "A": "A"
    },
    "embeddingType": {
      "A": "UMAP"
    },
    "featureValueColormapRange": {
      "A": [
        0.15,
        0.23
      ]
    },
    "obsSetColor": {
      "A": [
        {
          "path": [
            "Neutrophil sorts (citrin state)",
            "NO"
          ],
          "color": [
            248,
            225,
            222
          ]
        },
        {
          "path": [
            "Neutrophil sorts (citrin state)",
            "INT"
          ],
          "color": [
            237,
            163,
            181
          ]
        },
        {
          "path": [
            "Neutrophil sorts (citrin state)",
            "HI"
          ],
          "color": [
            204,
            68,
            148
          ]
        },
        {
          "path": [
            "Neutrophil sorts (citrin state)",
            "WKM"
          ],
          "color": [
            204,
            204,
            204
          ]
        },
        {
          "path": [
            "Labels from Tang et al, 2017",
            "neutrophils"
          ],
          "color": [
            117,
            112,
            179
          ]
        },
        {
          "path": [
            "Labels from Tang et al, 2017",
            "others"
          ],
          "color": [
            204,
            204,
            204
          ]
        },
        {
          "path": [
            "Labels from Athanasiadis et al, 2017",
            "neutrophils"
          ],
          "color": [
            117,
            112,
            179
          ]
        },
        {
          "path": [
            "Labels from Athanasiadis et al, 2017",
            "others"
          ],
          "color": [
            204,
            204,
            204
          ]
        },
        {
          "path": [
            "Cell Cycle phase",
            "G1"
          ],
          "color": [
            249,
            247,
            182
          ]
        },
        {
          "path": [
            "Cell Cycle phase",
            "S"
          ],
          "color": [
            234,
            146,
            94
          ]
        },
        {
          "path": [
            "Cell Cycle phase",
            "G2M"
          ],
          "color": [
            151,
            29,
            43
          ]
        }
      ]
    }
  },
```
### Layout

The "layout" defines the layout of the configuration. One section for each required widget. 
The canvas is 12x12, "x" and "y" define the coordinates of the left upper corner of the widget, whereas "w" and "h", the width and the height respectively.

```
  "layout": [
    {
      "component": "scatterplot",
      "coordinationScopes": {
        "dataset": "A",
        "embeddingType": "A",
        "featureValueColormapRange": "A",
        "obsSetColor": "A"
      },
      "x": 0,
      "y": 0,
      "w": 6,
      "h": 12
    },
    {
      "component": "description",
      "coordinationScopes": {
        "dataset": "A"
      },
      "x": 6,
      "y": 6,
      "w": 6,
      "h": 3,
      "props": {
        "description": "Visualisation based on Fig. 3 from Kirchberger and Shoeb et al. 2024: scRNA-seq of zebrafish neutrophils reveals a continous maturation process. UMAP of scRNA-seq data ( n=18,150 cells) showing FACS-sorted neutrophil populations (lysC+/mmp9 = NO, INT, HI) and unsorted whole kidney marrow cells (WKM). Reference-based labelling of cell types by projecting cells to two zebrafish hematopoietic reference atlases in the same UMAP. The cluster with a high concentration of G2M phase cells is dominated by cell cycle effects."
      }
    },
    {
      "component": "featureList",
      "coordinationScopes": {
        "dataset": "A",
        "obsSetColor": "A"
      },
      "x": 9,
      "y": 0,
      "w": 3,
      "h": 3
    },
    {
      "component": "obsSetSizes",
      "coordinationScopes": {
        "dataset": "A",
        "obsSetColor": "A"
      },
      "x": 6,
      "y": 0,
      "w": 6,
      "h": 6
    },
    {
      "component": "obsSets",
      "coordinationScopes": {
        "dataset": "A",
        "obsSetColor": "A"
      },
      "x": 6,
      "y": 0,
      "w": 3,
      "h": 3
    }
  ]
}
```

The configs for the datasets in this repository are in the .h5ad.zarr directory.
More example configs are available on the [Vitessce Website](http://vitessce.io/examples/), by selecting an example and clicking "Edit" in the top right corner.

## Upload JSON

Upload the JSON to the GitHub Page. Ensure that the URL points to the .h5ad.zarr directory in the GitHub pages  (user.github.io/example/example.h5ad.zarr)

# Vitessce URL
A shareable Vitessce.io URL can now be generated by following this template.

* http://vitessce.io/#?url=https://example.github.io/example/example.h5ad.zarr/config.json


# Alternatively

Alternatively, the config can also be created in R (or in Python) and the widgets can be launched in the R environment directly. This does not have many benefits appart from the fact that it does not require a local server, it can access the zarr file directly.
Either way, the steps are very similar to the JSON format:

Configure the vitessce config:

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

Add views:

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

Link views:

```r
vc$link_views(
  list(heatmap, scatterplot_UMAP),
  list(CoordinationType$FEATURE_VALUE_COLORMAP_RANGE),
  list(c(0.15, 0.2))
)
```

Configure Layout:

```r
vc$layout(vconcat(
  hconcat(heatmap, scatterplot_UMAP),
  hconcat(cell_sets, genes), 
  hconcat(desc)
))
```

Render the Vitessce widgets:

```r
vc$widget(theme = "dark")
```

Export the Vitessce config, which then can be uploaded or copied to the Vitessce App directly. Optionally: Edit the exported config.JSON to make changes without having to export from R again.

```r
vc$export (with_config=TRUE, base_url= '<GitHub_repository_url>', out_dir = './export_zfish
``` 

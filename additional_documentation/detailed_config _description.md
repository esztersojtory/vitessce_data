**The following steps walk through the construction of the config, but it often might be easier to edit/adjust a preexisting one.**

### Header

```
{
  "version": "1.0.16",
  "name": "Zebrafish Neutrophil Figure 3, Kirchberger and Shoeb et al. 2024",
  "initStrategy": "auto",

```

### Linking the Datasets

The url should point to the example.h5ad.zarr folder, whether it is served locally (through http-server) or online (through GitHub Pages).

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

The "options" part of the file definition above is specific to the anndata-cells.zarr file type. The value for "path" must be pointing to an array in the Zarr store.
The embeddingType coordination object is a coordination type as it allows us to coordinate which scatterplots should be displaying the same dimensionality reduction (PCA or UMAP).
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

"obsSets" in the options defines the metadata that is to be displayed. These will be listed under the "Cell Sets" widget.

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

The Feature matrix is automatically exported to the "X" folder from the Seurat object.

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
The obsSetColor allows the addition of custom colours to each obsSet variable. At the time of writing, if one colour is specified, all others will be automatically set to grey, so they all have to be explicitely specified.
The makers of Vitessce have communicated that a Colour Maps feature is going to be released, where the individual colours will no longer have to be specified with an RGB value, but it has not yet been implemented.

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

The "layout" defines the layout of the widgets on the 12x12 canvas.
The "x" and "y" define the coordinates of the left upper corner of the widget, whereas "w" and "h", the width and the height respectively:

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
  ],
  "initStrategy": "auto"
}
```

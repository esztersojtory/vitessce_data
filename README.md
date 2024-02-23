# In a Glimpse

This page aims to simplify the process of transforming a Seurat object to a Vitessce visualisation. 

Vitessce allows the transformation of Seurat objects into interactive visualizations. The process begins with converting the Seurat object into a zarr store, which acts as the foundation for data hosting. Users have the option to host their data on GitHub Pages or locally. Once the data is hosted, users can create a Vitessce configuration JSON file, either from scratch or by modifying a template provided by the Vitessce App. This file specifies the dataset, the visualisation types, the links between them as well as the design choices. After uploading the config JSON file to GitHub, a unique URL is generated, enabling users to access their customized Vitessce visualization. 

Starting File: Processed and Analysed Seurat Object

**Steps:**
1. Transform to Zarr store
2. Data hosting (on GitHub Pages or locally)
3. Create a config in the Vitessce App + customise as needed
4. Uploading config.JSON to GitHub to create a unique URL

Final Product: Interactive Vitessce Visualisation accessible on the web (if data is hosted publicly).

# Transform Seurat object to zarr store in R

The following instructions detail process of creating a custom Vitessce visualisation from a Seurat object.

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

Transform to zarr store (.h5ad.zarr) (making sure that the adatapath directory exists prior to running):

```r
adata_path <- file.path("data", "example", "example.h5ad.zarr")
vitessceAnalysisR::seurat_to_anndata_zarr(so, adatapath)
```

# Data Hosting

## Static Web Servers

### GitHub Pages

1. Create the GitHub repository where the data will be hosted.
2. Clone the export folder (.h5ad.zarr) to the previously created repository.
3. Add a blank ```.nojekyll``` file to the root of the repository.
4. Set up GitHub Pages, from the Settings tab, from the  branch of the repository where the data was uploaded to.

### Other Alternatives

The data can also be hosted on other static web servers such as AWS S3 buckets or Google Cloud, which could be necesary for particularly large files. However, in most cases the free GitHub Pages should work fine. The specific bucket policies for AWS and Google Cloud can be found on the [Vitessce Website](http://vitessce.io/docs/data-hosting/).

## Local Files

Local hosting allows for the visualisation of not yet published data.
To serve data locally, any web server that can serve a directory should work, but Vitessce recommend http-server:

```
brew install http-server
```
```
npm install --global http-server
```

Then, navigate to the data directory and run the server:

```
http-server ./ --cors -p 9000
```
If the data is hosted locally, the Vitessce App will not work in Chrome because of the security restrictions on accessing locally hosted files. Easiest is to use a different browser. This is no longer an issue when the files are not hosted locally.

# Create Vitessce config

The config.json can be created from scratch, or by editing a previous template/example. This Vitessce configuration will specify:

- The dataset
- The visualisation types (widgets)
- Linking of visualisation parameters across views
- Other design choices (colours, layout etc)

The Vitessce App provides basic configs. To access these, go the the [Vitessce App](http://vitessce.io/#?edit=true) and paste the URL to the .h5ad.zarr file in the textbox (as shown below). If the URL points to the correct filetype, 6 options will appear to select a view layout for the provided URL. Select one and Load from the Editor. This resulting visualisation can then be customised with the required metadata and additional widgets in the Editor. 

![Screenshot 2024-02-22 at 11 29 35](https://github.com/esztersojtory/vitessce_data/assets/156802036/e2308f37-7702-48f7-b44b-d525e31c83a9)


For customisation, I highly recommend copying from examples. The configs that I have created can be found in this repository in the .h5ad.zarr directories. More example configs are available on the [Vitessce Website](http://vitessce.io/examples/), by selecting an example and clicking "Edit" in the top right corner.

### Detailed Instructions
I have uploaded further detailed instructions for config.JSON customisation as well as for constructing a config in R. These can be found at [Additional Documentation folder](https://github.com/esztersojtory/vitessce_data/tree/main/additional_documentation)


## Upload JSON

In order to create a unique URL of the Vitessce visualisation, the config.JSON file needs to be accessible publicly. Easiest is probably the same GitHub directory where the zarr store is. Ensure that the URL in the config.JSON points to the .h5ad.zarr directory in the GitHub Page ([user.github.io/example/example.h5ad.zarr](http://user.github.io/example/example.h5ad.zarr)).

# Vitessce URL

A shareable [Vitessce.io](http://vitessce.io/) URL can now be generated by following this template.

- http://vitessce.io/#?url=https://example.github.io/example/example.h5ad.zarr/config.json

Stream Visualisation
================
Grace Heron
29/07/2020

# Stream Visualisation

Reference for some nice streams (subjective). Everything works for
R4.0.2 and proj6. Important packages:

``` r
library(rgdal)
```

    ## Loading required package: sp

    ## rgdal: version: 1.5-12, (SVN revision 1018)
    ## Geospatial Data Abstraction Library extensions to R successfully loaded
    ## Loaded GDAL runtime: GDAL 3.0.4, released 2020/01/28
    ## Path to GDAL shared files: C:/Users/Grace/Documents/R/win-library/4.0/rgdal/gdal
    ## GDAL binary built with GEOS: TRUE 
    ## Loaded PROJ runtime: Rel. 6.3.1, February 10th, 2020, [PJ_VERSION: 631]
    ## Path to PROJ shared files: C:/Users/Grace/Documents/R/win-library/4.0/rgdal/proj
    ## Linking to sp version:1.4-2
    ## To mute warnings of possible GDAL/OSR exportToProj4() degradation,
    ## use options("rgdal_show_exportToProj4_warnings"="none") before loading rgdal.

``` r
library(raster)
library(tidyverse)
```

    ## -- Attaching packages --------------------------------------------- tidyverse 1.3.0 --

    ## v ggplot2 3.3.2     v purrr   0.3.4
    ## v tibble  3.0.3     v dplyr   1.0.0
    ## v tidyr   1.1.0     v stringr 1.4.0
    ## v readr   1.3.1     v forcats 0.5.0

    ## -- Conflicts ------------------------------------------------ tidyverse_conflicts() --
    ## x tidyr::extract() masks raster::extract()
    ## x dplyr::filter()  masks stats::filter()
    ## x dplyr::lag()     masks stats::lag()
    ## x dplyr::select()  masks raster::select()

``` r
library(ggspatial)
library(ggrepel)
library(SSN)
```

    ## Loading required package: RSQLite

## Yankee-Fork Streams

``` r
sites <- readOGR("gisdata/yankeefork","sites")
```

    ## OGR data source with driver: ESRI Shapefile 
    ## Source: "C:\spmodels\streamviz\gisdata\yankeefork", layer: "sites"
    ## with 208 features
    ## It has 10 fields
    ## Integer64 fields read as strings:  VisitID VisitYr

``` r
watershed <- readOGR("gisdata/yankeefork/watershed.shp", "watershed")
```

    ## OGR data source with driver: ESRI Shapefile 
    ## Source: "C:\spmodels\streamviz\gisdata\yankeefork\watershed.shp", layer: "watershed"
    ## with 1 features
    ## It has 17 fields
    ## Integer64 fields read as strings:  OBJECTID_1 OBJECTID_2

``` r
streams <- readOGR("gisdata/yankeefork","streams")
```

    ## OGR data source with driver: ESRI Shapefile 
    ## Source: "C:\spmodels\streamviz\gisdata\yankeefork", layer: "streams"
    ## with 276 features
    ## It has 32 fields
    ## Integer64 fields read as strings:  COMID FCODE DUP_COMID StreamOrde reachID rid netID

``` r
ggplot() +
  layer_spatial(watershed, fill = "gray60", alpha = 0.5, colour = "gray40") +
  layer_spatial(streams, aes(size = afvArea), colour = "gray50") +
  layer_spatial(sites, colour = "coral", pch = 1, cex = 2, show.legend = TRUE)+
  coord_sf() + 
  labs(x = "Longitude", y = "Latitude") + 
  annotation_north_arrow(height = unit(0.3, "in"),
                         width = unit(0.3, "in"), # GRID NORTH? OR TRUE NORTH?
                         which_north = "grid", 
                         location = "bl") +
  scale_size(range = c(0, 2), guide = FALSE) +
  scale_shape_manual(values=c(25, 24)) + 
  theme_bw() + 
  theme(legend.title = element_blank(),
        axis.title.y=element_blank(),
        axis.title.x=element_blank(),
        axis.text.y = element_text())
```

![](streamviz_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

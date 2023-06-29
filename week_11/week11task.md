---
title: "Week 11: Getting into SHP"
author: "Maddy McGreer"
date: "June 29, 2023"
output:
  html_document:  
    keep_md: true
    toc: true
    toc_float: true
    code_folding: hide
    fig_height: 6
    fig_width: 12
    fig_align: 'center'
---






```r
# Use this R-Chunk to import all your datasets!

wells <- st_read("/Users/maddy/Downloads/Wells/Wells.shp")
```

```
## Reading layer `Wells' from data source `C:\Users\maddy\Downloads\Wells\Wells.shp' using driver `ESRI Shapefile'
## Simple feature collection with 195091 features and 33 fields
## Geometry type: POINT
## Dimension:     XY
## Bounding box:  xmin: -117.3642 ymin: 41.02696 xmax: -111.0131 ymax: 49.00021
## Geodetic CRS:  WGS 84
```

```r
dams <- st_read("/Users/maddy/Downloads/Idaho_Dams/Dam_Safety.shp")
```

```
## Reading layer `Dam_Safety' from data source 
##   `C:\Users\maddy\Downloads\Idaho_Dams\Dam_Safety.shp' using driver `ESRI Shapefile'
## Simple feature collection with 1127 features and 23 fields
## Geometry type: POINT
## Dimension:     XY
## Bounding box:  xmin: -117.0866 ymin: 42.00058 xmax: -111.0725 ymax: 48.95203
## Geodetic CRS:  WGS 84
```

```r
water <- st_read("/Users/maddy/Downloads/water/hyd250.shp")
```

```
## Reading layer `hyd250' from data source 
##   `C:\Users\maddy\Downloads\water\hyd250.shp' using driver `ESRI Shapefile'
## Simple feature collection with 30050 features and 26 fields
## Geometry type: LINESTRING
## Dimension:     XY
## Bounding box:  xmin: 2241685 ymin: 1198722 xmax: 2743850 ymax: 1981814
## Projected CRS: NAD83 / Idaho Transverse Mercator
```

```r
shp <- st_read("/Users/maddy/Downloads/shp/County-AK-HI-Moved-USA-Map.shp")
```

```
## Reading layer `County-AK-HI-Moved-USA-Map' from data source 
##   `C:\Users\maddy\Downloads\shp\County-AK-HI-Moved-USA-Map.shp' 
##   using driver `ESRI Shapefile'
## Simple feature collection with 3115 features and 15 fields
## Geometry type: MULTIPOLYGON
## Dimension:     XY
## Bounding box:  xmin: -2573301 ymin: -1889441 xmax: 2256474 ymax: 1565782
## Projected CRS: Albers
```

## Background

_Map Snake River and Henry's Fork water sources in Idaho_

## Data Wrangling


```r
# Use this R-Chunk to clean & wrangle your data!
# Filter wells dataset
filtered_wells <- wells[wells$production > 5000, ]

# Filter dams dataset
filtered_dams <- dams[dams$SurfaceAre> 50, ]

# Filter water bodies dataset
filtered_water <- water[water$FEAT_NAME %in% c("Snake River", "Henrys Fork"), ]

filtered_states <- shp[shp$StateName %in% c("Idaho"), ]
```

## Data Visualization


```r
# Use this R-Chunk to plot & visualize your data!
idaho_map <- ggplot() +
  geom_sf(data = filtered_states, fill = "lightgray", color = "white") +
  geom_sf(data = filtered_water, fill = "blue") +
  geom_sf(data = filtered_dams, fill = "red") +  
  geom_sf(data = filtered_wells, fill = "blue") +  
  labs(title = "Water System Features in Idaho")

print(idaho_map)
```

![](week11task_files/figure-html/plot_data-1.png)<!-- -->

```r
ggsave("idaho_map.png", plot = idaho_map, width = 10, height = 8)
```

## Conclusions

_Shows water sources in Idaho for Snake River and Henry's Fork._

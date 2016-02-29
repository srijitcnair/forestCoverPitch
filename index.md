---
title       : Predicting Predominant Forest Cover
subtitle    : Roosevelt National Forest, Northern Colorado
author      : Srijit Chandrashekhar Nair 
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Overview

This application predicts the forest cover type of a wilderness area in the Roosevelt National Park, northern Colorado.It is a prediction using Cartographic Variables (not based on satelite imagery) and is based on the parameters that you select in "Input Values" section in the left side panel.

This is based on the Kaggle competition "Forest Cover Type Prediction" (https://www.kaggle.com/c/forest-cover-type-prediction)

--- .class #id 

## Inputs

The input data includes:
 * Elevation
 * Aspect
 * Slope
 * Horizontal Distance To Hydrology
 * Vertical Distance To Hydrology
 * Horizontal Distance To Roadways
 * Hillshade 9am (0 to 255 index)
 * Hillshade Noon (0 to 255 index)
 * Hillshade 3pm (0 to 255 index)
 * Horizontal Distance To Fire Points
 * Wilderness Area
 * Soil Type

--- .class #id 

## Output

Output predicts the type of forest cover which will be one of

 * Spruce/Fir
 * Lodgepole Pine
 * Ponderosa Pine
 * Cottonwood/Willow
 * Aspen
 * Douglas-fir
 * Krummholz

--- .class #id 

## How it works

This program loads the Kaggle training set from https://www.kaggle.com/c/forest-cover-type-prediction/data and makes Gradient Bost model using Caret library.

Thanks to the R script by Apurva Dubey at Kaggle (https://www.kaggle.com/apurvadubey/forest-cover-type-prediction/r-starter-code), a modified version of which was used for this product. 

A sample from the training set is shown below


```
## 'data.frame':	15120 obs. of  56 variables:
##  $ Id                                : int  1 2 3 4 5 6 7 8 9 10 ...
##  $ Elevation                         : int  2596 2590 2804 2785 2595 2579 2606 2605 2617 2612 ...
##  $ Aspect                            : int  51 56 139 155 45 132 45 49 45 59 ...
##  $ Slope                             : int  3 2 9 18 2 6 7 4 9 10 ...
##  $ Horizontal_Distance_To_Hydrology  : int  258 212 268 242 153 300 270 234 240 247 ...
##  $ Vertical_Distance_To_Hydrology    : int  0 -6 65 118 -1 -15 5 7 56 11 ...
##  $ Horizontal_Distance_To_Roadways   : int  510 390 3180 3090 391 67 633 573 666 636 ...
##  $ Hillshade_9am                     : int  221 220 234 238 220 230 222 222 223 228 ...
##  $ Hillshade_Noon                    : int  232 235 238 238 234 237 225 230 221 219 ...
##  $ Hillshade_3pm                     : int  148 151 135 122 150 140 138 144 133 124 ...
##  $ Horizontal_Distance_To_Fire_Points: int  6279 6225 6121 6211 6172 6031 6256 6228 6244 6230 ...
##  $ Wilderness_Area1                  : int  1 1 1 1 1 1 1 1 1 1 ...
##  $ Wilderness_Area2                  : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Wilderness_Area3                  : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Wilderness_Area4                  : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Soil_Type1                        : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Soil_Type2                        : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Soil_Type3                        : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Soil_Type4                        : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Soil_Type5                        : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Soil_Type6                        : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Soil_Type7                        : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Soil_Type8                        : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Soil_Type9                        : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Soil_Type10                       : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Soil_Type11                       : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Soil_Type12                       : int  0 0 1 0 0 0 0 0 0 0 ...
##  $ Soil_Type13                       : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Soil_Type14                       : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Soil_Type15                       : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Soil_Type16                       : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Soil_Type17                       : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Soil_Type18                       : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Soil_Type19                       : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Soil_Type20                       : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Soil_Type21                       : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Soil_Type22                       : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Soil_Type23                       : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Soil_Type24                       : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Soil_Type25                       : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Soil_Type26                       : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Soil_Type27                       : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Soil_Type28                       : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Soil_Type29                       : int  1 1 0 0 1 1 1 1 1 1 ...
##  $ Soil_Type30                       : int  0 0 0 1 0 0 0 0 0 0 ...
##  $ Soil_Type31                       : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Soil_Type32                       : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Soil_Type33                       : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Soil_Type34                       : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Soil_Type35                       : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Soil_Type36                       : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Soil_Type37                       : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Soil_Type38                       : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Soil_Type39                       : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Soil_Type40                       : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Cover_Type                        : int  5 5 2 2 5 2 5 5 5 5 ...
```

--- .class #id

## How to use

Access the application here https://srijitcnair.shinyapps.io/forestCover/ and use the documentation from https://srijitcnair.shinyapps.io/aboutForestCover/ 

Thank you !
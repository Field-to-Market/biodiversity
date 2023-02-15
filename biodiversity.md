Biodiversity Metrics in Ag Landscapes
================

## Import National Landcover Data (NLCD)

``` r
library(tidyverse)
library(landscapemetrics)     # landscape metrics calculation
library(raster)               # spatial raster data reading and handling
library(gt)

wash <- raster("data/landcover_wa.tiff") 
iowa <- raster("data/landcover_ia.tiff") 
georgia <- raster("data/landcover_ga.tiff") 
kansas <- raster("data/landcover_ks.tiff")
```

Let’s look at the four farms in this example.

``` r
par(mfrow = c(2,2))
plot(wash)
title("WA")
plot(iowa)
title("IA")
plot(kansas)
title("KS")
plot(georgia)
title("GA")
```

![](biodiversity_files/figure-commonmark/unnamed-chunk-2-1.png)

## Shannon’s diversity index

It is a widely used metric in biodiversity and ecology and takes both
the number of classes and the abundance of each class into account.

$$SHDI = − \sum_{i = 1}^m ​P_i​ \times\ \ln(P_i​)$$

where $P_i$ is the proportion of class $i$, and $m$ is the number of
classes.

``` r
map(c(wash, kansas, georgia, iowa), lsm_l_shdi) |> 
  list_rbind() |> 
  mutate(farm = c("wash", "kansas", "georgia", "iowa")) |> 
  gt()
```

<div id="grfjbebeag" style="padding-left:0px;padding-right:0px;padding-top:10px;padding-bottom:10px;overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>html {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Helvetica Neue', 'Fira Sans', 'Droid Sans', Arial, sans-serif;
}

#grfjbebeag .gt_table {
  display: table;
  border-collapse: collapse;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 16px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: auto;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#grfjbebeag .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#grfjbebeag .gt_caption {
  padding-top: 4px;
  padding-bottom: 4px;
}

#grfjbebeag .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#grfjbebeag .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 0;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#grfjbebeag .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#grfjbebeag .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#grfjbebeag .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#grfjbebeag .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#grfjbebeag .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#grfjbebeag .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#grfjbebeag .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 5px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#grfjbebeag .gt_group_heading {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  text-align: left;
}

#grfjbebeag .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#grfjbebeag .gt_from_md > :first-child {
  margin-top: 0;
}

#grfjbebeag .gt_from_md > :last-child {
  margin-bottom: 0;
}

#grfjbebeag .gt_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#grfjbebeag .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
}

#grfjbebeag .gt_stub_row_group {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
  vertical-align: top;
}

#grfjbebeag .gt_row_group_first td {
  border-top-width: 2px;
}

#grfjbebeag .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#grfjbebeag .gt_first_summary_row {
  border-top-style: solid;
  border-top-color: #D3D3D3;
}

#grfjbebeag .gt_first_summary_row.thick {
  border-top-width: 2px;
}

#grfjbebeag .gt_last_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#grfjbebeag .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#grfjbebeag .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#grfjbebeag .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#grfjbebeag .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#grfjbebeag .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#grfjbebeag .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding-left: 4px;
  padding-right: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#grfjbebeag .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#grfjbebeag .gt_sourcenote {
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#grfjbebeag .gt_left {
  text-align: left;
}

#grfjbebeag .gt_center {
  text-align: center;
}

#grfjbebeag .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#grfjbebeag .gt_font_normal {
  font-weight: normal;
}

#grfjbebeag .gt_font_bold {
  font-weight: bold;
}

#grfjbebeag .gt_font_italic {
  font-style: italic;
}

#grfjbebeag .gt_super {
  font-size: 65%;
}

#grfjbebeag .gt_footnote_marks {
  font-style: italic;
  font-weight: normal;
  font-size: 75%;
  vertical-align: 0.4em;
}

#grfjbebeag .gt_asterisk {
  font-size: 100%;
  vertical-align: 0;
}

#grfjbebeag .gt_indent_1 {
  text-indent: 5px;
}

#grfjbebeag .gt_indent_2 {
  text-indent: 10px;
}

#grfjbebeag .gt_indent_3 {
  text-indent: 15px;
}

#grfjbebeag .gt_indent_4 {
  text-indent: 20px;
}

#grfjbebeag .gt_indent_5 {
  text-indent: 25px;
}
</style>

| layer | level     | class | id  | metric | value     | farm    |
|-------|-----------|-------|-----|--------|-----------|---------|
| 1     | landscape | NA    | NA  | shdi   | 0.2814261 | wash    |
| 1     | landscape | NA    | NA  | shdi   | 0.7569519 | kansas  |
| 1     | landscape | NA    | NA  | shdi   | 1.9066371 | georgia |
| 1     | landscape | NA    | NA  | shdi   | 0.8802212 | iowa    |

Shannon’s diversity index

</div>

## Shannon’s evenness index

SHEI is a ‘Diversity metric’. It is the ratio between the actual
Shannon’s diversity index and and the theoretical maximum of the Shannon
diversity index. It can be understood as a measure of dominance.

$$SHEI = \frac{− \sum \limits_{i = 1}^m ​P_i​ \times\ \ln(P_i​)}{\ln(m)}$$

where $P_i$ is the proportion of class $i$, and $m$ is the number of
classes.

``` r
map(c(wash, kansas, georgia, iowa), lsm_l_shei) |> 
  list_rbind() |> 
  mutate(farm = c("wash", "kansas", "georgia", "iowa")) |> 
  gt()
```

<div id="mtsojbmyyd" style="padding-left:0px;padding-right:0px;padding-top:10px;padding-bottom:10px;overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>html {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Helvetica Neue', 'Fira Sans', 'Droid Sans', Arial, sans-serif;
}

#mtsojbmyyd .gt_table {
  display: table;
  border-collapse: collapse;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 16px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: auto;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#mtsojbmyyd .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#mtsojbmyyd .gt_caption {
  padding-top: 4px;
  padding-bottom: 4px;
}

#mtsojbmyyd .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#mtsojbmyyd .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 0;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#mtsojbmyyd .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#mtsojbmyyd .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#mtsojbmyyd .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#mtsojbmyyd .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#mtsojbmyyd .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#mtsojbmyyd .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#mtsojbmyyd .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 5px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#mtsojbmyyd .gt_group_heading {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  text-align: left;
}

#mtsojbmyyd .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#mtsojbmyyd .gt_from_md > :first-child {
  margin-top: 0;
}

#mtsojbmyyd .gt_from_md > :last-child {
  margin-bottom: 0;
}

#mtsojbmyyd .gt_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#mtsojbmyyd .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
}

#mtsojbmyyd .gt_stub_row_group {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
  vertical-align: top;
}

#mtsojbmyyd .gt_row_group_first td {
  border-top-width: 2px;
}

#mtsojbmyyd .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#mtsojbmyyd .gt_first_summary_row {
  border-top-style: solid;
  border-top-color: #D3D3D3;
}

#mtsojbmyyd .gt_first_summary_row.thick {
  border-top-width: 2px;
}

#mtsojbmyyd .gt_last_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#mtsojbmyyd .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#mtsojbmyyd .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#mtsojbmyyd .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#mtsojbmyyd .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#mtsojbmyyd .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#mtsojbmyyd .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding-left: 4px;
  padding-right: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#mtsojbmyyd .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#mtsojbmyyd .gt_sourcenote {
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#mtsojbmyyd .gt_left {
  text-align: left;
}

#mtsojbmyyd .gt_center {
  text-align: center;
}

#mtsojbmyyd .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#mtsojbmyyd .gt_font_normal {
  font-weight: normal;
}

#mtsojbmyyd .gt_font_bold {
  font-weight: bold;
}

#mtsojbmyyd .gt_font_italic {
  font-style: italic;
}

#mtsojbmyyd .gt_super {
  font-size: 65%;
}

#mtsojbmyyd .gt_footnote_marks {
  font-style: italic;
  font-weight: normal;
  font-size: 75%;
  vertical-align: 0.4em;
}

#mtsojbmyyd .gt_asterisk {
  font-size: 100%;
  vertical-align: 0;
}

#mtsojbmyyd .gt_indent_1 {
  text-indent: 5px;
}

#mtsojbmyyd .gt_indent_2 {
  text-indent: 10px;
}

#mtsojbmyyd .gt_indent_3 {
  text-indent: 15px;
}

#mtsojbmyyd .gt_indent_4 {
  text-indent: 20px;
}

#mtsojbmyyd .gt_indent_5 {
  text-indent: 25px;
}
</style>

| layer | level     | class | id  | metric | value     | farm    |
|-------|-----------|-------|-----|--------|-----------|---------|
| 1     | landscape | NA    | NA  | shei   | 0.1446244 | wash    |
| 1     | landscape | NA    | NA  | shei   | 0.3445037 | kansas  |
| 1     | landscape | NA    | NA  | shei   | 0.7040627 | georgia |
| 1     | landscape | NA    | NA  | shei   | 0.3431729 | iowa    |

Shannon’s evenness index

</div>

## Simpson’s diversity index

SIDI is a ‘Diversity metric’. It is widely used in biodiversity and
ecology. It is less sensitive to rare class types than Shannon’s
diversity index. It can be interpreted as the probability that two
randomly selected cells belong to the same class.

$$SIDI = 1 − \sum_{i = 1}^m ​P_i^2$$

``` r
map(c(wash, kansas, georgia, iowa), lsm_l_sidi) |> 
  list_rbind() |> 
  mutate(farm = c("wash", "kansas", "georgia", "iowa")) |> 
  gt()
```

<div id="zsijcmjapj" style="padding-left:0px;padding-right:0px;padding-top:10px;padding-bottom:10px;overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>html {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Helvetica Neue', 'Fira Sans', 'Droid Sans', Arial, sans-serif;
}

#zsijcmjapj .gt_table {
  display: table;
  border-collapse: collapse;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 16px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: auto;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#zsijcmjapj .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#zsijcmjapj .gt_caption {
  padding-top: 4px;
  padding-bottom: 4px;
}

#zsijcmjapj .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#zsijcmjapj .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 0;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#zsijcmjapj .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#zsijcmjapj .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#zsijcmjapj .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#zsijcmjapj .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#zsijcmjapj .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#zsijcmjapj .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#zsijcmjapj .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 5px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#zsijcmjapj .gt_group_heading {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  text-align: left;
}

#zsijcmjapj .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#zsijcmjapj .gt_from_md > :first-child {
  margin-top: 0;
}

#zsijcmjapj .gt_from_md > :last-child {
  margin-bottom: 0;
}

#zsijcmjapj .gt_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#zsijcmjapj .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
}

#zsijcmjapj .gt_stub_row_group {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
  vertical-align: top;
}

#zsijcmjapj .gt_row_group_first td {
  border-top-width: 2px;
}

#zsijcmjapj .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#zsijcmjapj .gt_first_summary_row {
  border-top-style: solid;
  border-top-color: #D3D3D3;
}

#zsijcmjapj .gt_first_summary_row.thick {
  border-top-width: 2px;
}

#zsijcmjapj .gt_last_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#zsijcmjapj .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#zsijcmjapj .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#zsijcmjapj .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#zsijcmjapj .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#zsijcmjapj .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#zsijcmjapj .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding-left: 4px;
  padding-right: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#zsijcmjapj .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#zsijcmjapj .gt_sourcenote {
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#zsijcmjapj .gt_left {
  text-align: left;
}

#zsijcmjapj .gt_center {
  text-align: center;
}

#zsijcmjapj .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#zsijcmjapj .gt_font_normal {
  font-weight: normal;
}

#zsijcmjapj .gt_font_bold {
  font-weight: bold;
}

#zsijcmjapj .gt_font_italic {
  font-style: italic;
}

#zsijcmjapj .gt_super {
  font-size: 65%;
}

#zsijcmjapj .gt_footnote_marks {
  font-style: italic;
  font-weight: normal;
  font-size: 75%;
  vertical-align: 0.4em;
}

#zsijcmjapj .gt_asterisk {
  font-size: 100%;
  vertical-align: 0;
}

#zsijcmjapj .gt_indent_1 {
  text-indent: 5px;
}

#zsijcmjapj .gt_indent_2 {
  text-indent: 10px;
}

#zsijcmjapj .gt_indent_3 {
  text-indent: 15px;
}

#zsijcmjapj .gt_indent_4 {
  text-indent: 20px;
}

#zsijcmjapj .gt_indent_5 {
  text-indent: 25px;
}
</style>

| layer | level     | class | id  | metric | value     | farm    |
|-------|-----------|-------|-----|--------|-----------|---------|
| 1     | landscape | NA    | NA  | sidi   | 0.1083380 | wash    |
| 1     | landscape | NA    | NA  | sidi   | 0.4030255 | kansas  |
| 1     | landscape | NA    | NA  | sidi   | 0.8042571 | georgia |
| 1     | landscape | NA    | NA  | sidi   | 0.3977780 | iowa    |

Simpson’s diversity index

</div>

### References

Hesselbarth, M.H.K., Sciaini, M., With, K.A., Wiegand, K., Nowosad, J.
2019. landscapemetrics: an open-source R tool to calculate landscape
metrics. Ecography 42:1648-1657(ver. 0).

McGarigal, K., SA Cushman, and E Ene. 2012. FRAGSTATS v4: Spatial
Pattern Analysis Program for Categorical and Continuous Maps. Computer
software program produced by the authors at the University of
Massachusetts, Amherst. Available at the following web site:
https://www.umass.edu/landeco/

Shannon, C., and W. Weaver. 1949. The mathematical theory of
communication. Univ. IllinoisPress, Urbana

Simpson, E. H. 1949. Measurement of diversity. Nature 163:688

## Other things to explore

- [this
  link](https://smalltownbigdata.github.io/feb2021-landcover/feb2021-landcover.html)
- [this
  link](https://gatesdupontvignettes.com/2019/06/16/nlcd-velox-dplyr.html)
- and [this
  link](https://jakubnowosad.com/posts/2022-02-17-lsm-bp3/#reading-the-data)

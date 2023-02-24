Biodiversity Metrics in Ag Landscapes
================

The Semi-Natural Habitat (SNH) composite score is

$$SNH = (CP + Veg) \times{(1 + SizeRatio) + Configuration + Management}$$

where  
$CP$ = Conservation practice base score,  
$Veg$ = Vegetative diversity score

therefore, the proposed biodiversity metric would be

$$
New\ Metric = (SNH + CoverCrops + Tillage) \times{LC} + PM + Rotation - Conversion
$$

where  
LC = Landscape Complexity score ([Examples of landscape complexity
calculations](#examples-of-landscape-complexity-calculations))  
PM = Pest Management score

To explore the point values and the calculations under different farming
scenarios, please explore these spreadsheets and the accompanying
sensitivity analysis:

- [Scenario 1](scenario/scenario-01.xlsx)

- [Scenario 2](scenario/scenario-02.xlsx)

- [Scenario 3](scenario/scenario-03.xlsx)

## Comments from reviewers

> Overall I must applaud your team on the shift in focus from the
> previous document. It is very impressive and I believe a much more
> robust framework. However it does open up some doors that are
> complicated to walk through and some that are very difficult to close.

> Could we call it Biodiversity Potential Index or Conservation
> Potential Index?
>
> I would advise moving away from the term Habitat. Your focus is on
> Biodiversity and you do not need to invoke ‘habitat’ to get there.  
>   
> Technically there is no such thing as ‘wildlife habitat’. There is
> quail habitat, rabbit habitat, or deer habitat.

> I think your metric should be driven by semi-natural vegetation.
>
> The literature is abundantly clear that the amount of non-crop
> vegetation is what is driving biodiversity in ag landscapes.

> It is only valuable to differentiate between cover types if you are
> going to assign different values to different cover types. That is a
> noble endeavor but will present some challenges because different
> cover types may be more important in different parts of the country.

> Be careful with Rangeland Analysis Platform as it is not as good east
> of the Mississippi river as it is west of it.

> “edge” is double-edged sword in wildlife biodiversity.

> if grower is operating an hot bed of biodiversity already, then the
> conservation-friendly actions they take will have a larger impact,
> depending on what metric is in question. However, if a VERY
> conservation-friendly farmers is operating in an extremely bio-hostile
> landsdape, then their actions will have considerably less impact on
> the landscape. … In other words the landscape context DOES matter, and
> I support quantifying it, BUT how that information is used \[in the
> calculation\] is unclear.

## Concerns and questions for reviewers

- This metrics revision still does not capture the value of the habitat
  creation from a flooded rice field
- What can be categorized as a cover crop?
  - Could we consider using months of a ***living/rooted*** land cover
    and months/quality of a ***residue*** cover
- Does this revision represent an overall improvement according to the
  available science
- Should we or how would we track temporal changes in the diversity
  index?
  - Farm-by-farm analysis could be more data heavy, so another option is
    county based reporting with FSA
- What should the point values be in each section of the calculation,
  for different practices?
- How should we present a biodiversity result?
  - Quantitative (e.g., 0.76, 1.21)

  - Qualitative (e.g., low, moderate)

## Examples of landscape complexity calculations

We could consider using a landscape complexity score as a sort of
multiplier in the overall equation. Here are some examples:

### Import National Landcover Data (NLCD)

``` r
library(tidyverse)
library(landscapemetrics) # landscape metrics calculation
library(raster)           # spatial raster data reading and handling
library(knitr)            # markdown tables

wash <- raster("data/landcover_wa.tiff") 
iowa <- raster("data/landcover_ia.tiff") 
georgia <- raster("data/landcover_ga.tiff") 
kansas <- raster("data/landcover_ks.tiff")
```

Let’s look at the four farms in this example.

<details>
<summary>Code</summary>

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

</details>

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
  kable(digits = 2)
```

| layer | level     | class |  id | metric | value | farm    |
|------:|:----------|------:|----:|:-------|------:|:--------|
|     1 | landscape |    NA |  NA | shdi   |  0.28 | wash    |
|     1 | landscape |    NA |  NA | shdi   |  0.76 | kansas  |
|     1 | landscape |    NA |  NA | shdi   |  1.91 | georgia |
|     1 | landscape |    NA |  NA | shdi   |  0.88 | iowa    |

Table 1 Shannon’s diversity index

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
  kable(digits = 2)
```

| layer | level     | class |  id | metric | value | farm    |
|------:|:----------|------:|----:|:-------|------:|:--------|
|     1 | landscape |    NA |  NA | shei   |  0.14 | wash    |
|     1 | landscape |    NA |  NA | shei   |  0.34 | kansas  |
|     1 | landscape |    NA |  NA | shei   |  0.70 | georgia |
|     1 | landscape |    NA |  NA | shei   |  0.34 | iowa    |

Table 2 Shannon’s evenness index

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
  kable(digits = 2)
```

| layer | level     | class |  id | metric | value | farm    |
|------:|:----------|------:|----:|:-------|------:|:--------|
|     1 | landscape |    NA |  NA | sidi   |  0.11 | wash    |
|     1 | landscape |    NA |  NA | sidi   |  0.40 | kansas  |
|     1 | landscape |    NA |  NA | sidi   |  0.80 | georgia |
|     1 | landscape |    NA |  NA | sidi   |  0.40 | iowa    |

Table 3 Simpson’s diversity index

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

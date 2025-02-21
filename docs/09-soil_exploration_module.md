---
editor_options: 
  markdown: 
    wrap: 72
---

# (PART\*) Student Activity {.unnumbered}



# Download

Coming soon!

# Introduction

In this activity, you'll have a chance to become familiar with the BioDIGS soil testing data. This dataset includes information on the inorganic components of each soil sample, particularly metal concentrations. Human activity can increase the concentration of inorganic compounds in the soil. When cars drive on roads, compounds from the exhaust, oil, and other fluids might settle onto the roads and be washed into the soil. When we put salt on roads, parking lots, and sidewalks, the salts themselves will eventually be washed away and enter the ecosystem through both water and soil. Chemicals from factories and other businesses also leech into our environment. All of this means the concentration of heavy metals and other chemicals will vary among the soil samples collected for the BioDIGS project.

## Before You Start

This activity requires RStudio. Make sure you have access to a working version of this software.


## Objectives

This activity will teach you how to use the AnVIL platform to:

1.  Open data from an R package
2.  Examine objects in R
3.  Calculate summary statistics for variables in the soil testing data
4.  Create and interpret histograms and boxplots for variables in the soil testing data

# Part 1. Examining the Data

We will use the `BioDIGSData` package to retrieve the data. We first need to install the package from where it is stored on GitHub.

:::dictionary
**Packages and libraries**

You might see or hear the term "package" or "library" when working with the R programming language. 

Packages are collections of R functions, data, and documentation that extend the base functionality of R. They are the fundamental units of shareable code in R. Packages are developed by the R community and made available through repositories like CRAN (Comprehensive R Archive Network), Bioconductor, and GitHub. When you install a package, you gain access to all the functions, data, and documentation provided by that package.

Libraries are the directories where packages are stored.

We also use the `library` command to load and attach packages to the R environment. When you load a package using `library(package_name)`, you make the functions and objects from a package available for use in your current R session.

:::


You may first need to install the `remotes` package to your RStudio environment. `remotes` is a package (or chunk of pre-written code) that allows you to download code that has been stored on GitHub into RStudio.



``` r
install.packages("remotes")
remotes::install_github("fhdsl/BioDIGSData", upgrade = "never")
```

If you're having trouble with the code above, you can also try:


``` r
install.packages("remotes")
remotes::install_url('https://github.com/fhdsl/BioDIGSData/archive/refs/tags/v1.0.0.0.tar.gz')
```

Once you've installed the package, we can load the package (which just means we have access to all the data stored in the BioDIGSData package). Then we assign the soil testing data to an *object*. This command follows the code structure:

dataset_object_name <- stored_BioDIGS_dataset

The "dataset_object_name" is what RStudio will call the dataset after you open it. The "stored_BioDIGS_dataset" is what the dataset is called within the BioDIGSData package. Finally, the arrow ("\<-") tells R to open the "stored_BioDIGS_dataset" and save it in your environment as "dataset_object_name".

The order of these commands might be odd to our eyes, but it makes perfect sense to RStudio!

The soil testing data is called `BioDIGS_soil_data` in the BioDIGSData package. When we save this dataset into our environment, we're calling is `soil.values`.


``` r
library(BioDIGSData)

soil.values <- BioDIGS_soil_data()
```

It *seems* like the dataset loaded, but it's always a good idea to verify. There are many ways to check, but the easiest approach (if you're using RStudio) is to look at the Environment tab on the upper right-hand side of the screen. You should now have an object called `soil.values` that includes some number of observations for 28 variables. The *observations* refer to the number of rows in the dataset, while the *variables* tell you the number of columns. As long as neither the observations or variables are 0, you can be confident that your dataset loaded.

<img src="resources/images/09-soil_exploration_module_files/figure-html//1u2CIcN2AxprMbWLzAldr_V-njdvjCS8HLYjvuy6jmfs_g33497bd5a49_0_9.png" alt="If the dataset loaded, you will see an object with non-zero observations and variables in the Environment tab." width="100%" style="display: block; margin: auto;" />

Let's take a quick look at the dataset. We can do this by clicking on soil.values object in the Environment tab. (Note: this is equivalent to typing `View(soil.values)` in the R console.)

This will open a new window for us to scroll through the dataset.

<img src="resources/images/09-soil_exploration_module_files/figure-html//1u2CIcN2AxprMbWLzAldr_V-njdvjCS8HLYjvuy6jmfs_g33497bd5a49_0_12.png" alt="You can click on the object in the Environment tab to open a new window that allows you to scroll through the loaded dataset." width="100%" style="display: block; margin: auto;" />

Well, the data definitely loaded, but those column names aren't immediately understandable. What could **As_EPA3051** possibly mean? In addition to the dataset, we need to load the *data dictionary* as well.

::: dictionary
**Data dictionary:** a file containing the names, definitions, and attributes about data in a database or dataset.
:::

In this case, the data dictionary can help us make sense of what sort of values each column represents. The data dictionary for the BioDIGS soil testing data is available in the R package (see code below), but we have also reproduced it here.


``` r
?BioDIGS_soil_data()
```

<img src="resources/images/09-soil_exploration_module_files/figure-html//1u2CIcN2AxprMbWLzAldr_V-njdvjCS8HLYjvuy6jmfs_g33497bd5a49_0_15.png" alt="The data dictionary shows up under the Help tab." width="100%" style="display: block; margin: auto;" />

::: dictionary
-  **collection_date**: Date sample was collected (soil was removed from a site).
-  **site_id**: Unique letter and number site name. Check BioDIGS_metadata() for GPS coordinates, origin, and more.
-  **sample_id**: Unique sequencing sample identifier.
-  **site_name_rep_detail**: Detailed label for the sample, intended to help disambiguate in case of confusion.
-  **As_EPA3051**: Arsenic (mg/kg), EPA Method 3051A. Quantities < 3.0 are not detectable.
-  **Cd_EPA3051**: Cadmium (mg/kg), EPA Method 3051A. Quantities < 0.2 are not detectable.
-  **Cr_EPA3051**: Chromium (mg/kg), EPA Method 3051A
-  **Cu_EPA3051**: Copper (mg/kg), EPA Method 3051A
-  **Ni_EPA3051**: Nickel (mg/kg), EPA Method 3051A
-  **Pb_EPA3051**: Lead (mg/kg), EPA Method 3051A
-  **Zn_EPA3051**: Zinc (mg/kg), EPA Method 3051A
-  **water_pH**: Water pH
-  **OM_by_LOI_pct**: Organic Matter by Loss on Ignition
-  **P_Mehlich3**: Phosphorus (mg/kg), using the Mehlich 3 soil test extractant
-  **K_Mehlich3**: Potassium (mg/kg), using the Mehlich 3 soil test extractant
-  **Ca_Mehlich3**: Calcium (mg/kg), using the Mehlich 3 soil test extractant
-  **Mg_Mehlich3**: Magnesium (mg/kg), using the Mehlich 3 soil test extractant
-  **Mn_Mehlich3**: Manganese (mg/kg), using the Mehlich 3 soil test extractant
-  **Zn_Mehlich3**: Zinc (mg/kg), using the Mehlich 3 soil test extractant
-  **Cu_Mehlich3**: Copper (mg/kg), using the Mehlich 3 soil test extractant
-  **Fe_Mehlich3**: Iron (mg/kg), using the Mehlich 3 soil test extractant
-  **B_Mehlich3**: Boron (mg/kg), using the Mehlich 3 soil test extractant
-  **S_Mehlich3**: Sulfur (mg/kg), using the Mehlich 3 soil test extractant
-  **Na_Mehlich3**: Sodium (mg/kg), using the Mehlich 3 soil test extractant
-  **Al_Mehlich3**: Aluminum (mg/kg), using the Mehlich 3 soil test extractant
-  **Est_CEC**: Cation Exchange Capacity (meq/100g) at pH 7.0 (CEC)
-  **Base_Sat_pct**: Base saturation (BS). This represents the percentage of CEC occupied by bases (Ca2+, Mg2+, K+, and Na+). The %BS increases with increasing soil pH (Figure 5). The availability of Ca2+, Mg2+, and K+ increases with increasing %BS.
-  **P_Sat_ratio**: Phosphorus saturation ratio. This is the ratio between the amount of phosphorus present in the soil and the total capacity of that soil to retain phosphorus. The ability of phosphorus to be bound in the soil is primary a function of iron (Fe) and aluminum (Al) content in that soil.
:::

Using the data dictionary, we find that the values in column `As_EPA3051` give us the arsenic concentration in mg/kg of each soil sample, as determined by [EPA Method 3051A](https://www.epa.gov/sites/default/files/2015-12/documents/3051a.pdf). This method uses a combination of heat and acid to extract specific elements (like arsenic, cadmium, chromium, copper, nickel, lead, and zinc) from soil samples.

While arsenic can occur naturally in soils, higher levels suggest the soil may have been contaminated by mining, hazardous waste, or pesticide application. Arsenic is toxic to humans.

::: reflection
QUESTIONS:

1.  What data is found in the column labeled "Fe_Mehlich3"? Why would we be interested how much of this is in the soil? (You may have to search the internet for this answer.)

2.  What data is found in the column labeled "Base_Sat_pct"? What does this variable tell us about the soil?
:::

You may notice that some cells in the `soil.values` table contain *NA*. This just means that the soil testing data for that sample isn't available yet. We'll take care of those values in the next part.

::: reflection
QUESTIONS:

3.  How many observations are in the soil testing values dataset that you loaded? What do each of these observations refer to?

4.  How many different soil characteristics are in the dataset? How can you tell?
:::

# Part 2. Summarizing with Statistics

Now that we have the dataset loaded, let's explore the data in more depth.

First, we should remove those samples that don't have soil testing data yet. We *could* keep them in the dataset, but removing them at this stage will make the analysis a little cleaner. In this case, as we know the reason the data are missing (and that reason will not skew our analysis), we can safely remove these samples. This will _not_ be the case for every data analysis.

We can remove the unanalyzed samples using the `drop_na()` function from the `tidyverse` package. The `tidyverse` package is a collection of code that makes organizing data tables (also known as data wrangling) easy in R. The `drop_na` function removes any rows from a table that contains *NA* for a particular column. This command follows the code structure:

dataset_new_name <- dataset_object_name %>% drop_na(column_name)

The `%>%` is called a pipe and it tells R that the commands after it should all be applied to the object in front of it. (In this case, we can filter out all samples missing a value for "As_EPA3051" as a proxy for samples without soil testing data.)

The following code block opens the `tidyverse` package, then tells R to filter out all samples missing a value for "As_EPA3051" (as a proxy for samples without soil testing data) from the `soil.values` object, and finally to save the new, filtered dataset as an object called `soil.values.clean`.


``` r
#install.packages('tidyverse') if you haven't already installed the tidyverse!

library(tidyverse)

soil.values.clean <- soil.values %>% drop_na(As_EPA3051)
```

Great! Now let's calculate some basic statistics. For example, we might want to know what the mean (average) arsenic concentration is for all the soil samples. We can use a combination of two functions: `pull()` and `mean()`. `pull()` lets you extract a column from your table for statistical analysis, while `mean()` calculates the average value for the extracted column.

This command follows the code structure:

object_name %>% pull(column_name) %>% mean()

For this chunk of code, R will calculate the mean for the AS_EPA3051 values in the `soil.values.clean` dataset. Because we have not used the `<-` to save the mean in a new object, R will simply display the mean.


``` r
soil.values.clean %>% pull(As_EPA3051) %>% mean()
```

```
## [1] 5.10875
```

We can run similar commands to calculate the standard deviation (`sd`),
minimum (`min`), and maximum (`max`) for the soil arsenic values.


``` r
soil.values.clean %>% pull(As_EPA3051) %>% sd()
```

```
## [1] 5.606926
```

``` r
soil.values.clean %>% pull(As_EPA3051) %>% min()
```

```
## [1] 0
```

``` r
soil.values.clean %>% pull(As_EPA3051) %>% max()
```

```
## [1] 27.3
```
The soil testing dataset contains samples from multiple geographic regions, so maybe it's more meaningful to find out what the average arsenic values are for each region. However, first we need to grab a new dataset from the `BioDIGSData` package that tells us the geographic regions of each sample.

Let's open `BioDIGS_metadata`, save it as `soil.meta`, and look at it.




``` r
soil.meta <- BioDIGS_metadata()

View(soil.meta)
```

<img src="resources/images/09-soil_exploration_module_files/figure-html//1u2CIcN2AxprMbWLzAldr_V-njdvjCS8HLYjvuy6jmfs_g33497bd5a49_0_21.png" alt="The metadata, or data about the data, opens when you view the soil.meta object." width="100%" style="display: block; margin: auto;" />

The metadata (or, data about the samples) contains information stored as 7 different variables. We can see that this dataset contains a variable called `site_id` that matches a column in the `soil.values` and `soil.values.clean` datasets. This is important! Using this variable, we can combine the `soil.values.clean` and `soil.meta` into a single dataset.

The command for combining the datasets follows the code structure:

combined_dataset_name <- first_dataset %>% inner_join(second_dataset, by = column_name_in_common)

The `inner_join` command tells R to combine the first_dataset and the second_dataset based on matching information in the column_name_in_column. It also only keeps those rows that are found in both datasets. Finally, the `<-` tells R to save this combined dataset as a new object.




``` r
soil.combined <- soil.values.clean %>% inner_join(soil.meta, by = "site_id")

View(soil.combined)
```

<img src="resources/images/09-soil_exploration_module_files/figure-html//1u2CIcN2AxprMbWLzAldr_V-njdvjCS8HLYjvuy6jmfs_g33497bd5a49_0_21.png" alt="The soil characteristics and metadata have been combined in the soil.combined dataset, which you see if you scroll to the end of the table." width="100%" style="display: block; margin: auto;" />

When you scroll through the `soil.combined` dataset, you now see the metadata columns after all the soil characteristics. In particular, there's a column called `origin` which gives the town or city location for each sample. Many of the samples from the pilot study came from 5 places in Maryland: Baltimore, Derwood, Boyds, Germantown, and Bethesda.

Let's look at the average arsenic content of the soil for each of these locations! We have to do a little bit of clever coding trickery for this using the `group_by` and `summarize` functions. 

First, we tell R to split our dataset up by a particular column (in this case, `origin`) using the `group_by` function, then we tell R to summarize the mean arsenic concentration for each group.

When using the `summarize` function, we tell R to make a new table (technically, a tibble in R) that contains two columns: the column used to group the data and the statistical measure we calculated for each group.

This command follows the code structure:

dataset %>% group_by(column_name) %>% summarize(mean(column_name))


``` r
soil.combined %>%
    group_by(origin) %>%
    summarize(mean(As_EPA3051))
```

```
## # A tibble: 5 × 2
##   origin         `mean(As_EPA3051)`
##   <chr>                       <dbl>
## 1 Baltimore, MD                5.56
## 2 Bethesda, MD                 3.46
## 3 Boyds, MD                    6.94
## 4 Derwood, MD                  4.26
## 5 Germantown, MD               4.30
```

Now we know that the mean arsenic concentration might be different for each region of origin. 

::: reflection
QUESTIONS:

5.  All the samples in the initial pilot study were collected from public park land. Some parks were located in suburban and rural areas, while others were collected from urban parks. Why might soil arsenic concentration be different for rural parks than for urban parks?

6.  What is the mean iron concentration for samples in this dataset? What about the standard deviation, minimum value, and maximum value?

7.  Calculate the mean iron concentration by region of origin. Which region has the highest mean iron concentration? What about the lowest?
:::

Let's say we're interested in looking at mean concentration for any element that was determined using EPA Method 3051. Given that there are 8 of these measures in the `soil.combined` dataset, it would be time consuming to run our code from above that calculates overall mean for each individual measure.

We can add two arguments to our `summarize` statement to calculate statistical measures for multiple columns at once: the `across` argument, which tells R to apply the `summarize` command to multiple columns; and the `ends_with` parameter, which tells R which columns should be included in the statistical calculation.

We are using `ends_with` because for this question, all the columns that we're interested in end with the string 'EPA3051'.

This command follows the code structure:

dataset %>% group_by(column_name) %>% summarize(across(ends_with(common_column_name_ending), mean))


``` r
soil.combined %>%
    group_by(origin) %>%
    summarize(across(ends_with('EPA3051'), mean))
```

```
## # A tibble: 5 × 8
##   origin       As_EPA3051 Cd_EPA3051 Cr_EPA3051 Cu_EPA3051 Ni_EPA3051 Pb_EPA3051
##   <chr>             <dbl>      <dbl>      <dbl>      <dbl>      <dbl>      <dbl>
## 1 Baltimore, …       5.56     0.359        34.5       35.0       17.4       67.2
## 2 Bethesda, MD       3.46     0.375        67.5       17.0       26.6       41.8
## 3 Boyds, MD          6.94     0.0525       16.8       16.8       12.9       31.6
## 4 Derwood, MD        4.26     0.335        39.5       31.3       35.1       42.1
## 5 Germantown,…       4.30     0.602        19.9       23.3       17.7       38.2
## # ℹ 1 more variable: Zn_EPA3051 <dbl>
```

This is a *much* more efficient way to calculate statistics.

::: reflection
QUESTIONS:

8.  Calculate the maximum values for concentrations that were determined using EPA Method 3051. (HINT: change the function you call in the `summarize` statement.) Which of these metals has the maximum concentration you see, and in which region is it found?

9.  Calculate both the mean and maximum values for concentrations that were determined using the Mehlich3 test. (HINT: change the terms in the the part of the code that uses `ends_with`, as well as the function you call in the `summarize` statement.) Which of these metals has the highest average and maximum concentrations, and in which region are they found?
:::

# Part 3. Visualizing the Data

Often, it can be easier to immediately interpret data displayed as a plot than as a list of values. For example, we can more easily understand how the arsenic concentration of the soil samples are distributed if we create histograms compared to looking at point values like mean, standard deviation, minimum, and maximum.

One way to make histograms in R is with the `hist()` function. This function only requires that we tell R which column of the dataset that we want to plot. (However, we also have the option to tell R a histogram name and a x-axis label.)

We can again use the `pull()` command and pipes (`%>%`) to choose the column we want from the `soil.values.clean` dataset and make a histogram of them.

This combination of commands follows the code structure:

dataset %>% pull(column_name) %>% hist(main = chart_title, xlab = x_axis_title)


``` r
soil.combined %>% 
    pull(As_EPA3051) %>% 
    hist(main = 'Histogram of Arsenic Concentration', 
         xlab ='Concentration in mg/kg' )
```

<img src="resources/images/09-soil_exploration_module_files/figure-html/unnamed-chunk-20-1.png" width="672" />

We can see that almost all the soil samples had very low concentrations of arsenic (which is good news for the soil health!). In fact, many of them had arsenic concentrations close to 0, and only a few sampling locations appear to have high levels of arsenic.

We might also want to graphically compare arsenic concentrations among the geographic regions in our dataset. We can do this by creating boxplots. Boxplots are particularly useful when comparing the mean, variation, and distributions among multiple groups.

In R, one way to create a boxplot is using the `boxplot()` function. We don't need to use pipes for this command, but instead will specify what columns we want to use from the dataset inside the `boxplot()` function itself.

This command follows the code structure:

boxplot(column_we're_plotting ~ grouping_variable, data = dataset, main = "Title of Graph", xlab = "x_axis_title", ylab = "y_axis_title")


``` r
boxplot(As_EPA3051 ~ origin, 
        data = soil.combined,
        main = "Arsenic Concentration by Geographic Region",
        xlab = "Region",
        ylab = "Arsenic Concentration in mg/kg")
```

<img src="resources/images/09-soil_exploration_module_files/figure-html/unnamed-chunk-21-1.png" width="672" />

By using a boxplot, we can quickly see that, while two sampling sites within Baltimore, MD have a very high concentration of arsenic in the soil (indicated by the two small circles on the plot), in general there isn't a difference in arsenic content between any of our locations.

::: reflection
QUESTIONS:

10. Create a histogram for *iron* concentration, as well as a boxplot comparing iron concentration by region. Is the iron concentration similar among regions? Are there any outlier sites with unusually high or low iron concentrations?

11. Create a histogram for *lead* concentration, as well as a boxplot comparing lead concentration by region. Is the lead concentration similar among regions? Are there any outlier sites with unusually high or low lead concentrations?
:::


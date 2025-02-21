
# Activity Questions

## Part 1. Examining the Data
1. What data is found in the column labeled "Fe_Mehlich3"? Why would we be interested how much of this is in the soil? (You may have to search the internet for this answer.)

2. What data is found in the column labeled "Base_Sat_pct"? What does this variable tell us about the soil?

3. How many observations are in the soil testing values dataset that you loaded? What do each of these observations refer to?

4. How many different soil characteristics are in the dataset? How can you tell?

## Part 2. Summarizing the Data with Statistics

5. All the samples in the initial pilot study were collected from public park land. Some parks were located in suburban and rural areas, while others were collected from urban parks. Why might soil arsenic concentration be different for rural parks than for urban parks?

6. What is the mean iron concentration for samples in this dataset? What about the standard deviation, minimum value, and maximum value?

7. Calculate the mean iron concentration by region. Which region has the highest mean iron concentration? What about the lowest?

8. Calculate the maximum values for concentrations that were determined using EPA Method 3051. (HINT: change the function you call in the `summarize` statement.) Which of these metals has the maximum concentration you see, and in which region is it found?

9. Calculate both the mean and maximum values for concentrations that were determined using the Mehlich3 test. (HINT: change the terms in the `columns_to_include` vector, as well as the function you call in the `summarize` statement.) Which of these metals has the highest average and maximum concentrations, and in which region are they found?

## Part 3. Visualizing the Data

10. Create a histogram for _iron_ concentration, as well as a boxplot comparing iron concentration by region. Is the iron concentration similar among regions? Are there any outlier sites with unusually high or low iron concentrations?

11. Create a histogram for _lead_ concentration, as well as a boxplot comparing lead concentration by region. Is the lead concentration similar among regions? Are there any outlier sites with unusually high or low lead concentrations?


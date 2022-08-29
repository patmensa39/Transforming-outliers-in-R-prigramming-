# Transforming-outliers-in-R-prigramming-
#Tranforming outliers ####
### make sure to install all the required packages 
pacman::p_load(datasets, pacman, rio, tidyverse)

### loading the Island dataset
### The areas in thousands of square miles of the landmasses which 
### exceed 10,000 square miles.
?islands
islands
view(islands)
glimpse(islands)

### making a histogram 
islands %>% hist(main = "Boxplot of Island")

### making boxplot for the data 
islands %>% boxplot(horizontal = TRUE)

### Removing outliers 
### somrting the observation in descending value 
islands_philant <- islands %>% enframe() %>% # this converts the vector to tibbles 
  arrange(desc(value)) %>% # this sorts by descending order 
  print()

### Filtering out the continents 
islands_philant %<>% filter(value < 1000) %>% print()

### making histogram and boxplot again
### making a histogram 
islands_philant %>% pull(value) %>% hist(main = "Boxplot of Island")

### making boxplot for the data 
islands_philant %>% select(value) %>% boxplot(horizontal = TRUE)

### Another way is also called winsorizing and this refers to giving
### extreme cases the same value as less extreme cases, which can make
### more robust analysis, even if it is peculiar in this case
view(islands_philant)

### the largest continent is Greeland area about 840000 square mile 
islands_philant <- islands_philant %>% 
  mutate(value = ifelse(
    value > 840,
    840, 
    value
    )
  ) %>% 
  print()


### making a histogram 
islands_philant %>% pull(value) %>% hist(main = "Boxplot of Island")

### making boxplot for the data 
islands_philant %>% select(value) %>% boxplot(horizontal = TRUE)

### creating a new groups 
islands_princess <- islands %>% enframe() %>% arrange(desc(value)) %>% print()
islands_princess %<>% mutate(landmass = ifelse
                             (value < 1000,
                               "island", 
                               "Continent")) %>% print()

### Boxplot for only islands
islands_princess %>% filter(landmass == "island") %>% 
  select(value) %>% boxplot(
    horizontal = TRUE,
    main = "Boxplot of Islands",
    xlab = "Thousands of square miles"
  ) 


### Transforming the data 
### let us havea lot of the previous data 
islands %>% hist(main = "Histogram")
islands %>% boxplot(horizontal = TRUE)
### this is positively skewed

## By transforming the data using log tranformation
islands %>% log() %>% hist(main = "Histogram")
islands %>% log() %>% boxplot(horizontal = TRUE)


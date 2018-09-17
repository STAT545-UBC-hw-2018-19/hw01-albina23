---
title: "Gapminder Data Exploration"
output: 
    html_document:
        theme: cerulean
        toc: true
        keep_md: true

---
## Setting up access to the dataset

List all the available datasets in R

```r
data(package = .packages(all.available = TRUE))
```

```
## Warning in data(package = .packages(all.available = TRUE)): datasets have
## been moved from package 'base' to package 'datasets'
```

```
## Warning in data(package = .packages(all.available = TRUE)): datasets have
## been moved from package 'stats' to package 'datasets'
```

I will be analyzing gapminder dataset. First, install the `gapminder` R package using this command: install.packages('gapminder')

If you already have the package installed, access the package


```r
library(gapminder)
```

```
## Warning: package 'gapminder' was built under R version 3.5.1
```

## Exploring of data frames

Previewing dataset with the head function

```r
head(gapminder)
```

```
## # A tibble: 6 x 6
##   country     continent  year lifeExp      pop gdpPercap
##   <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
## 1 Afghanistan Asia       1952    28.8  8425333      779.
## 2 Afghanistan Asia       1957    30.3  9240934      821.
## 3 Afghanistan Asia       1962    32.0 10267083      853.
## 4 Afghanistan Asia       1967    34.0 11537966      836.
## 5 Afghanistan Asia       1972    36.1 13079460      740.
## 6 Afghanistan Asia       1977    38.4 14880372      786.
```

Explore some of its properties

Provide a summary of variables and its quantitative characteristics

```r
summary(gapminder)
```

```
##         country        continent        year         lifeExp     
##  Afghanistan:  12   Africa  :624   Min.   :1952   Min.   :23.60  
##  Albania    :  12   Americas:300   1st Qu.:1966   1st Qu.:48.20  
##  Algeria    :  12   Asia    :396   Median :1980   Median :60.71  
##  Angola     :  12   Europe  :360   Mean   :1980   Mean   :59.47  
##  Argentina  :  12   Oceania : 24   3rd Qu.:1993   3rd Qu.:70.85  
##  Australia  :  12                  Max.   :2007   Max.   :82.60  
##  (Other)    :1632                                                
##       pop              gdpPercap       
##  Min.   :6.001e+04   Min.   :   241.2  
##  1st Qu.:2.794e+06   1st Qu.:  1202.1  
##  Median :7.024e+06   Median :  3531.8  
##  Mean   :2.960e+07   Mean   :  7215.3  
##  3rd Qu.:1.959e+07   3rd Qu.:  9325.5  
##  Max.   :1.319e+09   Max.   :113523.1  
## 
```

Display the internal structure to the dataset (an alternative to summary)

```r
str(gapminder)
```

```
## Classes 'tbl_df', 'tbl' and 'data.frame':	1704 obs. of  6 variables:
##  $ country  : Factor w/ 142 levels "Afghanistan",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ continent: Factor w/ 5 levels "Africa","Americas",..: 3 3 3 3 3 3 3 3 3 3 ...
##  $ year     : int  1952 1957 1962 1967 1972 1977 1982 1987 1992 1997 ...
##  $ lifeExp  : num  28.8 30.3 32 34 36.1 ...
##  $ pop      : int  8425333 9240934 10267083 11537966 13079460 14880372 12881816 13867957 16317921 22227415 ...
##  $ gdpPercap: num  779 821 853 836 740 ...
```

Measure the number of rows

```r
nrow(gapminder)
```

```
## [1] 1704
```

Measure the number of columns

```r
ncol(gapminder)
```

```
## [1] 6
```

Alternatively, you can find dataset's dimensions

```r
dim(gapminder)
```

```
## [1] 1704    6
```

## Exploring Variables

The dataset consists of panel data: 3 variables (life expectancy, population and GDP per capita) are recorded from 1952 to 2007 for each country and the corresponding continent (some data is missing). 

### Individual statistics
We can measure various summary statistics (means, medians, max/min, etc.) for our variables by adding the variable name after $.

Find mean, max, min, standard deviation, and variance for the life expectancy variable

```r
mean(gapminder$lifeExp)
```

```
## [1] 59.47444
```

```r
max(gapminder$lifeExp)
```

```
## [1] 82.603
```

```r
min(gapminder$lifeExp)
```

```
## [1] 23.599
```

```r
sd(gapminder$lifeExp)
```

```
## [1] 12.91711
```

```r
var(gapminder$lifeExp)
```

```
## [1] 166.8517
```

### Adding If statements

If you wanted to know whether the GDP per capita for any country in any given year has ever exceeded 110,000 USD, you can add the following if statement

```r
if (max(gapminder$gdpPercap) > 110000){
  print('The GDP per capita for one of the countries has exceeded 110,000 USD.')
}else{
  print('The GDP per capita for any of the countries has never exceeded 110,000 USD.')
}
```

```
## [1] "The GDP per capita for one of the countries has exceeded 110,000 USD."
```

### Printing desired outcomes
You can print any of the selected summary statistics


```r
print('The standard deviation of the life expectancy variable is')
```

```
## [1] "The standard deviation of the life expectancy variable is"
```

```r
print(sd(gapminder$lifeExp))
```

```
## [1] 12.91711
```

### Group statistics
We can find various summary statistics (means, medians, max/min, etc.) for different groups using the 'aggregate' function.

Find the average population size of a country in a given year

```r
aggregate(pop ~ year, gapminder, mean)
```

```
##    year      pop
## 1  1952 16950402
## 2  1957 18763413
## 3  1962 20421007
## 4  1967 22658298
## 5  1972 25189980
## 6  1977 27676379
## 7  1982 30207302
## 8  1987 33038573
## 9  1992 35990917
## 10 1997 38839468
## 11 2002 41457589
## 12 2007 44021220
```

Find max GDP per capita by continent

```r
aggregate(gdpPercap ~ continent, gapminder, max)
```

```
##   continent gdpPercap
## 1    Africa  21951.21
## 2  Americas  42951.65
## 3      Asia 113523.13
## 4    Europe  49357.19
## 5   Oceania  34435.37
```

Find median life expectancy for each country across all years

```r
aggregate(lifeExp ~ country, gapminder, median)
```

```
##                      country  lifeExp
## 1                Afghanistan 39.14600
## 2                    Albania 69.67500
## 3                    Algeria 59.69100
## 4                     Angola 39.69450
## 5                  Argentina 69.21150
## 6                  Australia 74.11500
## 7                    Austria 72.67500
## 8                    Bahrain 67.32250
## 9                 Bangladesh 48.46600
## 10                   Belgium 73.36500
## 11                     Benin 50.04700
## 12                   Bolivia 51.94100
## 13    Bosnia and Herzegovina 70.27500
## 14                  Botswana 52.92700
## 15                    Brazil 62.41250
## 16                  Bulgaria 70.85500
## 17              Burkina Faso 47.12950
## 18                   Burundi 45.03100
## 19                  Cambodia 48.18600
## 20                  Cameroon 49.60550
## 21                    Canada 74.98500
## 22  Central African Republic 44.09900
## 23                      Chad 48.45000
## 24                     Chile 68.80850
## 25                     China 64.74618
## 26                  Colombia 65.24500
## 27                   Comoros 51.93600
## 28          Congo, Dem. Rep. 45.25700
## 29               Congo, Rep. 53.93850
## 30                Costa Rica 72.10000
## 31             Cote d'Ivoire 48.15950
## 32                   Croatia 70.55000
## 33                      Cuba 73.18300
## 34            Czech Republic 70.83500
## 35                   Denmark 74.66000
## 36                  Djibouti 47.66550
## 37        Dominican Republic 62.75750
## 38                   Ecuador 62.82600
## 39                     Egypt 54.66250
## 40               El Salvador 57.45150
## 41         Equatorial Guinea 42.84300
## 42                   Eritrea 44.33850
## 43                  Ethiopia 44.71300
## 44                   Finland 73.53500
## 45                    France 74.36000
## 46                     Gabon 54.67700
## 47                    Gambia 43.71100
## 48                   Germany 73.15000
## 49                     Ghana 52.75000
## 50                    Greece 74.46000
## 51                 Guatemala 57.08300
## 52                    Guinea 41.82650
## 53             Guinea-Bissau 38.39600
## 54                     Haiti 50.69200
## 55                  Honduras 59.15550
## 56          Hong Kong, China 74.52500
## 57                   Hungary 69.54000
## 58                   Iceland 76.55000
## 59                     India 55.40200
## 60                 Indonesia 54.43050
## 61                      Iran 58.66100
## 62                      Iraq 57.92850
## 63                   Ireland 72.56500
## 64                    Israel 73.75500
## 65                     Italy 74.23000
## 66                   Jamaica 70.66000
## 67                     Japan 76.24500
## 68                    Jordan 62.43650
## 69                     Kenya 53.83450
## 70          Korea, Dem. Rep. 66.91050
## 71               Korea, Rep. 65.94450
## 72                    Kuwait 70.32600
## 73                   Lebanon 66.54100
## 74                   Lesotho 49.12950
## 75                   Liberia 42.41750
## 76                     Libya 59.79850
## 77                Madagascar 47.92500
## 78                    Malawi 44.38800
## 79                  Malaysia 66.62800
## 80                      Mali 42.81500
## 81                Mauritania 52.22550
## 82                 Mauritius 65.82050
## 83                    Mexico 66.21850
## 84                  Mongolia 56.49000
## 85                Montenegro 73.52350
## 86                   Morocco 57.69000
## 87                Mozambique 42.28850
## 88                   Myanmar 57.05750
## 89                   Namibia 53.38650
## 90                     Nepal 48.17100
## 91               Netherlands 75.64500
## 92               New Zealand 73.03000
## 93                 Nicaragua 58.38400
## 94                     Niger 41.94450
## 95                   Nigeria 45.17000
## 96                    Norway 75.63000
## 97                      Oman 60.04750
## 98                  Pakistan 55.10050
## 99                    Panama 69.57650
## 100                 Paraguay 66.61350
## 101                     Peru 59.92650
## 102              Philippines 61.07100
## 103                   Poland 70.91500
## 104                 Portugal 71.59000
## 105              Puerto Rico 73.59500
## 106                  Reunion 68.47450
## 107                  Romania 69.41000
## 108                   Rwanda 43.71650
## 109    Sao Tome and Principe 59.45050
## 110             Saudi Arabia 60.85100
## 111                  Senegal 50.62900
## 112                   Serbia 70.23100
## 113             Sierra Leone 37.56050
## 114                Singapore 71.27750
## 115          Slovak Republic 70.89000
## 116                 Slovenia 71.01650
## 117                  Somalia 41.47350
## 118             South Africa 53.53050
## 119                    Spain 75.34500
## 120                Sri Lanka 67.35300
## 121                    Sudan 49.06900
## 122                Swaziland 48.09250
## 123                   Sweden 75.93000
## 124              Switzerland 75.80000
## 125                    Syria 62.89250
## 126                   Taiwan 71.37500
## 127                 Tanzania 49.05850
## 128                 Thailand 63.54550
## 129                     Togo 54.17900
## 130      Trinidad and Tobago 68.56600
## 131                  Tunisia 61.94250
## 132                   Turkey 60.27150
## 133                   Uganda 48.43800
## 134           United Kingdom 73.40000
## 135            United States 74.01500
## 136                  Uruguay 70.14300
## 137                Venezuela 68.00650
## 138                  Vietnam 57.29000
## 139       West Bank and Gaza 62.58550
## 140              Yemen, Rep. 46.64400
## 141                   Zambia 46.06150
## 142                 Zimbabwe 53.17650
```


### Adding selection condtions

Download dplyr package to help us sort, summarize and filter tabular data using this command: install.packages('dplyr')

If the package is already installed, get access to it

```r
library(dplyr)
```

```
## Warning: package 'dplyr' was built under R version 3.5.1
```

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

I have an option to only work with the selected subgroups. For instance, I want to find an average population of a Europen country in 2005. Notice how you can add multiple conditions here.

```r
    filter(gapminder, continent == "Europe", year == 2005)
```

```
## Warning: package 'bindrcpp' was built under R version 3.5.1
```

```
## # A tibble: 0 x 6
## # ... with 6 variables: country <fct>, continent <fct>, year <int>,
## #   lifeExp <dbl>, pop <int>, gdpPercap <dbl>
```

```r
    MeanPopulation <- mean(gapminder$pop)
    print(MeanPopulation)
```

```
## [1] 29601212
```

## Visualizing data


### Simple plots
We can visualize the relationships between these variables. 

Here is a scatterplot between population size (on the x-axis) and GDP per capita (on the y-axis)

```r
plot(gapminder$pop, gapminder$gdpPercap)
```

![](hw01_gapminder_files/figure-html/unnamed-chunk-17-1.png)<!-- -->

Here is a scatterplot between time (measured in years on the x-axis) and GDP per capita (on the y-axis)

```r
plot(gapminder$year, gapminder$gdpPercap)
```

![](hw01_gapminder_files/figure-html/unnamed-chunk-18-1.png)<!-- -->

### Complex plots

If we want to be able to capture more complexity in our groups, we need to install the ggplot package using this command: install.packages('ggplot2')

If the package is already installed, get access to it

```r
library(ggplot2)
```

```
## Warning: package 'ggplot2' was built under R version 3.5.1
```

Here we can visualize various continents by using colors.

```r
ggplot(gapminder, aes(x=pop, y=gdpPercap, color=continent))+geom_point()+ xlab("Country population size") + 
  ylab("GDP per capita") 
```

![](hw01_gapminder_files/figure-html/unnamed-chunk-20-1.png)<!-- -->

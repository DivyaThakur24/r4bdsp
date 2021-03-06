

## Lesson 6 Normal Distribution

In this exercise we will find the probability of a given observation within a normal distribution. All definitions and data come from the original Udacity lesson and can be found [here.](https://storage.googleapis.com/supplemental_media/udacityu/1478678538/Lesson6.pdf "Lesson6.pdf")

```r
#install.packages("ggfortify")
#library(ggfortify)
```

### Probability Distribution Function. 
The probability distribution function is a normal curve with an area of 1 beneath it, to represent the cumulative frequency of values.

#### Data

```r
mean=1.85; sd=.15
lb=1.7; ub=2

#generate student heights
x <- seq(-4,4,length=100)*sd + mean
hx <- dnorm(x,mean,sd)
```
#### Create a density plot

```r
plot(x, hx, type="n", xlab="Student Height Values", ylab="Density",
  main="Probability Distribution")
lines(x, hx)
polygon(c(lb,x,ub), c(0,hx,0), col="blue") 
```

<img src="lesson6_files/figure-html/unnamed-chunk-4-1.png" width="672" />

### Finding the probability
If given an observation, you can find the probability and show the area below, above, and between particular observations. To do this you must first calculated the z-score


#### Z-Score
 
$$ z=\frac{x-\mu}{\sigma}$$
The z-score is calculated  by taking the observation minus the mean and dividing by the standard deviation. If given an observation of 2.05 meters the z-score would be calculated  as follows:


```r
obs1 <- 2.05
obs1_zscore <- round((obs1 - mean)/sd, 2)

print(paste0("The Z-Score for a student with the Height of 2.05 meters is: ", obs1_zscore))
```

```
#> [1] "The Z-Score for a student with the Height of 2.05 meters is: 1.33"
```

#### Proportion using a z-table
From this we know that a height of 2.05 is 1.33 standard deviations away from the mean. With this information we can use a z-table to determine the proportion of students that are shorter than this. The z-table can be found [here.](https://s3.amazonaws.com/udacity-hosted-downloads/ZTable.jpg, "ztable")

From the z-table we get a proportion of .9082 or 90.82 percent. A person with the height of 2.05 is taller than 90.82 percent of the students.
We can calculate this in R using the pnorm function. It will give a more exact number than when using the table.

#### Proportion using pnorm


```r
#you can also use pnorm to get the proportion rather than looking it up in a z-table
obs1_spor <- round(pnorm(obs1, mean, sd),4)
print(paste0("The proportion of students shorter than 2.05 meters is: ", obs1_spor))
```

```
#> [1] "The proportion of students shorter than 2.05 meters is: 0.9088"
```

#### Plotting the percentage
By multiplying the proportion by 100 you can get the percentage of students that are shorter than 2.05 meters. You can also find the percentage of students that are taller than 2.05 meters by subtracting the proportion from 1.

```r
# a plot of the data
plot(x, hx, type="n", xlab="Student Height Values", ylab="Density",
  main="Percentage above and below 2.05 meters")

# to plot the proportion I grab the samples that are above and below the observation. 
i <- x <= obs1
o <- x >= obs1
lines(x, hx)

# I use the gathered high and low observations to make polygons showing the area.
polygon(c(lb,x[i],obs1), c(0,hx[i],0), col="blue") 
polygon(c(obs1,x[o],ub), c(0,hx[o],0), col="yellow") 

#label the percentage
taller_obs1 <- round((1 - obs1_spor) * 100, 2)
shorter_obs1 <- obs1_spor * 100
text(1.8,.3, labels = shorter_obs1, col = "white")
text(2.1,.3, labels = taller_obs1, col = "blue")
```

<img src="lesson6_files/figure-html/unnamed-chunk-7-1.png" width="672" />

#### Proportion of a range
You can find the proportion of students that fall between a given range by subtraction the proportion of the first observation from the proportion of the second observation.


```r
obs2 <- 1.87
obs2_spor <- round(pnorm(obs2, mean, sd),4)

# a plot of the data
plot(x, hx, type="n", xlab="Student Height Values", ylab="Density",
  main="Percentage between 1.87 and 2.05 meters")

# to plot the proportion I grab the samples that are between the observations. 
i_diff <- x >= obs2 & x <= obs1

lines(x, hx)

# I use the gathered range of observations to make polygons showing the area.
polygon(c(obs2,x[i_diff],obs1), c(0,hx[i_diff],0), col="blue") 


#label the percentage
between_obs <- round((obs1_spor - obs2_spor) * 100, 2)

text(1.95,.3, labels = between_obs, col = "white")
```

<img src="lesson6_files/figure-html/unnamed-chunk-8-1.png" width="672" />

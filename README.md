# Assignment 3
Amanda Elmore  
October 11, 2014  

Complete the exercises listed below and submit as a pull request to the [Assignment 3 repository](http://www.github.com/microbialinformatics/assignment03).  Format this document approapriately using R markdown and knitr. For those cases where there are multiple outputs, make it clear in how you format the text and interweave the solution, what the solution is.

Your pull request should only include your *.Rmd and *.md files. You may work with a partner, but you must submit your own assignment and give credit to anyone that worked with you on the assignment and to any websites that you used along your way. You should not use any packages beyond the base R system and knitr.

This assignment is due on October 10th.

------

1.  Generate a plot that contains the different pch symbols. Investigate the knitr code chunk options to see whether you can have a pdf version of the image produced so you can print it off for yoru reference. It should look like this:

    <img src="pch.png", style="margin:0px auto;display:block" width="500">


```r
pdf("Elmoreplot.pdf") #save the plot to pdf
x<-c(1:25)
plot.pdf=plot(x, rep(1,25), axes=FALSE, main="PCH Symbols", xlab="PCH value", ylab="",  pch=c(1:25), cex=2)
axis(1, at = seq(1,25,1), label=x, tck=1, col.ticks="grey") #add hash marks
axis(1, at=seq(1,25,1), col.ticks="black") #add smaller black ticks
```

2.  Using the `germfree.nmds.axes` data file available in this respositry, generate a plot that looks like this. The points are connected in the order they were sampled with the circle representing the beginning ad the square the end of the time course:

    <img src="beta.png", style="margin:0px auto;display:block" width="700">

```r
x<-read.table(file="germfree.nmds.axes", header=T)
x$mouse<-as.factor(x$mouse)
mouselevels<-levels(x$mouse) #make a variable with the mouse names to be used in a for loop
plot(x$axis1, x$axis2, type="n", xlab="NMDS Axis 1", ylab="NMDS Axis 2")
colors<-c("black","blue","red","green","dark red")
for (i in c(1:5)){ #print lines onto the plot for each of the mice individually with a different color
y <- subset(x,mouse==mouselevels[i])
y <- y[order(y$day),] #just in case the days are out of order
lines(y$axis1, y$axis2, col=colors[i], lwd=1.5)
day1<- subset(y, y$day==min(y$day))
day21<- subset(y, y$day==max(y$day))
points(day1$axis1, day1$axis2, pch=16, cex=2, col=colors[i])
points(day21$axis1, day21$axis2, pch=15, cex=2, col=colors[i])
}
legend(0,-.1, legend=c("Mouse 337", "Mouse 343","Mouse 361","Mouse 387","Mouse 389"), lwd=4,col=colors)
```

![plot of chunk unnamed-chunk-2](./README_files/figure-html/unnamed-chunk-2.png) 

3.  On pg. 57 there is a formula for the probability of making x observations after n trials when there is a probability p of the observation.  For this exercise, assume x=2, n=10, and p=0.5.  Using R, calculate the probability of x using this formula and the appropriate built in function. Compare it to the results we obtained in class when discussing the sex ratios of mice.

```r
x<-2
n<-10
p<-0.5
binomial.formula <- choose(n,x)*p^x*(1-p)^(n-x)
binomial.builtin <- dbinom(x,n,p) #function built in to R that gives the probability of drawing a certain number of something from a binomial distribution (from Lecture 9)
```

**Answer: ** The probability of making 2 observations after 10 trials when there is a 0.5 probability of the observations is 0.0439 based on the actual formula and this is the same answer you get when using the binomial probability function built in to R (0.0439). The sex ratios example from lecture is the same because the probability of a male or female pup is 0.5.



4.  On pg. 59 there is a formula for the probability of observing a value, x, when there is a mean, mu, and standard deviation, sigma.  For this exercise, assume x=10.3, mu=5, and sigma=3.  Using R, calculate the probability of x using this formula and the appropriate built in function


```r
x<-10.3
mu<-5
sigma <- 3
formula <- 1/(sqrt(2*pi)*sigma)*exp(-(x-mu)^2/(2*sigma^2))
builtin <- dnorm(x,mu,sigma)
```
**Answer: ** The probability of observing the value 10.3 when thre is a mean 5 and standard deviation 3 is the same using the normal distribution formula (0.0279) and the R built in normal distribution function (0.0279).


5.  One of my previous students, Joe Zackular, obtained stool samples from 89 people that underwent colonoscopies.  30 of these individuals had no signs of disease, 30 had non-cancerous ademonas, and 29 had cancer.  It was previously suggested that the bacterium *Fusobacterium nucleatum* was associated with cancer.  In these three pools of subjects, Joe determined that 4, 1, and 14 individuals harbored *F. nucleatum*, respectively. Create a matrix table to represent the number of individuals with and without _F. nucleatum_ as a function of disease state.  Then do the following:

    * Run the three tests of proportions you learned about in class using built in R  functions to the 2x2 study design where normals and adenomas are pooled and compared to carcinomas.
    * Without using the built in chi-squared test function, replicate the 2x2 study design in the last problem for the Chi-Squared Test...
      * Calculate the expected count matrix and calculate the Chi-Squared test statistics. Figure out how to get your test statistic to match Rs default statistic.
      *	Generate a Chi-Squared distributions with approporiate degrees of freedom by the method that was discussed in class (hint: you may consider using the `replicate` command)
      * Compare your Chi-Squared distributions to what you might get from the appropriate built in R functions
      * Based on your distribution calculate p-values
      * How does your p-value compare to what you saw using the built in functions? Explain your observations.


6\.  Get a bag of Skittles or M&Ms.  Are the candies evenly distributed amongst the different colors?  Justify your conclusion.


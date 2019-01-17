---
layout: post
title:      "Stats Math I"
date:       2019-01-17 23:55:40 +0000
permalink:  stats_math_i
---


Here's a brief summary of Section 10 of the online data science coursework for Flatiron School:


```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

df = pd.read_csv("kc_house_data.csv")
```



This dataset gives us lots of continuous data we can start to explore.  Let's get started.

If you want to to stats, start finding the mean. Mean = the sum of the items to be counted divided by the number of items to be counted.



```
def find_mean(df):
    column_means = []
    for idx, col in enumerate(df):
        mean = (np.sum(df[col]) / len(df[col]))
        column_means.append(mean)
    print(f' Mean: {column_means}')
    return column_means
find_mean(math_df)
```



When you have the mean you can get the standard deviation, a basic number for how far away items are away from the mean.  Calculate this by the sumation of each variable subtrated by it's mean and divide that by number of items.  Find the square root of the result.




```
def var_and_stddev(df):

    columns_std = []
    columns_variance = []
    for idx, col in enumerate(df):
        selection = 0
        mn = np.mean(df[col])
        selection += (df[col]-mn)**2
        selection /= len(df[col])
        variance = np.sum(selection)
        columns_std.append(np.sqrt(variance))
        columns_variance.append(variance)

    return columns_std, columns_variance
stddev(math_df)
```


So far, so good, but we'd like to compare our predictor values to the price more directly.  Lets start with covariance.



```
y = math_df.price
math_df = df.drop(['price'], axis=1)

#this code doesn't work
'''
def covariance(x,y):
    covariance_list = []
    for idx, col in enumerate(x):
        covariance = (np.sum(x[col]-np.mean(x[col]))*(np.sum(y)-np.mean(y)))
        covariance /= len(math_df['sqft_living'])
        covariance_list.append(covariance)
    return covariance_list
covariance(math_df['sqft_living'],y)
'''

# covariance matrix =   ([[x,x], [x,y], 
#                        [y,x], [y,y]])
sqft = np.cov(math_df['sqft_living'],y)
flrs =  np.cov(math_df['floors'],y)
lat = np.cov(math_df['lat'],y)
print(f' square feet:\n {sqft} \n floors:\n {flrs} \n latitude:\n {lat}')
    
    
```



What I wanted to do was to write a function but I can't get it.  So instead I'll be interpreting the covariance matrix resulting in running each predictor against the price.  Did I mention I'll still a student?

These numbers are all very nice but we have they are not scaled.  We really don't know what the 2.367 positive relation for square footage means against the 5.09 of the floors.  We *can* however, scale thes using correlation AKA the Pearson correlation coefficient.

r = n sigma xy - (sigma x)(sigma y) / the square root of (sigma of (x - mean of x)^2) (sigma of (y - mean of y)^2)

what's returned is a scaled version of covariance from -1 being a negative 1:1 relation, and 1 being a positive 1:1 relation.



```
np.corrcoef(math_df['sqft_living'],y)
```

array([[1.       , 0.7019173],
             [0.7019173, 1.       ]])





I'm being lazy, I know -- but look at that! These variables have a 1 corrceof with themselves (we know the method works!) and a whoping 0.7 with eachother.

but wait--we can use this to fill in a linreg equation.

1st
m = r (std_y / std_x)

2nd
b = y_mean (m * x_mean)

3rd
y_prime = b + m*x_mean

And look at that.  We now have a decent predictor of what each addition square foot will do to the price of a home. 




```
def slope_y_intercept_and_linreg(r,x,y):
    #this code is broken but my intent should be clear
    mu_x = []
    std_x = []
    for idx, col in enumerate(x):
        mu = np.mean(x[col])
        mu_x.append(mu)
        
    mu_y = np.mean(y)
    for idx, col in enumerate(x):
        std = np.std(x[col])
        std_x.append(std)
    y_std = np.std(y)
    slopes = [r * (std_x[i]/std_y) for i in mu_x]
    intercepts = [mu_y - (slopes[i]*mu_x[i]) for i in slopes and mu_x]
    linreg = [intercepts[i] + (slopes[i]*mu_x[i]) for i in intercepts and slopes and mu_x]
    return linreg

slope_y_intercept_and_linreg()
```

Now that we've fitted a linear regression it's about time we tested it's strength as a prediting model.  For that we'll need to calculate R^s or the  SSR by subracting the quotient of dividing SSE by SST from one.

From here we have a good understanding of our data and can preform even more tests, but that will be the scope of a differnet blog post.

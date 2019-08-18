---
layout: post
title:      "Northwind Traders:"
date:       2019-08-18 15:20:53 +0000
permalink:  northwind_traders
---

## Hypothesis Testing Across the 7 Seas


To Null or not to Null, that is the question.  Or, more appropriately, to reject or fail to reject, as it all comes to to details when dealing with stats.  After all, any shmuck with with a know how of PR can twist their words and make claims that their Powdered Toast Flakes have... “a difference in affecting blood pressure” while this most certainly is true, it isn’t what the consumer would understand it to mean.  A PR officer would call that spin. A data scientist would call that vague at best.
<br>
<br>
Good science is strict:
<br>
<br>
If we really want to find out what’s going on when people eat Powdered Toast Flakes, we have to ask the right questions and quantify them with firm, rational, numbers, numbers don’t lie add spin.
<br>
<br>

Using the Northwind Traders database and SQL queries.  I was able to pull relevant information regarding questions that might be useful to know.  The first “Do discounts affect how much a customer orders?”  Informal questions like these drive the exploration of what would otherwise be a daunting heap of information.
<br>
<br>

<a href="https://imgur.com/su3XdP6"><img src="https://i.imgur.com/su3XdP6.png" title="source: imgur.com" width="750" height="500" /></a>

<br>
<br>
A good query joins relevant information across tables and filters down the results to something more manageable.  I chose to make pandas dataframes with some additional relevant information that I would use to further my understanding of the data.
<br>
<br>
```
q = '''select regiondescription, territorydescription, shipregion, shipcountry, unitprice, quantity
            from region r join Territory t on r.id=t.regionid
            join EmployeeTerritory et on t.id=et.territoryid
            join Employee e on et.Employeeid=e.id
            join 'Order' o on e.id=o.employeeid
            join OrderDetail od on o.id=od.OrderId
            where RegionDescription == 'Western';'''
q2 = '''select regiondescription, territorydescription, shipregion, shipcountry, unitprice, quantity
            from region r join Territory t on r.id=t.regionid
            join EmployeeTerritory et on t.id=et.territoryid
            join Employee e on et.Employeeid=e.id
            join 'Order' o on e.id=o.employeeid
            join OrderDetail od on o.id=od.OrderId
            where RegionDescription != 'Western';'''



west = pd.read_sql_query(q, cnx)
not_west = pd.read_sql_query(q2, cnx)
```
<br>
<br>
Asking the Hard Questions: Hypothesis Testing

<br>
<br>
“The Most Exciting Phrase in Science Is Not ‘Eureka!’ But ‘That’s funny …’"
 - Issac Asimov

<br>
<br>


More science is done my defining what things aren’t, much less likely are we to guess a relationship, a correlation, a gene sequence, or anything, than we are to guess what it isn’t.  This is the relationship between the null hypothesis (H0) and the alternative (Ha).  To turn the above question about discounts into a well phrase hypothesis statement we need to clarify things.  The null hypothesis is making as assumption that there is no difference between the control group and the experiment.  Conversely, we would describe the alternative as “different” “more than” “less than” and we could also use numbers to specify this difference.  We also want to state how much error we will allow in this test right from the get go.  Often times we allow for 5% error.
<br>
<br>
Succinctly written, the discount question would look like this:
<br>
<br>
H1:  Discounts have a significant increase on the number of products a customer orders to the 95% confidence level. orders with discounts sales price >= sales price orders without discounts.
<br>
<br>
H0: There is no increase in the number of products a customer will buy if they use discounts.  Orders with discounts total price = price of orders without discounts.
<br>
<br>

```
if p_value < 0.05:
    print(" HA = mu1 < mu2: We reject the Null.")
else:
    print(" H0 = mu1 == mu2: We fail to reject the Null.")
```
<br>
<br>
<a href="https://imgur.com/1Xb2JRZ"><img src="https://i.imgur.com/1Xb2JRZ.png" title="source: imgur.com" width="750" height="500" /></a>
<br>
<br>
What is a P value?
<br>
<br>
A P value is a standard measure of results for a given hypothesis test.  Often, the data is formed into a normal standard distribution (if not transformed, we simply assume that the data takes this shape)  A T test can be used on a standard normal distribution to get the corresponding P value from the output of the test.
<br>
<br>
If the P is smaller than the the degree of confidence we originally set in our hypothesis, we can reject the null hypothesis.
<br>
<br>

```
#Two Sample T Test

mu1 = discount_.mean()
mu2 = no_discount_.mean()
std1 = discount_.std(ddof=1)
std2 = no_discount_.std(ddof=1)
n1 = len(discount_)
n2 = len(no_discount_)

num = (n1-1)*std1**2 + (n2-1)*std2**2
den = n1 + n2 - 2
pooled_std = np.sqrt(num/den)
print(f'Both SDs pooled into one number: {pooled_std}\n')

se = pooled_std*np.sqrt((n2+n1)/(n1*n2))
print(f'standard error: {se}\n')
## Calculate T stat

t = ((mu1-mu2)/se)
print(f'T Stat: {t}')


## Calculate p_value


p_value = 1. - st.t.cdf(t, ((len(discount_)+len(no_discount_))-2), 0, 1)
print(f'P Value: {p_value}')
```

Even having a hypothesis that our data approves of this passed test can still be misleading.

<a href="https://imgur.com/iRdZSjG"><img src="https://i.imgur.com/iRdZSjG.png" title="source: imgur.com" width="750" height="500" /></a>
<br>
<br>

I don’t like to rely on this overlap, but it gives an approximate visual of what the discount data looks like.  This is an example of effect size.

 What is Effect Size?

Effect size does a good job of showing just how different two samples that share the same variable are.  This can be useful because it makes of a good visualization. Related metrics such as:  Overlap, Superiority, Threshold and the Misclassification Rate can all be used to tell us more about samples we’re dealing with.

Perhaps the more widely used calculation for effect size is the Cohen’s d, it is the number of standard deviations between the two means.  The above visualization also assumes normal standard distribution and for some data sets would be entirely misleading.  Say for example some sample is extremely skewed so that it’s mean and mode are huddled real close to the lowest limit.  In reality, the overlap would be less in this case because we can not assume equal standard deviation on either side of the mean like normal standard implies.<br>
<br>

```
def overlap_superiority(group1, group2):
    """Estimates overlap and superiority based on a sample.  Level Up: echo this."""
    n = len(group1 + group2)

    # Get a sample of size n from both groups
    mu1 = group1.mean()
    sd1 = group1.std(ddof=1)
    mu2 = group2.mean()
    sd2 = group2.std(ddof=1)
    group1_rv = scipy.stats.norm(mu1, std1)
    group2_rv = scipy.stats.norm(mu1, std1)
    group1_sample = len(group1)
    group2_sample = len(group2)
    
    # Identify the threshold between samples
    thresh = (mu1 + mu2) / 2
    print(f'     Simple Threshold: {int(thresh)}\nThe midpoint between two means.')
    
    # Calculate no. of values above and below for group 1 and group 2 respectively
    above = sum(group1 < thresh)
    below = sum(group2 > thresh)
    
    # Calculate the overlap
    overlap = (above + below) / n
    
    #calculate misclassification rate
    
    misclassify = overlap / 2
    
    # Calculate probability of superiority
    superiority = sum(x > y for x, y in zip(group1, group2)) / n
    print(f'''\n     Overlap: {round(overlap,4)}\nThe total AUC.\n
    Superiority: {round(superiority*100,2)}\nProbability that a randomly chosen sample from the first group is [higher] than one of the second group.\n
    Misclassification Rate: {round(misclassify,4)}\nThe chance of misclassification if using this metric alone as a predictor.''')
    

    return overlap, superiority
```

<br>
<br><br>
<br>


```
def Cohen_d(group1, group2):

    # Compute Cohen's d.

    # group1: Series or NumPy array
    # group2: Series or NumPy array

    # returns a floating point number 

    diff = group1.mean() - group2.mean()

    n1, n2 = len(group1), len(group2)
    var1 = group1.var()
    var2 = group2.var()

    # Calculate the pooled threshold as shown earlier
    pooled_var = (n1 * var1 + n2 * var2) / (n1 + n2)
    
    # Calculate Cohen's d statistic
    d = diff / np.sqrt(pooled_var)
    
    return d
```
<br>
<br>
Hypothesis testing is covered in greater detail in this <a href= "https://github.com/MarkEhler/dsc-2-final-project-online-ds-pt-112618" GitHub</a> repo.  This code was developed while working through the <a href = "https://flatironschool.com/" Flatiron School </a> Data Science program.  I welcome anyone who found themselves here to drop by my <a href= "https://github.com/MarkEhler" GitHub </a> profile and explore what I’ve been working on.



<br>
<br>
<a href="https://imgur.com/XlooOx7"><img src="https://i.imgur.com/XlooOx7.png" title="source: imgur.com"width="750" height="500" /></a>
<br>
<br>


The Hypothesis Tests:


Discounts orders ship more product than those without.

    Discounts by % > Without Discounts.

Northwind Traders has a preferred shipper by total order $$.

    Northwind Traders has a budget shipper by cheapest shipping per order.

Meat Orders are $100 more than other categories on average.

    Meat orders are stocked with more units than other categories.

The West Region earns more $$ than other regions.

    San Francisco ships 80% of the western orders by quantity.


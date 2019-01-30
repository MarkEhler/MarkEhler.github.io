---
layout: post
title:      "Wrapped Together"
date:       2019-01-30 13:46:15 +0000
permalink:  wrapped_together
---


While following a Learn.co tutorial designed to teach us how much more efficent Numpy is than pain-stakingly writing your own stand-in functions in Python (didn't have to pursuade me).   It was using the timeit.timeit() method to measure two identical large array manipuations against eachother.

I know how to do this.  You put each snippit of code in their own cell and run them with 

```
%%time
```

slapped on top.

So I struggled to make sense of why I should wrap all this in a function and tie it off with a pretty print statement.  I did come across an interesting snippit as I was reading.  Something I had heard of before in relation to OO programming.  The wrapper function.  Such an elegant thing.   It's just--something about it's sleekness.

Here's how they used it: 

```
def wrapper(func, *args, **kwargs):
    def wrapped():
        return func(*args, **kwargs)
    return wrapped
```


One reason we might want to do this is that we can eliminate the arguments for the function and then pass the wrapped version on to another function sans the messy arguments.  This is better than writing it just as a function for the fact that you can use this method on classes as well.  Your wrapper functions can benefit form multiple inheritance and therefor make testing and adaptation of your code run more smoothly.  Chalk this one up to common practice when building OO modules.

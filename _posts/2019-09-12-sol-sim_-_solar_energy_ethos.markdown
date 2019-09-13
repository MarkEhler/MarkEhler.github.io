---
layout: post
title:      "Sol-Sim - Solar Energy Ethos "
date:       2019-09-12 11:09:47 -0400
permalink:  sol-sim_-_solar_energy_ethos
---



Sol-Sim is a sunlight simulation that was developed to clear the cloudy skies obscuring the high skies of solar energy.   This educational tool should help people  gain more confidence in an eventual commitment or simply educate more  people about solar energy. Sol-Sim is an honest attempt to simulate the output of a solar panel without any expectations that the user invest and so hopefully can give an unbiased look into some of the key factors which can determine the effectiveness of a solar array.
<br>
<br>
currently you can view this simulator [here ](https://sol-sim.herokuapp.com/)
<br>
<br>

The domain of solar energy right now has reached a new dawn of effectiveness.  Solar arrays are cheaper to build than ever and despite the long wait for their financial payoff, offer other added benefits that some investors find appealing.  To talk about renewable energy is to talk about the state of the national electricity grid and a revolutionary way of thinking about power.  While developing this app I read two influential books.  Part 1 of [The Fifth Risk](https://www.goodreads.com/book/show/40109421-the-fifth-risk) and the entire text (think textbook) of [The Grid](https://www.goodreads.com/book/show/26073005-the-grid) give great insight into the current state of the industry at the times of their publishing.  For those of you without a couple extra days to spare I’ll sum things up for you.
<br>
<br>
In my opinion, electrical power has added itself to the list of basic human rights in the 21th century and the American electrical infrastructure is a national weak spot.  Many of our facilities are aging and we experience more blackout hours than any other developed nation.  Renewable energy already has an important role to play in powering many of our cities and while it is a remarkable benefit to harness this boon given to us by the world we inhabit, our existing way of doing things struggles to keep in touch with a harmonic balance of power.  What’s more, there is considerable money invested in doing things the old way and extracting our energy and keeping that power in the control of a few key gate-keepers (or should we say breaker-switchers).
<br>
<br>
However, thanks to new technological innovations the scales are beginning to tip in favor of a new, more robust energy grid.  Affordable solenoids, batteries, the addition of a virtual component to the grid, and most paramount, renewable energy plants, have become key players in the revolution.    
<br>
<br>
While better suited for this harmonic existence with our planet, renewable energy can seem unpredictable at first glance.  With this in mind I am interested in using data to give a statistical representation of what solar energy can do.
<br>
<br>
At its heart, Sol-Sim uses years worth of government data gleaned from solar test sites across America to record and observe changes in the power received from sunlight.  The key components in generating solar energy aren't immediately apparent.  The sceptical observer might point to cloud cover ruling our their area code for solar energy.  They are probably wrong.
<br>
<br>
I have found two things in my time working with weather data on this project and my [naive weather predictor](https://markehler.github.io/).  The first, humans are absolutely awful and keeping an accurate mental record of the observed weather. The second, complicated **astronomy math is the most important factor** in determining the output of a solar array.
<br>
<br>
  We can easily bypass an astronomy lecture by thinking of a solar window.  This window is an imaginary box in the sky.  The box is different depending on your location on Earth and inside of it the sun likes to shine down on you for most of the time, most of the year.  While the sun is inside this solar window is when plants, sunbathers, and solar arrays are most effective at what they do.  The goal of all these things should be to trace a short and direct path from themselves and the sun.  That is to say, a solar array will be most effective when positioned correctly and when experiencing direct sunlight.<br>
![](http://)
<a href="https://imgur.com/idvVoNZ"><img src="https://i.imgur.com/idvVoNZ.png" title="solar window" width="300" height="250", STYLE = "float: right;" /></a>
<br>
  So, if you had one day to spend getting a tan, spend it on the summer solstice, when the sun is most directly overhead, closest to you, and lighting the sky for the longest amount of time.  The majority of **this app is designed to register the directness of sunlight** adjusted for what the weather might be on a given day in a given place.  Although I’ve done some math on the outputs to help give more information to the user, in its current state, I have no way of checking to make sure that the angle, direction and surrounding trees concerning a solar array site (or rooftop) are best suited for panels.
<br>
<br>
<br>
  I would like to improve this app in the future to look at the user coordinates of the site using computer vision to check these factors and return a rating or heatmap of efficiency.  As is, this app wraps an XGBoost model in Flask framework and launched on Heroku to create my first attempt at a live app.  If you want to see more of my work you can find me on [GitHub](https://github.com/MarkEhler), on my blog.  Or feel free to reach out to me directly on [Linkedin](https://www.linkedin.com/in/mark-ehler-85052548/) with feedback or comments.
<br>
<br>
Have a sunny day<img src="https://pic.sopili.net/pub/emoji/twitter/2/72x72/1f31e.png" width=20 height=20>







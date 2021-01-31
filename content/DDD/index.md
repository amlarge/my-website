---
title: An example title
summary: Here we describe how to add a page to your site.
date: "2018-06-28T00:00:00Z"

reading_time: false  # Show estimated reading time?
share: false  # Show social sharing links?
profile: false  # Show author profile?
comments: false  # Show comments?

# Optional header image (relative to `static/media/` folder).
header:
  caption: ""
  image: ""
---

# Diners, Drive-Ins, and Lies:
## Guy Fieri's Most Commonly Visited Restaurants

The Food Network Show "Diners, Drive-Ins, and Dives" has been a television staple since 2007. Since then, Guy Fieri and crew have been to over 1,000 restaurants in the United States, and even some international locations. Not even COVID-19 can stop the Mayor of Flavortown: current episodes of the show feature the restaurants sending their food to him. The premise of the show was simple: to celebrate the unsung heroes of the dining scene: your greasy spoons, dimly lit hometown hangouts, and the remnants of mid-century Americana. However, a more recent viewer of the program may be left confused, as it seems that the premise has been abandoned. I was interested in finding this out.

\* Note: I realize it has been a meme for quite some time to give Guy Fieri grief. So I do want to state that this is by no means an attempt to drag Mr. Fieri through the mud. He has done an admirable job highlighting many hidden gems across the country and has built deep relationships with many of them. I do not want to diminish that.

Acquiring a list of locations visited is actually quite straightforward. There are two sources of information: Food Network's own website (https://www.foodnetwork.com/restaurants/shows/diners-drive-ins-and-dives) and a fan site (https://www.dinersdriveinsdiveslocations.com/). Both list the restaurant  name, address, and a brief description. I chose the latter, because it also provided season numbers for each location. 

I then scraped the website for all locations in the United States and Canada (I avoided international locations, given that dining cultures there might be such that the concept of a "dive" would be an entirely different concept). 

I would then perform some text analysis of each restaurants description, looking for descriptive words that might describe a restaurant. This included diners, drive-ins, dives, cafes, bars, bbq, pubs, brunch, bakerys, and tex-mex. Beyond the Big 3, these groups came from eyeballing the descriptions. 

It is also worth noting any given restaurant could be put into multiple categories.


In the end, I had a list of 1185 restaurants.

A list of categories and their counts can be found here:

However, I was interested in how these have changed over time. So I broke these down by season:


The major takeaway is that diners were by far the most common in early seasons, but have dropped in proportion since then. We also see that at this point, the types of restaurants visited are more varied.

However, before I declare "Guy Fieri is a liar!", I do want to point out that just because he doesn't visit many drive-ins, there are two things to keep in mind:

1. There aren't many drive-ins period
2. Guy Fieri might have been to more drive-ins than most other Food Network shows, such that if you see a drive-in on TV, it's most likely DDD.

To that end, I tried out this analysis on another show, Michael Symon's "Burgers, Brew, and 'Que", which not only should have a high overlap on types of restaurants visited, but clearly took inspiration from Guy Fieri on its title.


Thankfully, we do see that bars, bbq are pretty common words used in describing places Michal Symon goes, but drive-ins are not.

#### Considerations and Future Work

This analysis can be improved in a few ways. For one, the keywords I used to search are probably underestimating the quantity of restaurants visited (For example, the word "dive" many not show up in a restaurant's name or description"). Still, after hand-checking, I do catch a majority of what should be categorized. One way to improve categorization is linking the restaurants to databases like yelp, which would provide a wealth of information about a restaurant. For example, user reviews would give a hint as to the quality of restaurant, such as it's "diveyness." It also would provide information such as pricing, noise level, and other attributes. 

Furthermore, this could be extended out to how restaurant descriptions might vary between show. We see that Michael Symon goes to different restaurants, but it might be possible that word choice might vary between shows. Preliminary analysis shows that Burgers, Brew, and 'Que may use more poetic words like "succulent" in their descriptions. 

Ultimately, Diners, Drive-Ins, and Dives is not different from any other long-running show: While keeping their premise early on, but evolves and changes over time. And if it has a strong brand, it wouldn't make sense to change the name, would it? As long as the quality of restaurants hasn't decline (and it doesn't seem like that's the case), we can accept that Guy Fieri has broadened the scope of the show. 

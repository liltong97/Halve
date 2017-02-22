---
layout: post
title: "Exploring d3"
categories:
  - Metis
  - d3
tags:
  - Metis

---
I'm so amazed by the graphics produced by d3. Here's my first attempt...

### Desserts First
**Mean Changes in IUCN Status By Country:**
<iframe src="https://cdn.rawgit.com/liltong97/1d5fbba13346bfba0d57733941731bc0/raw/5bdb1ffe8298a318b390cc3071312178d2adcb3e/index.html" width="100%" height ="400px" marginwidth="0" marginheight="0" scrolling="no" class="d3_map"></iframe>

(If you're on mobile, this is horribly broken and for that I apologize. It's definitely a work in progress.)

### What's an IUCN Status?
The International Union for Conservation of Nature (IUCN) is the organization that determines which species are considered "endangered" or not. In fact, there are 7 categories an animal can be placed in (ranging from least concern -> extinct). 

<center><img src="https://upload.wikimedia.org/wikipedia/commons/thumb/1/18/Status_iucn3.1.svg/500px-Status_iucn3.1.svg.png" width ='80%' alt="IUCN categories" ></center>

### Where is this data from?
For my next Metis Project I am building a model to predict whether a species has changed in its IUCN status since the last time the species was surveyed. I will also be exploring if the model can predict whether or not the species had improved for the better or worse. 

The subset of species that I am using for my model are either mammals, birds, or reptiles and I only surveyed species that reside in countries that I could also obtain climate and forest area data for. These are the species that are mapped on the d3 map above.

### So what do the numbers mean?
What I call the "Mean IUCN Status Change" is taking the average of how many categories a species has moved for all species in that country. So for species that have declined by two categories (e.g. from near threatened, NT -> endangered, EN), they get a score of +2. For species that have improved by one category (e.g. from near threatened, NT -> least concern, LC), they get a score of -1. Averaging the scores of all the the species in the country gives you the "Mean IUCN Status Change."

Notice that the mean values are all extremely close to 0. The truth is, the majority (~94%) of species that I used for my model did not change at all in status since the last time they were surveyed by the IUCN, meaning they got a score of 0. My modeling steps became an exercise in dealing with skewed/imbalanced data--good practice for sure!

### Conclusions
More blog posts coming up about the modeling steps for this data set and the struggles of putting the d3 map together and displaying it on this page! Stay tuned...




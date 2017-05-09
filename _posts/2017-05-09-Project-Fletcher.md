---
layout: post
title: "Project Fletcher"
categories:
  - Metis
tags:
  - Metis
  - Data Science
  - Project

---
I've been behind on posting about these projects, but here's the fourth one on NLP called Project Fletcher.

### Project Goal
The goals for this project were pretty open ended. We just needed to use NLP in some form or fashion. For my project I decided to use it to make a job posting recommender that will aggregate postings from other job sites. What's unique about my recommender is that you can input huge blobs of text (like excerpts from your resume and job descriptions from another job posting that you liked) and it will provide you with postings most similar to what you inputted. Try putting huge blobs of text in other job sites and you'll see that it doesn't work so well!

### What jobs should I be looking for?
The first challenge of the project was coming up with a comprehensive list of available jobs out there so I knew what to search for. Unfortunately there doesn't exist a list of all possible job titles out there so I made best use of what was available--[wikipedia's list](https://en.wikipedia.org/wiki/Lists_of_occupations). I crawled through the different links on wikipedia's page to get a list of recognized job titles that I could then search through other job sites to get listings. Clearly this list is going to be heavily biased towards what's available on wikipedia, but from a general glance it seems to hit a lot of the major professions. 

### Aggregate job postings
After I had my list of jobs, I inputted them one by one into careerbuilder.com and indeed.com and scraped information from the job listings (mainly the title, description, and link to the posting) using BeautifulSoup from all the result pages that came up from each job. All the job information was dumped into a MongoDB that I had set up on AWS. 

Some job titles that I pulled from wikipedia were relatively obscure (who's ever had a job as a [Dramaturg](https://en.wikipedia.org/wiki/Dramaturge)) so I ended up throwing out jobs where there had less than 10 results from careerbuilder and indeed combined. 

### NLP
After I had all my documents, I used a bit of NLP to generate a way to suggest a job posting to someone given what they input. First, I took all the documents and used term frequency-inverse document frequency (TF-IDF) to convert the whole corpus into a matrix where each row represents a different job posting and the columns are now individual words found in all these documents. The entries are a number that indicate how frequent a word appears in that job posting but normalized by how often that word is found in the whole corpus of job postings. This way, words like "job", "hire", "company" are given very very little importance because they're practically found in every posting, while words like "data", "PCR", "java" are given stronger weight which can give us an idea of "topics" or themes that exist throughout all the documents given the distribution of weights due to the language used in each posting. 

To pull the most important "topics" out of this matrix we can use something called latent semantic analysis (LSA) which is basically a dimensionality reduction technique so instead of looking at individual words, you can look at the combination of words, their weights, and how it might contribute to what the posting is about. It makes the information more dense and rich, but you will be throwing away some of the nitty gritty that might not contribute much (e.g. ignoring some words that don't contribute much to any topic). Reducing it with LSA will also allow us to provide suggestions faster because the LSA matrix will be more lightweight than the full TFIDF matrix, which is an important trade-off to consider. 

How do we know how many of these "topics" we want? We can look at the explained variance and how much information we are gaining given each additional topic. The graph for our corpus is shown below:
![LSA explained variance]({{ site.url }}/images/project_fletcher/lsaexplainedvar.png)

We see that around 20 "topics", we aren't really gaining a significant amount of extra information about what makes some postings a different "theme" than others, so we can cut it off there. So now we can transform each job postings to be a combination of these 20 "topics" as opposed to just a bag of words of different frequencies. 


### The recommendation
Now with these matrices handy, to provide a recommendation we can first just take what the user inputted, throw it through the same transformations as we did the original documents (through TF-IDF and then through LSA). Now that we have matching columns, we can use a cosine-similarity which is essentially a dot product between the matrix of the inputted information and of each of the documents. We can then identify which documents give the largest dot product (implying highest similarity in terms of topic information) and then output those documents and provide the job posting link to the user so that they can apply or figure out whether or not it truly is a good fit. 

### Conclusions
Overall, this was a really interesting project and cool to build something that other job posting websites haven't prioritized. I think there is also a lot of potential in terms of improving it such that it can use NLP to automatically identify things like years of experience needed or if advanced degrees are needed so that we could implement sort features to help with that kind of thing. You could even look for more fuzzy filtering options like whether it's something like a "fast-paced environment" or a "diverse team."





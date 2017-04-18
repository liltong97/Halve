---
layout: post
title: "d3 First Steps"
categories:
  - d3
tags:
  - d3

---
Here are a few websites and tips to help people get started with d3!

### Don't start from scratch
My god, whatever you do, if you're new to d3 I would highly recommend **NOT** starting from scratch. There are a lot of components of a d3 visualization that need to all function correctly together for a pretty figure to show, and it can be a really big monster to debug if you're just starting off. 

Also, just a heads up, you will need to know at least a little bit of javascript because d3 (i.e. D3.js) is a javascript library. This means you probably also want to know enough about html and how javascript and html interact to be able to put your d3 visualization onto a webpage, or something like that. 

### So where do I start?
If you have an idea for what kind of visualization you're looking for, you can search for it on [blockbuilder.org](http://blockbuilder.org/search). This website is also super amazing for people working on d3 visualizations for reasons I will go into in a just a bit.

If you don't know what you're looking for feel free to browse through Mike Bostock's (the creator of d3) [examples](https://bl.ocks.org/mbostock). You can also look through this massive [portfolio](http://bl.ocks.org/enjalot/raw/211bd42857358a60a936/). 

### OK, I have my template.
Nice, so you can test any sorts of edits or modifications to the visualization that you picked to make it your own. The cool thing about using bl.ocks.org or blockbuilder.org is that you can browse and look at the visualizations if the base url is https://bl.ocks.org/ (like https://bl.ocks.org/blahblahexamplevis), but if you just replace the base url with blockbuilder.org/ (like blockbuilder.org/blahblahexamplevis; note you do not include the https://), it will take you to a page where you can edit the code for blahblahexamplevis and give you options to upload your own data, etc. 

It will also show you your changes to the visualization as you make them, so it really helps you figure out which part of the d3 you're changing and how broken it is! But, wait, there's more! Log into your github, save your edits, and you can change the base url to https://gist.github.com to find all the files you added and edits you made to the original d3 visualizations saved with your github account. Sick. 

Also, if any of that was unclear, they explain it on their [homepage](http://blockbuilder.org/) with videos and stuff too! 


### Getting it onto a website
So I also haven't gotten this completely solved, because I'm having issues with getting my d3 to display properly on mobile. But, the initial steps that I had to do to get it on my website, was to serve my gist up using [rawgit.com](http://rawgit.com/). And then, when you have your new served-up link, you can put it in an iframe on the website you're hoping to display it on. Just as an example, this is what I have for my d3 post:

```
<iframe src="https://rawgit.com/liltong97/1d5fbba13346bfba0d57733941731bc0/raw/413fd92c0b9e1d8ad82c328a3c12a703f7767fd1/index.html" width="100%" height ="400px" marginwidth="0" marginheight="0" scrolling="no" class="d3_map"></iframe>
```

Something extra tricky, however, is that if your d3 visualization uses packages/scripts that are files (as opposed to links), then you have to host those files up on separate rawgits and make sure that your d3 rawgit references the script rawgits. Or else, it won't have all the packages needed to run the visualization properly (FYI, that took forever for me to figure out....).

### All in all
I think d3 visualizations are very very time consuming and painful at times to get right, but definitely quite rewarding at the end of it. I'm still learning a lot about it for myself, so would love tips if you have them! Good luck to those starting out as well! 







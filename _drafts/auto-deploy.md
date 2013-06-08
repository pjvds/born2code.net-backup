---
layout: post
title: "auto deploy your blog"
categories: [programming]
keywords: [auto deploy, wercker, continuous integration]
---

In my [last blogpost](/blog/2013/05/27/simplify-your-jekyll-publishing-process-with-wercker/) I described how I used [wercker](http://wercker.com) to automate the publishing and deployment process of this blog. Since then I've fixed some typo's from my iPhone and I used it to publish draft posts to a staging area of my blog. The later helps me to increase the quality of my posts and allows others to proof read in the final form before they get published. You can expect a blogpost about this in the near future. Now I want to share that [wercker introduced auto-deploy](http://blog.wercker.com/2013/06/05/Autodeployment.html) and how this feature changed the way I blog.

## Auto Deployment

Whenever I push changes to my github repository wercker will start building my website. Now with the new auto deployment feature it will also deploy the changes if:

* The build is successful (green)
* The build is from a branch that matches the auto deploy branch list

I enabled the feature in the deployment target that I have created at [wercker](http://wercker.com), here is how it looks like:

![image]({{ '/auto-deploy-jekyll/deploy-target.png' | asset_url }})

You can see I enabled auto deploy for this target and configured it to only deploy successful build from the master branch. All deployment targets that have auto deploy enabled show the auto deploy icon as you can see just below the deployment target name.

## How did this change my workflow

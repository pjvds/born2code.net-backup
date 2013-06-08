---
layout: post
title: "auto deploy your blog"
categories: [programming]
keywords: [auto deploy, wercker, continuous integration]
---

In my [last blogpost](/blog/2013/05/27/simplify-your-jekyll-publishing-process-with-wercker/) I described how I leverage [wercker](http://wercker.com) to automate the publishing and deployment process of my blog. Since then I've fixed typo's from my iPhone and I used it to publish draft posts to a staging area of my blog. This staging area helps me to increase the quality of my posts and allows others to proof read in the final form before they get published. You can expect a blogpost about this in the near future. Now I want to share that [wercker introduced auto-deploy](http://blog.wercker.com/2013/06/05/Autodeployment.html) and how this feature changed the way I blog.

## Auto Deployment

Whenever I push changes to my github repository wercker will start building my website. Now with the new auto deployment feature it will also deploy the changes if:

* The build is successful (green)
* The build is from a branch that matches the auto deploy branch list

I enabled the feature in the deployment target that I have created at [wercker](http://wercker.com), here is how it looks like:

![image]({{ 'auto-deploy-jekyll/deploy-target.png' | asset_url }})

You can see I enabled auto deploy for this target and configured it to only deploy successful build from the master branch. All deployment targets that have auto deploy enabled show the auto deploy icon as you can see just below the deployment target name.

## How did this change my workflow

Before I enabled this auto deploy feature I committed all my changes directly on the master branch. But since I have enabled auto deploy I shifted to a [feature branching model](http://nvie.com/posts/a-successful-git-branching-model/). Whenever I start a new post I create a feature branch. For this post I created a branch `auto-deploy-jekyll`. On this branch I commit my work in small commits that make sense in the context of writing that post. Hereby I create a lists all my changes including details why I changed something. This log helps me to pick up where I left if I do not work on a post for a couple of days. It also helps when I'm working on one with other people. I can read the reason why the other person changed something.

When a post is finished I merge it to the master branch and push the changes. This triggers a build at wercker and when the build succeeds it will be deployed to production.

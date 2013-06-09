---
layout: post
title: "auto deploy your blog"
categories: [programming]
keywords: [auto deploy, wercker, continuous integration]
---

In my [last blogpost](/blog/2013/05/27/simplify-your-jekyll-publishing-process-with-wercker/) I described how I leverage [wercker](https://app.wercker.com/project/bykey/c38587366b136b180eb7108c9c250cdc) to automate the publishing and deployment pipeline of my jekyll blog. Since then I have fixed typo's from my iPhone, used [Prose](http://prose.io/) for editing and introduced a staging area for my blog. This staging area helps me to review my posts in their final form. You can expect a blogpost with the details somewhere soonish. In this post I share that [wercker introduced auto-deploy](http://blog.wercker.com/2013/06/05/Autodeployment.html) and how this feature changed the way I blog.

## Auto Deployment

Auto deployment allows you to let your software flow to production (testing, or any other deployment target) as a stream of small and predictable changes. Before a changeset flows to production it gets challanged by an automated build. The goals of a build is not only to transform the source code to a deployable form, but also to challange its quality. It allows teams to be more agile and it makes you more aware to spend time on the quality checks in your build.

## Auto deploy my jekyll blog 

I have to deployment targets, staging and production. Both have auto deployment enabled. Here is how my production deployment target looks like at wercker:

![image]({{ 'auto-deploy-jekyll/deploy-target.png' | asset_url }})

You can see I enabled auto deploy for this target by checking the auto deploy checkbox. I configured it to only deploy successful builds from the master branch.
All deployment targets that have auto deploy enabled show the auto deploy icon as you can see just below the deployment target name.

Wercker was already building my website with every push to my [Github repository](https://github.com/pjvds/born2code.net) and with the auto deploy feature enabled it will now deploy the build under the following condition:

* The build is successful (green), which means that jekyll generated the site correctly.
* The build is from a branch that matches the auto deploy branch-list, in my case master.

## How did this change my workflow

Before I enabled the auto deploy feature I was committing all my changes directly to the master branch. With auto deploy enabled I moved to a [feature branching model](http://nvie.com/posts/a-successful-git-branching-model/). Before I start a new post I create a feature branch that will hold all the changes made before it reaches the final state. Final state means merged to the master branch so that is gets auto deployed.

For this post I created a branch `auto-deploy-jekyll`. Started working on it and committed my changes in small commits. These commits make sense in the context of writing that post. They create the path to the final state of the post. Next to the textual changes, they also keep the reasons behind the decision I made. For example why I dropped a certain example or why I edited the screenshot. This is extremely helpful for my future self or co-authors. It helps to pick up where I left if, especially when I do not work on a post for a couple of weeks.

After a post is finished I merge it to the master branch and push the changes to Github. This triggers a build at wercker and when the build succeeds it will be auto deployed to production. In other words, deploying is as easy as merging to master.
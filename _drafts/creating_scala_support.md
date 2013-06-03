# Creating Scala support

[Wercker](http://wercker.com) hosted the [Scala Hackathon](http://www.meetup.com/amsterdam-scala/events/116311362/). A meetup with only one goal; hacking scala code. Since people showed interesest in wercker during the [last meetup](http://www.meetup.com/amsterdam-scala/events/116300402/) I decided to do a short lightning talk to show wercker and tell what continuous delivery can do for you and your project.

## Open platform

One of the goals of wercker is to become an open platform to build, test and deliver software. This means users should be able to extend the platform easily. Before we can talk about adding Scala support, we need to understand some core concents of wercker.

Let us start by the build pipeline. This is roughly what wercker does everytime a developer pushes his code to git.

1. Cloning project repository from git.
2. Provision a box based on the project requirements.
3. Execute the build pipeline in this box.
4. Store pipeline output

## The box

A box is a virtual machine that is used to execute the build and deploy pipeline. Wercker provides boxes for different programming environments like python, ruby and nodejs. A box consists of an operating system and packages, for example, the ruby box consists of ubuntu 12.04, ruby 1.9.3 and other packages that are common for ruby development like webkitserver. 

Because wercker cannot create boxes for all possible situations they give users the option to create boxes themself. Creating a box is easy, in fact, it is nothing more than a simple yaml-file. This means you do not need to install any software to create your own box. Also contributing to an existing box is rather easy and users can leverage all the power from social coding platforms like Github and Bitbucket.

Let's start by creating the first part of the `wercker-box.yml` file that just contains the basic properties of the box.

	name: scala
	version: 0.0.1
	type: main

The box is named `scala` and has a version of `0.0.1` to define we are still just experimenting. The type property is set to `main`, which defines that this box should be used to execute a build and deployment pipeline. The other possible value would have been `service`, which we would have used if we were creating a service box, like: MySQL, RabbitMQ or MongoDB.

Because a lot of boxes have quite some things in comon 

For the Scala box I have the following requirements:

* Ubuntu 12.04 LTS
* OpenJDK 7
* Scala 2.10

I also want some packages to be available that are pretty common for software development in general, like: build-essentials, curl, git-core and imagemagick.


	apt-get install openjdk-7-jdk -qq
	
	sudo wget http://www.scala-lang.org/downloads/distrib/files/scala-2.10.0.tgz
	tar zxvf scala-2.10.0.tgz
	mv scala-2.10.0 /usr/share/scala
	
	ln -s /usr/share/scala/bin/scala /usr/bin/scala
	ln -s /usr/share/scala/bin/scalac /usr/bin/scalac
	ln -s /usr/share/scala/bin/fsc /usr/bin/fsc
	ln -s /usr/share/scala/bin/sbaz /usr/bin/sbaz
	ln -s /usr/share/scala/bin/sbaz-setup /usr/bin/sbaz-setup
	ln -s /usr/share/scala/bin/scaladoc /usr/bin/scaladoc
	ln -s /usr/share/scala/bin/scalap /usr/bin/scalap

	wget http://repo.typesafe.com/typesafe/ivy-releases/org.scala-sbt/sbt-launch//0.12.3/sbt-launch.jar
	printf 'java -Xmx512M -jar `dirname $0`/sbt-launch.jar "$@"' > sbt
	chmod +x ./sbt
	sudo mv sbt /usr/share/sbt
	sudo mv sbt-launch.jar /usr/share/sbt-launch.jar
	sudo ln -s /usr/share/sbt /usr/bin/sbt
	sudo ln -s /usr/share/sbt-launch.jar /usr/bin/sbt-launch.jar
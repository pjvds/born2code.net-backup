# Creating Scala support

Tomorrow [wercker](http://wercker.com) will host the [Scala Hackathon](http://www.meetup.com/amsterdam-scala/events/116311362/). A meetup with only one goal; hacking some scala code. Since people showed interesest in wercker during the [last meetup](http://www.meetup.com/amsterdam-scala/events/116300402/) I decided to do a short lightning talk to show wercker and tell what continuous delivery can do for you and your project. But here is the catch, at the time of writing wercker does not support Scala out of the box. So let us find out how we can add this.

## Open platform

One of the goals of wercker is to become an open platform to build, test and deliver software. This means users should be able to extend the platform easily. Before we can talk about adding Scala support, we need to understand some core concents of wercker.

Let us start by the build pipeline. This is roughly what wercker does everytime a developer pushes his code to git.

1. Cloning project repository from git.
2. Provision a box based on the project requirements.
3. Execute the build pipeline in this box.
4. Store pipeline output

## The box

After the code is cloned and checked out a box is provisioned. Then the build pipeline is executed inside this box. For Scala we need a box that has all the basics for an Scala environment installed (OpenJDK 7, SBT)

	apt-get install openjdk-7-jre -qq
	
	$sudo wget http://www.scala-lang.org/downloads/distrib/files/scala-2.10.0.tgz
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
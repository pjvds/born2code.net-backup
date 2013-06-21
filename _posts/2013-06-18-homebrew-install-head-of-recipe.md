---
layout: post
title: "Get the libgit2 go language bindings working"
categories: [programming]
keywords: [homebrew, brew, install, HEAD]
---

Today I want to read some [git](http://git-scm.com/) repository information from an application written in Google's [Go](http://golang.org/). Luckilly for me there are Go bindings available for [libgit2](http://libgit2.github.com/) called [git2go](http://github.com/libgit2/git2go). And here is where the problem started.

I already have libgit2 installed, so a simple `go get` of the git2go package is the first and list thing I need to do before I can read git repository information from my Go application. So I execute the following:

    $ go get github.com/libgit2/git2go
    # github.com/libgit2/git2go
    error: 'git_packbuilder_insert_commit' undeclared (first use in this function)
    error: (Each undeclared identifier is reported only once

What, an error?! I guess that the libgit2 that I have installed is outdated an try to update it. I use [homebrew](http://mxcl.github.io/homebrew/) on my MacBook so updating it is simple:

    $ brew upgrade libgit2
    Error: libgit2-0.18.0 already installed

Time to read the docs, something I should have done in the first place anyway. The [README](https://github.com/libgit2/git2go#git2go) from git2go states that it needs to have top-of-the-branch libgit2. Time to remove the libgit2 installation and install the `HEAD` version which points to the `development` branch of `https://github.com/libgit2/libgit2.git`:

    brew uninstall libgit2
    brew install libgit2 --HEAD

The `--HEAD` options will install the bleeding edge top-of-the-branch version of a tool. Note that not all homebrew recipes offer such a version, but in my experience most of them do.

I just need to execute another `go get github.com/libgit2/git2go` and I'm ready to go!

I have [SublimeText](http://sublimetext.com) with the [GoSublime](https://github.com/DisposaBoy/GoSublime) package installed that turns SublimeText into my favorite Go IDE. Time to write a short test to make sure that git2go works and that I understand its API correctly.

{% highlight go %}
package main

import (
  "github.com/libgit2/git2go"
  "testing"
)

func TestCanGetHead(t *testing.T) {
  repo, err := git.OpenRepository(".")
  defer repo.Free()
  if err != nil {
    t.Errorf("Could not open repository: %s", err.Error())
  }

  headRef, err := repo.LookupReference("HEAD")
  defer headRef.Free()
  if err != nil {
    t.Errorf("Could not read HEAD: %s", err.Error())
  }

  if headRef.Name() != "HEAD" {
    t.Errorf("expected ref name to be HEAD instead of %s", headRef.Name())
  }

  if headRef.Type() != git.SYMBOLIC {
    t.Errorf("expected HEAD to be a symbolic reference instead of %v", headRef.Type())
  }

  ref, err := headRef.Resolve()
  defer ref.Free()
  if err != nil {
    t.Errorf("Could not resolve HEAD ref: %s", err.Error())
  }

  commitId := ref.Target()
  t.Logf("HEAD is at %s", commitId.String())
}
{% endhighlight %}

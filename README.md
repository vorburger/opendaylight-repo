# opendaylight-repo

http://OpenDaylight.org sources managed via [Repo (from Android)](https://source.android.com/setup/developing#repo), see [Using repo command reference](https://source.android.com/setup/using-repo).

_Please Star and Watch this project on GitHub if it's of interest to you!  Contributions most welcome._

## How to use

Install [the Repo tool](https://raw.githubusercontent.com/vorburger/opendaylight-repo/master/repo) doing something like this:

    mkdir ~/bin
    PATH=~/bin:$PATH
    curl https://raw.githubusercontent.com/vorburger/opendaylight-repo/master/repo > ~/bin/repo
    chmod a+x ~/bin/repo

Now, typically from a directory inside which you may already have like an `odlparent/` or `netvirt/` etc. for (some) ODL projects' git repos, do:

    repo init -u https://github.com/vorburger/opendaylight-repo

You now have the sources of all of ODL's projects, and can:

    TBD...

## Why?

As of the start of 2018, it is a huge PITA to drive change accross the [many individual Git repos that comprise ODL](https://git.opendaylight.org/gerrit/#/admin/projects/).  Git sobmodules as used in ODL's releng-autorelease don't really adequately address the challenge.

Series of changes (pseudo) "linked" through Gerrit topics, and listed on [Wiki pages for Big Changes](https://wiki.opendaylight.org/view/ODL_Root_Parent:BigBumpOfJan2018) are a (very) Poor Man's solution to this problem.

Isolation of projects with separate lifecycle and releases sounds like a good idea at first, but then leads to a really slow rate of adopting change (worst for the "lowest" layered projects; not a problem in "top" projects), and much pain when you bump version to switch over to new dependencies.

A huge single git repo would be neat, but is "organizationally" challenging... but who knows, maybe we'll get (back) there eventually? ;) Part of the more social than technical problem could probably be solved, e.g. through the [Gerrit OWNERS Plugin](https://gerrit.googlesource.com/plugins/owners/+/refs/heads/master/README.md).  In the short term, using this tool gives you "single Git repo like" experience.

Read these if you need more background why working like this is a Great Idea:
* https://cacm.acm.org/magazines/2016/7/204032-why-google-stores-billions-of-lines-of-code-in-a-single-repository/fulltext
* https://mike-bland.com/2011/12/02/coding-and-testing-at-google-2006-vs-2011.html

## What else & more...

The `./repo` Python script included here is [the unmodified one obtained from Android](https://source.android.com/setup/downloading#installing-repo). This script is a "bootstrap" kinda thing; upon the initial `repo init` it will clone more of itself to `.repo/repo/repo`, and then delegate to that.

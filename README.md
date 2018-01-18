# opendaylight-repo

http://OpenDaylight.org sources managed via [Repo (from Android)](https://source.android.com/setup/developing#repo).

## How to use

_TBD_

## Why?

As of the start of 2018, it is a huge PITA to drive change accross the [many individual Git repos that comprise ODL](https://git.opendaylight.org/gerrit/#/admin/projects/).  Git sobmodules as used in ODL's releng-autorelease don't really adequately address the challenge.  

Series of changes (pseudo) "linked" through Gerrit topics, and listed on [Wiki pages for Big Changes](https://wiki.opendaylight.org/view/ODL_Root_Parent:BigBumpOfJan2018) are a (very) Poor Man's solution to this problem.

Isolation of projects with separate lifecycle and releases sounds like a good idea at first, but then leads to a really slow rate of adopting change (worst for the "lowest" layered projects; not a problem in "top" projects), and much pain when you bump version to switch over to new dependencies.

A huge single git repo would be neat, but is "organizationally" challenging... but who knows, maybe we'll get (back) there eventually? ;) Part of the more social than technical problem could probably be solved, e.g. through the [Gerrit OWNERS Plugin](https://gerrit.googlesource.com/plugins/owners/+/refs/heads/master/README.md).  In the short term, using this tool gives you "single Git repo like" experience.

Read these if you need more background why working like this is a Great Idea:
* https://cacm.acm.org/magazines/2016/7/204032-why-google-stores-billions-of-lines-of-code-in-a-single-repository/fulltext
* https://mike-bland.com/2011/12/02/coding-and-testing-at-google-2006-vs-2011.html 

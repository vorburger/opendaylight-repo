# opendaylight-repo

http://OpenDaylight.org sources managed via [Repo (from Android)](https://source.android.com/setup/developing#repo), see [Using repo command reference](https://source.android.com/setup/using-repo).

_Please Star and Watch this project on GitHub if it's of interest to you!  Contributions most welcome._

## How to use

Install [the Repo tool](https://raw.githubusercontent.com/vorburger/opendaylight-repo/master/repo) doing something like this:

    mkdir ~/bin
    PATH=~/bin:$PATH
    curl https://raw.githubusercontent.com/vorburger/opendaylight-repo/master/repo > ~/bin/repo
    chmod a+x ~/bin/repo

Now do this to have it `git clone` all of ODL's projects (in 4 parallel download threads):

    repo init --config-name -u https://github.com/vorburger/opendaylight-repo
    repo sync

[`repo start`](https://source.android.com/setup/using-repo#start) can now create branches. By default it only affects the git repo of the current project, but by being in the root directory and using `*` we can branch all of them:

    repo start your-next-cool-cross-project-thing *

[`repo diff`](https://source.android.com/setup/using-repo#diff) and [`repo status`](https://source.android.com/setup/using-repo#status) check changes across *all* projects' repos, not just the repo of the current working directory:

    repo status
    repo diff

[`repo sync`](https://source.android.com/setup/using-repo#sync) fetches and rebases, again on *all* projects:

    repo sync

Before we can push to Gerrit, we still `git commit`, as always, using [`repo forall`](https://source.android.com/setup/using-repo#forall) from the root directory like this (or with `--amend`):

    repo forall * -c git commit -asm "Great new stuff"

[`repo upload`](https://source.android.com/setup/using-repo#upload) will push local changes to Gerrit:

    repo upload

NB: _If you run repo upload without any arguments, it will search all the projects for changes to upload._ So perhaps not something you want to run on your existing ODL git clones? ;-) Not a problem if you used repo init & sync on a fresh location, of course.

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

Check out [Troubleshooting network issues](https://source.android.com/setup/downloading#troubleshooting-network-issues) from Android's doc, if you have any.

[opendaylight-eclipse-setup](https://github.com/vorburger/opendaylight-eclipse-setup) is an unrelated project you may like as well?

## How to build full ODL autorelease

This isn't really directly related to Repo, but useful in this context... ;-)

    sudo dnf install xmlstarlet

    ./releng/autorelease/scripts/fix-relativepaths.sh

    rm -rf ~/.m2/repository/org/opendaylight/

    mvn -Pq -T 1.5C -s ~/.m2/settings-odl-no-snapshot.xml -am -pl controller/opendaylight/archetypes/opendaylight-startup/ clean install

The settings-odl-no-snapshot.xml is a copy of the ODL settings.xml (`cp ~/.m2/settings.xml ~/.m2/settings-odl-no-snapshot.xml`) with line 78 for `<activeProfile>opendaylight-snapshots</activeProfile>` removed.

The Maven `-am` option (AKA `--also-make`) used above also builds projects required by `-pl`; its opposite is `-amd` (AKA `--also-make-dependents`) to also build projects that depend on the `-pl` project, that is useful to determine cross project impacts, for example:

    mvn -T 1.5C -amd -pl infrautils/testutils clean install

## How to build ODL on a fresh CentOS

This isn't really directly related to Repo, but documented here anyway... ;-)

    sudo yum install -y java-1.8.0-openjdk-devel git zip unzip wget nano epel-release xmlstarlet
    curl -s "https://get.sdkman.io" | bash
    logout & login (or source "$HOME/.sdkman/bin/sdkman-init.sh")
    sdk install maven 3.5.2
    wget -q -O - https://raw.githubusercontent.com/opendaylight/odlparent/master/settings.xml > ~/.m2/settings.xml
    echo 'MAVEN_OPTS="-Xmx4096m -Xverify:none"' >>~/.bashrc
    logout & login (or export MAVEN_OPTS="-Xmx4096m -Xverify:none")
    
Now you can build, e.g. using as above - or also just a single ODL project.

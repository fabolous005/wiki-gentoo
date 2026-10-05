<!-- source: https://wiki.gentoo.org/wiki/News_project/Guide | group: Gentoo Wiki (Main) | wiki-title: News project/Guide -->
---
title: News project/Guide
url: https://wiki.gentoo.org/wiki/News_project/Guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-09-25"
fingerprint: a2e09c850cb3d3ee
license: CC BY-SA 4.0
---

# News project/Guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This guide details the process of creating the **G**entoo **M**onthly **N**ewsletter (GMN).

## Overview

This guide will show you how to write a Gentoo Monthly Newsletter (GMN) from start to finish. It's more or less structured like the newsletter itself.

In 2013 we moved the GMN to a blog using WordPress for ease of work flow and to encourage everyone to participate. Please review some of the
[previous editions](https://www.gentoo.org/news/) to familiarize yourself with coding style; you want the code to be nice and easy to read, both for yourself, and for your teammates.

The GMN relies on volunteers and members of the community for content every month. If you are interested in writing for the GMN or thinking of another way to contribute, please send an e-mail to [gmn@gentoo.org](mailto:gmn@gentoo.org)

### Files

Putting together a newsletter requires several scripts and files. These can be found in our [code repository](http://sources.gentoo.org/viewcvs.py/gentoo/src/gwn/). Be sure to save the following files to your GMN working directory:

- gmn\_bugzie.py
- gmn\_bugzie\_aggregate.py
- get\_portage\_stats.py
- print-dev-stats.py
- pygooglechart.py
- gwn\_adds\_removes.py
- get\_glsas.py

There's also a  [newsletter template](http://dev.gentoo.org/~dabbott/downloads/Newsletter_Template.txt) available. This template will let you get a quick start on each monthly newsletter. Adjust the dates, times, locations, links, and numbers for each new edition. It's pretty straightforward.

## Articles

Feature articles are arguably the heart of the newsletter. However, articles don't usually just fall from the sky: you may have to actively scour the internet looking for articles, news releases, blogs, etc. pertaining to Gentoo. However, rather than do all this work yourself, you can make better use of your time by actively soliciting help from the user community and your fellow developers. Get *them* to send in articles and links of interest. The more they write up ahead of time, the better, as you'll need to do less editing. When someone sends interesting material your way, be sure to give them an author or contributor credit in the GMN. Participation should be fun *and* rewarding.

### Gentoo news

Lead off the GMN (after the usual introduction) with general distribution news. The GMN is the first place people look for things like Gentoo release announcements, [Council](http://www.gentoo.org//proj/en/council) and [Trustees](https://www.gentoo.org/inside-gentoo/foundation/) meetings, and other important news. Oftentimes critical systemwide changes or security notes may be found here, such as the mask & removal of PHP4, stabilization of the latest Portage version, baselayout changes, and similar. Or mention projects that Gentoo is participating in, such as Google Summer of Code; see the [April 2008](http://www.gentoo.org//news/en/gmn/20080424-newsletter.xml) newsletter for a nice example.

The Council and Trustees post meeting summaries, minutes, and agenda both to the mailing lists and to their individual project pages. Use the summaries (with links to the full documents) here; this way you don't waste time duplicating their efforts. Occasionally there will be an important project meeting for security, desktop, Portage, etc. Make sure to include such meeting notes. Pester the project secretaries/leads for minutes and summaries if you have to. Remember, you don't have much time to play investigative journalist. Try to get the other projects to cooperate with you. Publicity should good for them, after all.

Next: the mini-calendar with the next month or three's events. Upcoming events might include ~~monthly bugdays~~, Gentoo IRC meetings, and Linux/FOSS trade shows.

### Tips and tricks

This chapter contains useful information for administering your system, keeping things up-to-date, tweaking applications, monitoring boxes, and so on. If you're lucky, you can get users and other developers to email some collected tips or "best practices" each month. You may want to cull the forums for useful material.

## Statistics collection

The first half of the newsletter features articles, and those vary quite a bit month-to-month. The second half contains general statistics on Bugzilla, the Portage tree, developer changes, and so on. It's very formulaic, straightforward, but also can be the most time consuming part of the newsletter to write. This is where you'll use all the scripts we have to round out the GMN.

The statistical chapters are assembled as follows:

### Bugzilla statistics

To generate the nice table of [Bugzilla](http://bugs.gentoo.org) statistics (new bugs, closed bugs, distribution, etc.), do the following:

 **Generating Bugzilla statistics** 

`user $````
touch breport
```
`user $````
python2 gmn_bugzie.py > breport
```
`user $````
python2 gmn_bugzie_aggregate.py 
```
The first script, gmn\_bugzie.py, generates a statistics file called breport after querying Bugzilla. This script also generates the text and tables to include in the blog.

The second script, gmn\_bugzie\_aggregate.py, creates and generates three graphs: activity.png, opened.png, and closed.png. Be sure to edit the script to point to the file you generated with the first script, gmn\_bugzie.py. These three files come courtesy [Google Chart](https://code.google.com/apis/chart/) (via pygooglechart.py). Save the graphs to your working directory; you'll be adding them to the blog later.

### Portage statistics

To create the Portage statistics (number of packages by keyword, distribution, etc.), you'll need an unmodified Portage installation. This means you shouldn't be using `PORTDIR_OVERLAY` or anything else that affects the number of categories and packages reported. Also, you'll have to use the downloaded metadata cache obtained from syncing, so you can't set various fun things like `portdbapi.auxdbmodule = cache.metadata_overlay.database` in /etc/portage/modules.

First, you'll need to emerge portage-utils, and run one of its utilities as shown.

 **Obtaining Portage statistics** 

`user $``python2 get_portage_stats.py`
This script creates the table suitable for direct inclusion in the GMN post and a URL to a Google chart. Download the chart and save it to your working directory. The output from the results of the script will tell you what to save it as. You'll be uploading this file with the rest of the charts soon. Again, make sure to add the proper link to this file within the blog.

The package adds/removes are generated by saving the logs emailed by infrastructure, and running gwn\_adds\_removes.py on them:

 **Generating package adds/removes** 

`user $``python2 gwn_adds_removes.py add-removals.1220227200.log > foo.txt`
In this example, the output of the command is saved to a file, rather than printed straight to the terminal. gwn\_adds\_removes.py all you have to do is paste it into the newsletter.

### Developer statistics

You can get the developer statistics (total recruited, away, etc.) by first downloading the devaway XML file, and then by hand-editing the print-dev-stats.py script to point at your own CVS checkout directory.

 **Obtaining developer statistics** 

`user $``python2 print-dev-stats.py`
Once you've done this, paste the numbers into the developers summary section (number recruited, active, and away).

For developer changes (joining/leaving projects or teams), run a diff of the changes to herds.xml since the last issue. This information is available [in CVS](http://sources.gentoo.org/viewcvs.py/gentoo/xml/htdocs/proj/en/metastructure/herds/herds.xml) . It's also advisable to check the project pages as well. For projects that have been moved to [Gentoo Wiki](https://wiki.gentoo.org/wiki/Project:Gentoo) , use [this](https://wiki.gentoo.org/index.php?title=Special:RecentChanges&limit=500&days=30&namespace=510&tagfilter=new_project_member) link to get the list of people who joined a project, and [this](https://wiki.gentoo.org/index.php?title=Special:RecentChanges&limit=500&days=30&namespace=510&tagfilter=removed_project_member) link for people who left one.

You should also keep a close eye on new developer emails sent to the gentoo-dev-announce and/or gentoo-dev mailing lists. You should receive automated retirement notices generated by Infra sent directly to the GMN email alias.

### Security stats

 **Obtaining security statistics** 

`user $``python2 get_glsas.py`
Again once done paste results into security section.

## Finishing up

As a courtesy to your fellow Gentoo developers, solicit feedback on the latest issue by writing to the gentoo-core mailing list at least one day in advance of publication, making sure to cc [gmn@gentoo.org](mailto:gmn@gentoo.org) to include anyone not a Gentoo developer. Be sure to provide them with a working URL to the issue.

### Final touches

Once you've applied the final fixes from gentoo-core, you'll need to add a few more bits to the issue *immediately* before committing. Time is important here, as the webnodes take awhile to update.

### Blog publication

During the creation of the blog post we will set up the publish date to the last day of the month and as we edit the page, update it as we go. Once the date for publication has been set it will publish automatically on that date. Make sure to change the category to news so that [Planet Gentoo](http://planet.gentoo.org/) picks it up.

Write up the front page announcement for www.gentoo.org and add it to gentoo/xml/htdocs/news/. See the [Feb 2014 news item](http://www.gentoo.org//news/20140301-gmn.xml) for how it's done. Add it to CVS (cvs add).

Make sure to update the [index page](http://www.gentoo.org/news/en/gmn/) as well.

There...the front page announcements is in place, the newsletter is online, now you can take a break! At least until the date for the next issue approaches, And it all starts over again...

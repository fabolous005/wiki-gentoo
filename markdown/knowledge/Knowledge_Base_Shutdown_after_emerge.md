<!-- source: https://wiki.gentoo.org/wiki/Knowledge_Base:Shutdown_after_emerge | group: Gentoo Knowledge | wiki-title: Knowledge_Base:Shutdown_after_emerge -->
---
title: Knowledge Base:Shutdown after emerge
url: https://wiki.gentoo.org/wiki/Knowledge_Base:Shutdown_after_emerge
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-11-10"
fingerprint: "379457d31985dfeb"
license: CC BY-SA 4.0
---

# Knowledge Base:Shutdown after emerge

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Emerge is not running

This command shuts down the computer only if emerge finishes successfully, it also saves output to a emerge-log.log file in the current directory that you can reviewed next time the computer boots. For example for updating and shutting down we have:

`root #``emerge --verbose --deep --newuse --update --with-bdeps=y @world | tee emerge-log.log && shutdown -h now`
If you want to turn off the computer only if emerge quits successfully:

`root #``emerge --verbose --deep --newuse --update --with-bdeps=y @world | tee emerge-log.log ; shutdown -h now`
### elogind

If you are using [elogind](https://wiki.gentoo.org/wiki/Elogind), you can use the command [loginctl](https://wiki.gentoo.org/wiki/Elogind#loginctl) to shut down your computer in a preferred manner once the emerge is complete.

`root #``emerge PACKAGE && loginctl COMMAND`
## Emerge is already running

If emerge is already running, press `ctrl`-`z`. The process will be paused

\[1\]+ Stopped emerge kdebase-meta

Now type

`root #``bg && wait && poweroff`
bg resumes executing of emerge in the background. wait waits for last command sent to background to terminate. When emerge finishes with success, poweroff command will be executed.

### Alternatives

This can be accomplish in a similar way by pausing the process with `ctrl`-`z` as described above and then typing this:

`root #``fg; poweroff`
or

`root #``fg && poweroff`
This continues the paused process and after this process finishes it continues the next command (poweroff) ...

It is important to notice that *&&* will only continue with the next command, if the first command completed successfully. *&&* is the logical AND operator of the shell. By using *;* the next command will be executed no matter what happened earlier. *;* is only a command separator.

A variant of this can be used to play a sound when emerge is done.

`root #``fg; aplay whateversoundfile`
The fg variant is a little more straight forward probably.

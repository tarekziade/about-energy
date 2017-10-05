about:energy
============

On laptops, power management is a key feature to prolonge battery life
as much as possible.

Monitoring power consumption can be done at the system level and each operating
system has a flurry of tools to do it. The metrics you can use will produce
very low-level metrics, like the Running Average Power Limit (RAPL)
(https://01.org/rapl-power-meter) or higher level ones, like tracking
CPU/GPU/Network/Disk usage at the process level.

For instance, Powertop (xxx) provides metrics similar to top, but
focuses on what consumes energy. An application that uses a lot of RAM
but idles will not impact energy by much, as opposed to one that uses
100% of the CPU and will eventually drain your battery.
Tools like Powertop will point them as the cause of a sudden battery life
drops.

In a perfect world, the newest Firefox uses one process per tab and all
other tabs are happily idling in the background. The only one that is draining
the battery is the active one. Background tabs might wake up from time to time,
but will gently go back to sleep.

But that's far from reality. Modern web pages are dynamic and will use
features like web sockets, background workers, notifications etc.
They might suck your resources even if they are not the active tab.

A webmail client like gmail is a typical example. Even if the tab is
in the background, it will stay active. If you activate Google Hangout,
you are going to constantly interact with servers.

In Firefox, assuming es10 is the default, we can track a few metrics
per tabs. As a matter of fact, "about:performance" is doing
that in its "Performance of Web Pages" section. This section will
display the CPU usage and impact on framerate (XXX see how its computed),
among other metrics.

But that page is not focusing on power metrics. For instance
it's not checking the network usage. (xxx verify this)

In that case, would it make sense to extend this feature and have a specialized
"about:energy" page in Firefox, like what Powertop has at the OS level?

Would this help tracking web pages that are responsible for reducing
battery life?

This document investigates whether it would make sense to add an
"about:energy" page in Firefox to specifically track power used by
tabs and how it could look like.


What to measure?
================

Measuring power at the process level is quite hard, and there's no precise
correlation between what you can measure and the power consumption of a process
-- and indirectly, how much it affects battery usage.

Finding the cause is also hard, there's no silver bullet. If a web page
starts to eat of lot of resources, more digging is required.

However, tracking a few metrics can give us some hints on which web pages
are responsible for energy consumption, this is very interesting
in particular if they are active in the background.

A few useful per-process (per-tab) measures:

- CPU and GPU usage
- Wake-up counts / Low C-state residency
- Network activity
- Disk activity
- Tab in the foreground or background

If a tab has high values in some of those numbers, it's lkely that
it's causing more energy consumption than other processes.

Defining a threshold? XXX

Finding the cause:

- Animation inspector
- Profilers (gecko etc)
- dtrace, perf, TimerFirings logging


good reads:

- https://developer.mozilla.org/en-US/docs/Mozilla/Performance/Power_profiling_overview
- https://developer.mozilla.org/en-US/docs/Mozilla/Performance


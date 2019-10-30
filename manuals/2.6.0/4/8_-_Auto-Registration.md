---
layout: manpage
title: Automatic Registration
meta: 2.6.0
nav: 8 - Auto Registration
navversion: nav26
---

<p>You have a lot of machines coming right "off the truck" and you
want to plug them in, install them however (perhaps using PXE
menus, koan with --profile, a live CD, etc) and then have the
system mac address recorded in cobbler so you can change things
later.</p>

<p>In the simplest of use cases, you could set up a cobbler default
profile that did nothing more than serve up a mostly blank
kickstart, set up a post install trigger to email your admin, and
then all turned on machines would register automagically, and then
you could reassign them to other profiles using the cobbler command
line tools. (See <a href="Triggers">Triggers</a>)</p>

<p>There are then quite a few ways in which you might want to use
this.</p>

<h2>Warning</h2>

<p>Though this feature cannot overwrite system records, it does have
the ability to create a /LOT/ of cobbler system records remotely if
someone wanted to write a script to do it. It should not be enabled
on a public network, so use your judgement. In most cases it's
perfectly safe.</p>

<p>For instance, this was originally written for FreeLinuxPC.org,
which had a large amount of machines on a private network that was
for installation purposes only. On a restricted installation-only
subnet this should be fine.</p>

<h2>How It Works</h2>

<p>A tool called "cobbler-register" is installed as part of the koan
package.</p>

<p>There is also a
<a href="Kickstart%20Snippets">Kickstart Snippets</a> that is in
every default kickstart that says "if this feature is enabled in
the settings file, and this is a profile based install, register
this system automatically in %post to create a system record for
this system if it did not already exist.</p>

<p>If you want to run cobbler-register over SSH instead that works
too.</p>

<p>cobbler-register is smart and will set all fields to "netboot
disabled" to prevent newly registered systems from being
reinstalled unless you ask them to be.</p>

<pre><code>cobbler-register [---server=cobbler.example.org] --profile=name [--hostname=override-hostname.example.org]
</code></pre>

<p>The server will be sourced from the environment variable
COBBLER_SERVER if installed previously with cobbler, meaning you
won't need to provide it in many cases. The hostname can also be
used if available.</p>

<p>This registration script will populate all physical interfaces
found, including IP, MAC, and netmask info. The user may wish to
fill in additional information later by a script or using
web interface or the
command line -- not everything can be autodetected.</p>
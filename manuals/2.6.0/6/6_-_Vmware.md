---
layout: manpage
title: VMWare support for koan
meta: 2.6.0
nav: 6 - VMWare
navversion: nav26
---

<p>One goal of Cobbler is to abstract out all your installation types.  Basically the idea is create a profile one, use it for PXE, reinstallations (--replace-self) and virtualized machines all at once.</p>

<h1>Installing</h1>

<p>To get this working, you will need to be using Cobbler to host your PXE infrastructure.</p>

<p>You can then, using special koan syntax (one single command line invocation), install a Cobbler profile /inside/ VMware, pulling the RAM and Disk storage parameters from Cobbler.</p>

<p>This allows you to treat VMware just as another installation type ... the same kickstart and parameters you use to install inside your bare metal machines and other virtualization types can be used for vmware in very similar ways.</p>

<h1>Setup</h1>

<p>First, grab yourself some caffeine.</p>

<p>Second, set up VMware as normal.  (As of the time of writing this, we support VMware server and workstation, and there is a patch being worked on to support nearly all VMware variants)</p>

<ul>
<li>Install the latest vmware-server.  At the time of testing, I managed to get VMWare to work on EL 5.</li>
<li>Install the latest any-any patch to make vmware actually work on said platform</li>
<li>run /usr/bin/vmware-config.pl as appropriate</li>
<li>enter your free serial number when requested</li>
<li>make sure the vmware service is enabled and started</li>
</ul>


<p>Then do some minimal cobbler setup:</p>

<ul>
<li>add cobbler profiles and cobbler system records to taste</li>
<li>cobbler must be set up to be your PXE infrastructure, which it should be doing already hopefully</li>
<li>"cobbler system add" a new system and specify a mac address in the range of 00:50:56:00:00:00 to 00:50:56:3F:FF:FF (VMWare's allowed range).</li>
</ul>


<p>Note that MAC addresses outside that range will not work, and you cannot skip the step of creating them in that range.  Unlike Xen/KVM they are also
not auto-generated by koan... the reason for this is, without the mapping in Cobbler, PXE control over what profile is installed is meaningless.</p>

<p>Then, on the guest OS, run the following:</p>

<pre><code>koan --server=cobbler.example.org --system=foosball --virt --virt-type=vmware 
</code></pre>

<h1>What Happens</h1>

<p>Koan will ask cobbler for the RAM and Disk needs associated with "foosball", or rather, the profile associated with "foosball".  It will then automatically create your VMware configuration file (vmx) and disk image, and set that image up so that the rest of the installation happens over PXE.</p>

<p>Thus the installation is in tandem between PXE and the VMware libraries, though all of the profile data is still managed by cobbler.</p>
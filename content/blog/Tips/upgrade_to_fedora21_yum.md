Title: Upgrade to Fedora 21, the dnf way
Date: 2015-01-05 22:00
Authors: Georg Vogetseder

My fleet of Fedoras is always updated using yum directly. I presume that
`fedup` works well for most use cases, but I think using the package manager directly,
as with pacman or apt-get is the golden way to upgrading. The last really
hard Fedora upgrade this way was the Fedora 17 Upgrade, where `/bin` and
`/usr/bin` got merged. It was possible, but there was a lot of ugliness
involved when using 3rd-party RPMs. The Fedora 21 upgrade contains nothing nasty in
this department.

[The official Wiki page](https://fedoraproject.org/wiki/Upgrading_Fedora_using_yum#Fedora_20_-.3E_Fedora_21)
contains no warnings, just the usual RPM-Key import and a notice on choosing a
flavor. A `touch /.autorelabel` should fix SELinux Problems.

To spice things up, I thought to use
[`dnf`](https://github.com/rpm-software-management/dnf) - the new package
manager in Fedora for the upgrade. It utilizes `libsolv` from openSUSE for
dependency resolution and should work better than yum in this respect.

And it totally worked for me. Without a single problem, just by replacing the yum commands from
the wiki with dnf. Cool stuff.

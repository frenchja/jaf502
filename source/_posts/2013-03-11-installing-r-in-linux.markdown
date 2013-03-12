---
layout: post
title: "Installing R in Linux"
date: 2013-03-11 17:18
comments: true
sharing: true
categories: [Linux, R, PPC]
---

This guide is intended to faciliate the installation of up-to-date R packages
for users new to either R or Linux.  Unlike Windows binaries or Mac packages,
Linux software is often distributed as source-code and then compiled by package
maintainers.  The use of package managers has many advantages that I won't 
discuss here (see [Wikipedia](https://en.wikipedia.org/wiki/Package_management_system)).  
More importantly, the difference can be initially intimidating.  
However, once the user gets used to using package managers such as
[apt](https://en.wikipedia.org/wiki/Advanced_Packaging_Tool) or 
[yum](https://en.wikipedia.org/wiki/Yellowdog_Updater,_Modified) to install software, 
I'm confident they'll appreciate their ease of use.

These instructions are organized by system type.

Debian-based Distributions
--------------------------
### Ubuntu

Full installation instructions for Ubuntu can be found
[here](http://cran.rstudio.com/bin/linux/ubuntu/).  Luckily, CRAN mirrors have
compiled binaries of R which can be installed using the apt-get package manager.
To accomplish this, we'll first add the CRAN repo for Ubuntu packages to
`/etc/apt/sources.list`.  If you prefer to manually edit the sources.list file,
you can do so by issuing the following in the terminal:

``` bash Inspecting sources.list
sudo nano /etc/apt/sources.list
```

``` bash Installing R in Ubuntu
# Grabs your version of Ubuntu as a BASH variable
CODENAME=`grep CODENAME /etc/lsb-release | cut -c 18-`

# Appends the CRAN repository to your sources.list file 
sudo sh -c 'echo "deb http://cran.rstudio.com/bin/linux/ubuntu $CODENAME" >> /etc/apt/sources.list'

# Adds the CRAN GPG key, which is used to sign the R packages for security.
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E084DAB9

sudo apt-get update
sudo apt-get install r-base r-dev
```

<!-- more -->

### Debian

The instructions for installing R in Debian are similar to Ubuntu.  As the CRAN
[README file](http://cran.rstudio.com/bin/linux/debian/README.html) for Debian
points out: 

{% blockquote CRAN http://cran.us.r-project.org/bin/linux/debian/README.html README %}
After a release of Debian "stable", no new packages get added by
Debian to keep the release as 'stable' as possible. This implies that the R
release contained in the official Debian release will become outdated as time
passes.
{% endblockquote %}

Thus, we'll append the CRAN repository to the Debian list, just like
we did for Ubuntu:

``` bash Installing R in Debian Stable
# Appends the CRAN repository to your sources.list file 
sudo sh -c 'echo "deb http://cran.rstudio.com/bin/linux/debian lenny-cran/" >> /etc/apt/sources.list'

# Adds the CRAN GPG key, which is used to sign the R packages for security.
sudo apt-key adv --keyserver subkeys.pgp.net --recv-key 381BA480
sudo apt-get update
sudo apt-get install r-base r-base-dev
```

Next, you can search for additional R packages in terminal using `apt-cache`:
``` bash Searching for R packages using apt-get
apt-cache search ^r-.*
```

### Installing RStudio 
The RStudio package is compiled for both Debian and
Ubuntu distributions.  Therefore, the installation instructions are the same.  Copy the link for the latest RStudio package from http://www.rstudio.com/ide/download/desktop (e.g., [http://download1.rstudio.org/rstudio-0.97.320-amd64.deb](http://download1.rstudio.org/rstudio-0.97.320-amd64.deb)).

``` bash
sudo apt-get install http://download1.rstudio.org/rstudio-0.97.320-amd64.deb
```

RedHat-based Distributions
--------------------------
### RedHat EL6 (or CentOS 6+)

In order to get R running on RHEL 6, we'll need to add an additional repository
that allows us to install the new packages, EPEL.  [Extra Packages for
Enterprise Linux](https://fedoraproject.org/wiki/EPEL) (or EPEL) is a Fedora
Special Interest Group that creates, maintains, and manages a high quality set
of additional packages for Enterprise Linux, including, but not limited to, Red
Hat Enterprise Linux (RHEL), CentOS and Scientific Linux (SL).

``` bash Installing EPEL
# For El5 or CentOS 5
su -c 'rpm -Uvh http://download.fedoraproject.org/pub/epel/5/i386/epel-release-5-4.noarch.rpm'
sudo yum update
sudo yum install R

# For El6 or CentOS 6
su -c 'rpm -Uvh http://download.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm'
sudo yum update
sudo yum install R
```

Next, you can search for additional R packages in terminal using `yum`:
``` bash Searching for R packages using yum
yum list R-\*
```

### Fedora

For Fedora, life is much easier.  Current versions of Fedora have an up-to-date build of R in their repositories.

``` bash Installing R in Fedora
sudo yum update
sudo yum install R
```

### Installing RStudio
You'll want to copy the appropriate link for your system from the [RStudio Desktop Download](http://www.rstudio.com/ide/download/desktop) page.  In my case, it was `http://download1.rstudio.org/rstudio-0.97.320-x86_64.rpm`.

``` bash Installing RStudio on Fedora/RHEL/CentOS
sudo yum install http://download1.rstudio.org/rstudio-0.97.320-x86_64.rpm
```

Dealing with PPC-based Systems
==============================

As our lab has a lot of old iMacs that are still quite useful, I've recently
dealt with the best way to support R on these machines.  These machines are
still quite useful, particulary when their maximum potential RAM is installed.
However, Apple stopped supporting the machines after OS 10.5.8 ([see
here](https://en.wikipedia.org/wiki/Apple%27s_transition_to_Intel_processors)).
While the [nightly packages](http://r.research.att.com/) distributed do install
on PPC machines with OS X, they lack the R Framework compiled for PPC, meaning
that it's a useless installation.  This means that a user stuck on OS X 10.5.8
is tied to R 2.10, a very old R distribution that's incompatible with many
existing packages.

Any solutions?
--------------

My first solution was to compile R from source using
[MacPorts](https://www.macports.org/), a package manager similar in concept to
`yum` or `apt-get`.  While successful, it takes a *long*, *long*, time to build
R and its necessary dependencies on a 1.8 Ghz G5 processor.  From a system
administrator's perspective, this also is the least parsimonious solution
possible, since each machine has to be updated with each new release of R.

Thanks to [RStudio
Server](http://www.rstudio.com/ide/docs/server/getting_started), each machine
doesn't need to have R installed, as it can be run off a more powerful server
and accessed using a reasonably up-to-date browser.  I was able to install R and
RStudio on our RedHat EL6 server easily.  The trick was to make this as seemless
as possible from the user's perspective.  To accomplish this, I saved the
Bookmark to the Desktop.

{% img right http://gradstudents.wcas.northwestern.edu/~jaf502/images/ppc-r-bookmark.png R Bookmark %}

Next, I downloaded a large R icon using Google Images and edited the Bookmark's
icon to appear as if it were R.  To do this, just copy the R icon from within
Preview, select the icon of the Bookmark by right-clicking and selecting Get
Info, and pasting using `Command+V`.


{% img left http://gradstudents.wcas.northwestern.edu/~jaf502/images/ppc-r-preview.png 300 R Preview %}

{% img left http://gradstudents.wcas.northwestern.edu/~jaf502/images/ppc-r-getinfo.png Get Info %}

Finally, this was dragged to the OS X dock, appearing just as if it were R on the local machine, but without all the hassle and slow load times on PPC.

{% img http://gradstudents.wcas.northwestern.edu/~jaf502/images/ppc-r-dock.png Fake Dock Icon %}

It is worth noting that this solution is only important for users tied to OS X
10.5.8.  I've had great success with using [Fedora's PPC
build](https://fedoraproject.org/wiki/Architectures/PowerPC?rd=Arch:PPC), which
has packages compiled for PPC already.  However, as Linux is intimidating for
many users, I chose to install RStudio Server on our lab server.

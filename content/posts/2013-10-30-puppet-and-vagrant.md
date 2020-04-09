---
layout: post
title: Puppet and vagrant
date: 2013-10-30 23:13:00
comments: true
Tags:
- puppet
- vagrant
- devops
Category: tech
---

So I've been playing with [Puppet](https://puppetlabs.com/) and [Chef](https://www.opscode.com/chef/) for server provisioning. The two technologies attempt to fill the same, or at least similar tasks: managing system configuration. With [Vagrant](https://www.vagrantup.com/), testing these two systems has become extremely easy. Vagrant supports both methods for server provisioning and the turnaround time for the tests is extremely short.

I have tried for quite a while to run chef on a system, to no avail. I've often found that an unexpected crash puts end to my testing, whereas the first times I tried puppet I achieved success. I also feel the declarative style with which the puppet modules are built really suits the problems that puppet solves.

This post is less of a comparison of Chef vs Puppet, perhaps the first in a series comparing the two, but for now we start with Puppet.

## Simple configuration

Tools used:

* vagrant
* puppet
* librarian-puppet

I'm not going to go into details about the installation of the tools, apart from to say that vagrant is installed from the [vagrant website](https://vagrantup.com) and puppet/librarian-puppet are installed as gems.

A project can be completetly defined (for example for version control) in four files:

* Vagrantfile
* Puppetfile
* Gemfile
* manifests/default.pp (by convention)

The directory layout for the project, and what should be checked into version control is

``` bash
.
├── Gemfile
├── Puppetfile
├── Vagrantfile
└── manifests
    └── default.pp
```

The `Gemfile` stores the ruby gem versions, the `Puppetfile` sets the puppet modules, the `Vagrantfile` configures the virtual machine and the `manifests/default.pp` file defines the state of the virtual machine.

### Gemfile

A simple portable gemfile stores the required gems (puppet and librarian-puppet) and gives the option to version them. An example gemfile is


``` ruby
source "https://rubygems.org"

gem 'librarian-puppet'
gem 'puppet'
```


### Vagrantfile

This is where the vagrant virtual machine configuration is stored. The operating system and provisioning settings are stored here. A minimal example is given below, and sets up the box to use (precise64) and configures the puppet provisioning.


``` ruby
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "precise64"

  config.vm.provision :puppet do |puppet|
    puppet.manifests_path = "manifests"
    puppet.manifest_file = "default.pp"
    puppet.module_path = "modules"
  end
end
```

### librarian-puppet

A major part of development is the DRY principle, and applies to package configuration also. `librarian-puppet` allows pre-build and tested modules to install common packages. Puppetlabs maintain a repository of modules at [puppet forge](https://forge.puppetlabs.com/), or on [github](https://www.github.com) and these can be configured and managed through librarian-puppet. An example configuration is given below:

``` ruby
forge "https://forge.puppetlabs.com"

mod 'puppetlabs/stdlib'
mod 'puppetlabs/postgresql'
mod 'puppetlabs/apt'
```
where the `mod` lines define the required modules with the author prefix, and the forge line defines the source. The syntax is remarkably similar to the Gemfile.


### manifests/default.pp

This file describes the final configuration. I recommend following the [official puppet tutorial](https://docs.puppetlabs.com/learning/index.html), an example file which installs vim can be:

``` puppet
exec { "apt-get update":
  path => "/usr/bin",
}

package { "vim":
  require => Exec["apt-get update"],
  ensure => present,
}
```

though this does not use the modules. These can be defined and configured by reopening the class declaration, e.g.

``` ruby
# Puppetfile
mod 'puppetlabs/postgresql'

```
``` puppet
# manifests/default.pp
exec { "apt-get update":
    path => "/usr/bin",
}

package { "vim":
    ensure => present,
    require => Exec["apt-get update"],
}

class { "postgresql::client":
    require => Exec["apt-get update"],
}
```

This configuration will install the `vim` and `postgresql::client` packages ensuring that `apt-get update` has been run before.


## Starting a project from scratch

These files can be initialised through three commands:

* `bundle init`,
* `vagrant init`,
* `librarian-puppet init`, 

which are then configured to the users needs. Commands to run to create the full working system are

``` bash
bundle install
bundle exec librarian-puppet install
vagrant up
```

Simple eh? More documentation for these files can be found:

* [https://www.vagrantup.com](https://www.vagrantup.com)
* [https://puppetlabs.com/](https://puppetlabs.com/)
* [The excellent puppet tutorial](https://docs.puppetlabs.com/learning/index.html)
* [librarian-puppet](https://librarian-puppet.com/)

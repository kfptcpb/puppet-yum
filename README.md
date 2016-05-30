# Puppet yum module

## Please note this plugin is deprecated, future maintenance took over the Puppet Community. Please use new https://forge.puppet.com/puppet/yum

This module provides helpful definitions for dealing with *yum*.

### Requirements

Module has been tested on:

* Puppet 3.7.x
* CentOS 6, 7

# Usage

### yum

Manage main Yum configuration.

```puppet
class { 'yum':
  keepcache         => false|true,
  debuglevel        => number,
  exactarch         => false|true,
  obsoletes         => false|true,
  gpgcheck          => false|true,
  installonly_limit => number,
  keep_kernel_devel => false|true,
}
```

If `installonly_limit` is changed, purging of old kernel packages is triggered.

### yum::config

Manage yum.conf.

```puppet
yum::config { 'installonly_limit':
  ensure => 2,
}

yum::config { 'debuglevel':
  ensure => absent,
}
```

### yum::gpgkey

Import/remove GPG RPM signing key.

Key defined in recipe (inline):

```puppet
yum::gpgkey { '/etc/pki/rpm-gpg/RPM-GPG-KEY-puppet-smoketest1':
  ensure  => present,
  content => '-----BEGIN PGP PUBLIC KEY BLOCK-----
...
-----END PGP PUBLIC KEY BLOCK-----',
}
```

Key stored on Puppet fileserver:

```puppet
yum::gpgkey { '/etc/pki/rpm-gpg/RPM-GPG-KEY-elrepo.org':
  ensure => present,
  source => 'puppet:///modules/elrepo/RPM-GPG-KEY-elrepo.org',
}
```

### yum::plugin

Install or remove *yum* plugin:

```puppet
yum::plugin { 'versionlock':
  ensure => present,
}
```

### yum::versionlock

Locks explicitly specified packages from updates. Package name must
be precisely specified in format *`EPOCH:NAME-VERSION-RELEASE.ARCH`*.
Wild card in package name is allowed or automatically appended,
but be careful and always first check on target machine if your
package is matched correctly! Following definitions create same
configuration lines:

```puppet
yum::versionlock { '0:bash-4.1.2-9.el6_2.*':
  ensure => present,
}

yum::versionlock { '0:bash-4.1.2-9.el6_2.':
  ensure => present,
}
```

Correct name for installed package can be easily get by running e.g.:

```bash
$ rpm -q bash --qf '%|EPOCH?{%{EPOCH}}:{0}|:%{NAME}-%{VERSION}-%{RELEASE}.%{ARCH}\n'
0:bash-4.2.45-5.el7_0.4.x86_64
```

### yum::group

Install or remove *yum* package group:

```puppet
yum::group { 'X Window System':
  ensure  => present,
  timeout => 600,
}
```

### yum::localinstall

Install or remove packages via *yum* localinstall subcommand:

```puppet
# From URL
yum::localinstall { 'package-name':
  ensure => present,
  rpm_source => 'http://path/to/package/filename.rpm',
}
```

```puppet
# From Local Filesystem
yum::localinstall { 'package-name':
  ensure => present,
  rpm_source => '/path/to/package/filename.rpm',
}
```

Please note that the title must be the package name as it is referenced in the
package manager after it is installed.

# Contributors

* Eugene Dementiev <eugene@dementiev.eu>
* Mark McKinstry <mmckinst@nexcess.net>
* Trey Dockendorf <treydock@gmail.com>
* Derek McEachern <a0216331@ti.com>

***

CERIT Scientific Cloud, <support@cerit-sc.cz>

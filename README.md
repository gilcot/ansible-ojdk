# Open Java Developmet Kit

This role is to manage Open Java Development Kit (OpenJDK) on a Linux box.

[OpenJDK](https://openjdk.java.net) is a free and open-source
[reference implementation](https://web.archive.org/web/201603010102422/https://blogs.oracle.com/henrik/entry/moving_to_openjdk_as_the)
of the Java Platform, Standard Edition (J2SE), as defined by the
[Java Community Process (JCP)](https://jcp.org/en/home/index).
It's under 
[GNU General Public Licence version 2 with linking exception](http://openjdk.java.net/legal/gplv2+ce.html)
and includes: [HotSpot](https://en.wikipedia.org/wiki/HotSpot) virtual machine
([JVM](https://en.wikipedia.org/wiki/Listo_of_Java_virtual_machines)), and
`javac` (official Sun [compiler](https://en.wikipedia.org/wiki/Java_compiler),
not: <abbr title="Eclipse Compiler for Java">ECJ</abbr>,
<abbr title="GNU Compiler for Java">GCJ</abbr>,
<abbr title="IBM open-source compiler">Jikes</abbr>, etc.)

As it's a reference, it provides expected common denominator (other
implementations may add classes or not only J2SE...) So, they are some minor
[differences with Oracle JDK](https://javapapers.com/java/oracle-jdk-vs-openjdk-and-java-jdk-development-process/)
and also in the
[releas model](https://www.oracle.com/java/java2-screencasts.html?bcid=5582439790001&playerType=single-social&size=events)
and [paid support](https://blogs.oracle.com/java-platform-group/oracle-jdk-release-for-java-11-and-later).

  - [Starting](#starting)
    - [Requirements](#requirements)
    - [Installing](#installing)
  - [Using](#using)
    - [Variables](#variables)
    - [Examples](#examples)
  - [Misc](#misc)
    - [Licence](#licence)
    - [Authors](#authors)
    - [Contributions](#contributions)

## Starting

### Requirements

This role depends on no other role.

This role uses the distribution native package manager and configured
repositories. 

This role works for distributions there's a build for (see, for example,
https://wiki.openjdk.java.net/display/Build/Supported+Build+Platforms for
a nearly complete and up to date list.) Also note that available
[versions](https://en.wikipedia.org/wiki/OpenJDK#OpenJDK_versions)
may vary according to your distribution major release, and numbering differs
too.

|      v6 |      v7 |      v8 |      v9 |  v10 |  v11 |  v12 | distribution release |
| ------- | ------- | ------- | ------- | ---- | ---- | ---- | -------------------- |
|         | `7`     | `8`     |         |      |      |      | Alpine 3.3 / 3.4 / 3.5 / 3.6 / 3.7 / 3.8 /3.9 |
|         | `7`     | `8`     | `9`     |      |      |      | Alpine edge |
| 1.`6`.0 | 1.`7`.0 | 1.`8`.0 |         |      |      |      | ALT Linux Sisyphus |
|         | `7`     | `8`     |         | `10` | ` `  |      | Arch Linux |
| 1.`6`.0 | 1.`7`.0 | 1.`8`.0 |         |      |      |      | CentOS 6 |
| 1.`6`.0 | 1.`7`.0 | 1.`8`.0 |         |      | `11` |      | CentOS 7 |
| `6`     | `7`     |         |         |      |      |      | Debian 7 (Wheezy) |
|         | `7`     |         |         |      |      |      | Debian 8 (Jessie) |
|         |         | `8`     |         |      |      |      | Debian 9 (Stretch) |
|         |         | `8`     |         |      | `11` |      | Debian 10 (Buster) |
|         |         | `8`     |         |      | `11` | `12` | Debian Sid |
|         |         | 1.`8`.0 | `9`     |      | `11` | ...  | Fedora 27 / 28 |
|         |         | 1.`8`.0 |         | ...  | `11` | ...  | Fedora 29 / Rawhide |
| (port) | ` ` (+port) | `8` (+port) |  |      |      |      | FreeBSD 10 |
| (port) | ` ` (+port) | `8` (+port) |  |      | `11` |      | FreeBSD 11 / 12 |
|         | 1.`7`.0 |         |         |      |      |      | Mageia 4.1 |
|         |         | 1.`8`.0 |         |      |      |      | Mageia 5.1 / 6.1 / Caudron |
|         | `7`     | `8`     |         |      |      |      | NetBSD 7.0 / 7.1 / 7.2 |
|         |         | `8`     |         |      |      |      | NetBSD 8.0 |
|         | 1.`7`.0 | 1.`8`.0 | `9`     | `10` | `11` | `12` | OpenMandriva Cooker |
| 1.`6`.0 | 1.`7`.0 |         |         |      |      |      | OpenMandriva Lx 2013.0 |
| 1.`6`.0 | 1.`7`.0 | 1.`8`.0 |         |      |      |      | OpenMandriva Lx 2014.2 |
|         | 1.`7`.0 | 1.`8`.0 |         |      |      |      | OpenMandriva Lx 3.0 |
|         |       | 1\_`8`\_0 |         | `10` | `11` |      | openSUSE Leap 15.0 |
|     | 1\_`7`\_0 | 1\_`8`\_0 |         |      |      |      | openSUSE Leap 42.3 |
|         |       | 1\_`8`\_0 | `9`     | `10` | `11` |      | openSUSE Tumbleweed |
| 1.`6`.0 |         |         |         |      |      |      | ROSA 2012.1 / Entreprise Desktop |
|         | 1.`7`.0 |         |         |      |      |      | ROSA 2014.1 |
|         |         | 1.`8`.0 |         |      |      |      | ROSA 2016.1 |
|         | (7-)`7` | `8`     |         |      |      |      | Slackware 14.1/14.0/13.37/Current |
| 6-`6`   | (7-)`7` | (8-)`8` | 9-`9`   |      |      |      | Slackware 14.2 |
| `6`     | `7`     |         |         |      |      |      | Ubuntu 14.04 LTS (Trusty Thar) |
|         |         | `8`     | `9`     |      |      |      | Ubuntu 16.04 LTS (Xenial Xerus) |
|         |         | `8`     |         |      | `11` |      | Ubuntu 18.04 LTS (Bionic Beaver) / 18.10 (Cosmic Cuttlefish) |

### Installing

Create or add to your roles dependency file these lines:
  * from _[GitHub](https://github.com/gilcot/ansible-ojdk)_
```yaml
- src: http://github.com/gilcot/ansible-ojdk.git
  scm: git
  version: 1.0.0
  name: openjdk
```
  * or from _[Ansible Galaxy](https://galaxy.ansible.com/gilcot/openjdk/)_
```yaml
- src: gilcot.openjdk
  version: 1.0.0
  name: openjdk
```

Using that file, install the role in your controler host:
```sh
# roles is the roles folder path
# specs is the requirements file created previously
# last option force overriding, usefull to ensure version change
ansible-galaxy install -p roles -r specs -f
```

## Using

### Variables

This role uses very few variables:

#### `ojdk_version`

That's the JDK version to install.   
Beware, it's neither the package version nor the number in the package's
name! See [table above](#requirements)   
This value is mandatory and must be an integer.

#### `ojdk_state`

This is the desired _state_ ; and thus is mandatory. It's either:
  - `present` to install the package if not already done,
  - `absent` to remove the package if still there,
  - all other values accepted by the underlying module (e.g. `lastest` to upgrade the package to the latest fix release.)

#### `ojdk_gpg_uncheck`

It's a boolean (`no`/`false` or `yes`/`true`) use with some
packages managers to disable signatures/certificates check. That may be
usefull to disable such check in some rare cases.


#### `ojdk_repository`

This later is used by few packages managers, to set an additionnal
repository. This string format (URL or a path) and meaning is OS dependant
then.   

| distribution | mandatory | used for |
| ------------ | --------- | -------- |
| AIX          | &#x2713;  | path with AIX packages (required to install) |
| Debian based | &#x2717;  | URL to alternative repository where to found the desired OpenJDK versien |
| FreeBSD      | &#x2717;  | override the default ports source directory (package is build from source as no binary is available by default) |
| HP-UX        | &#x2713;  | the repository from which install or upgrade the package |
| Mac OSX      | &#x2717;  | the absolute path to the package file |
| OpenBSD      | &#x2717;  | override the default ports source directory (package is build from source as no binary is available by default) |
| Red Hat based | &#x2717;  | URL to alternative repository where to found the desired OpenJDK versien |
| Solaris      | &#x2713;  | location to install the package from (must be an acceptable path for `pkgadd` or a valid repository path with definition found in `/etc/opt/csw/pkgutil.conf`) |
| Windows      | &#x2717;  | either the absolute path to the packaged file, or an alternative URL to Chocolatey feed or path to a folder with a `.nupkg` files) |

Note that for some distributions, like CentOS and Ubuntu, it may be useful to
add the repository (either manually or a previous task) before calling this
role. This parameter does very basic setting.

### Examples

Now, you're ready to use it in your playbooks.   
Just be aware that operations should be performed as _root_ user
(that's why escalation privilege is used in the following examples.)

To install (default state) JDK12 on my servlets group:
```yaml
- hosts: servlets
  become: yes
  roles:
    - { role: openjdk, ojdk_version: 12 }
```

To remove JDK5 (example purpose, as this doesn't exist) on dummy host:
```yaml
- hosts: dummy
  become: yes
  roles:
    - { role: openjdk, ojdk_version: 5, ojdk_state: absent }
```
(the same, using the pure YAML syntax)
```yaml
- hosts: dummy
  become: yes
  roles:
    - role: openjdk
      ojdk_version: 5
      ojdk_state: absent
```


OK, now, if your control master's inventory is like:
```ini
[dummies]
dummy

[olders]
foo
bar

[newers]
baz
qix

[servlets:children]
olders
newers

[dummies:vars]
ojdk_version = 5
ojdk_state = absent

[olders:vars]
ojdk_version = 8

[newers:vars]
ojdk_version = 11

[servlets:vars]
ojdk_state = present
```
Then your playbook simply become:
```yaml
- hosts: dummy
  become: yes
  roles:
    - openjdk
```


## Misc

[![Build Status](https://travis-ci.org/gilcot/ansible-ojdk.svg?branch=master)](https://travis-ci.org/gilcot/ansible-ojdk)

### License

This role is copyleft under
[GNU GPLv3](https://www.gnu.org/licenses/quick-guide-gplv3.html)
(in [LICENSE](LICENSE) file)

#### Authors

@github/gilcot is a stanch defender of Free Software and only swears by Ansible system for deployments.

#### Contribution

  - Rate this role on Ansible Galaxy
  - Submit issues to help improving it
  - Request code merge to the code
  - Thank the author(s) with:
    - a [beer](https://beerpay.io/gilcot/ansible-ojdk),
    - a [coffee](buymeacoff.ee/4N0VtGXIj), 
    - a [ko-fi](Ko-fi.com/gilcot),
    - a [penny](https://liberapay.com/gilcot/donate),
    - etc.


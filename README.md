# xephem-deb
Packaging files for xephem program 

This repository contains files needed to create DEB package
from sources of excelent [XEphem](https://github.com/XEphem/XEphem)
program.

There are some [packages](https://build.opensuse.org/package/show/home:rjmathar/xephem) for ubuntu on opensuse build system, but I was
unable to download packaging files from it.

So I decided to write my own and publish them to allow everyone
build XEphem for their debian-based distributuons and their hardware
architecture.

Content of this repository is essentially what dpkg-source would put
into `xephem_<version>.debian.tar.xz` when building source package.

## Changes made

This repository contains patch `debian/patches/Fix-makefiles` which
would be applied to the source of XEphem. Basically it does two things

1. Adds `-D_XOPEN_SOURCE=500` to CFLAGS in `GUI/xephem/Makefile`.
   Without this define gcc on my Debian 13 system is unable to find
   functions `wait3` and `strptime`.
2. Adds toplevel makefile with targets build, install and clean, so
   debhelper would be able to build package without any overrides.

## Usage


How to build debian package from this packaging file

0. Install neccessary package building tools - `build-essential`, `dpkg-dev`
   and `devscripts`.
1. Download latest [release](https://github.com/XEphem/XEphem/releases) of XEphem
2. Rename `<version>.tar.gz` to `xephem_<version>.orig.tar.gz`
3. Unpack archive and rename unpacked directory from `XEphem-<version>`
   to `xephem-<version>`
4. Copy `debian` subdirectory from this repository to unpacked sources
   of XEphem
5. Make sure that latest entry in `debian/changelog` matches your actual
   version. If not, use `dch` program to add new entry.
6. Run `debuild` in  source directory of xephem. If it complains about
   missing build dependencies, install them and rerun 
   (if you use some advanced
   package building tool such as sbuild, it would be done
   automatically).

## TODO

1. It would be nice to use system libz, libpng and libjpeg instead of
bundled ones. But even without this package suites my needs.
2. May be it would be good idea to split package into
   architecture-dependent `xephem` and architecture-independent
   `xephem-data`. 

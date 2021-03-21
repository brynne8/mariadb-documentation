# Why Source RPMs (SRPMs) Aren't Packaged For Some Platforms

MariaDB source RPMs (SRPMs) are not packaged on all platforms for which MariaDB RPMs are packaged.

The reason is that MariaDB's build process relies heavily on <a undefined>cmake</a> for a lot of things. In this specific case, MariaDB's build process relies on [CMake CPack Package Generators](https://gitlab.kitware.com/cmake/community/wikis/doc/cpack/PackageGenerators) to build RPMs. The specific package generator that it uses to build RPMs is called <a undefined>CPackRPM</a>.

Support for source RPMs in <a undefined>CPackRPM</a> became usable with MariaDB's build system starting from around [cmake 3.10](https://cmake.org/cmake/help/v3.10/release/3.10.html). This means that we do not produce source RPMs on platforms where the installed <a undefined>cmake</a> version is older than that.

See also [Building MariaDB from a Source RPM](/mariadb-administration/getting-installing-and-upgrading-mariadb/compiling-mariadb-from-source/building-mariadb-from-a-source-rpm/).
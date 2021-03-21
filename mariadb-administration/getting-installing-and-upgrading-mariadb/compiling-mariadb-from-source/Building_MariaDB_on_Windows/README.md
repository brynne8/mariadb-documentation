# Building MariaDB on Windows

## Build Requirements

To build MariaDB you need the following:

- [Visual C++](http://www.microsoft.com/visualstudio): We currently support Visual Studio 2017 and 2019. Generally we try to support the two most recent VS versions, but build ourselves using the last one. Community editions will work fine; we only use them in our builds.
While installing Visual Studio, make sure to [add "Desktop Development with C++"](https://mariadb.com/kb/en/Building_MariaDB_on_Windows/+image/vs2019_workloads).

- [CMake](https://cmake.org/download): We recommend the latest release. Older releases might not support your version of Visual Studio. Visual Studio 2019 requires cmake 3.14 at least.

- [Git](https://git-scm.com/download): Required to
  build newer versions from the source tree. 
<ul start="1"><li>NOTE:   run 
</li></ul>

```sql
git config --global core.autocrlf input
```

after the installation, otherwise some  mtr tests will fail

In the "Adjusting your PATH" dialog, choose "Use Git from Windows command prompt", otherwise wrong (mingw64) git and perl  will be in your PATH

- [Bison from GnuWin32](http://gnuwin32.sourceforge.net/packages/bison.htm):
  Bison creates parts of the SQL parser. Choose "Complete package except
  sources" when downloading.
<ul start="1"><li>NOTE: <strong> Do not</strong> install this into your default path with spaces
   (e.g. under `C:\Program Files\GnuWin32`); the build will break due to
   [this bison bug](http://sourceforge.net/tracker/index.php?func=detail&amp;aid=2788969&amp;group_id=23617&amp;atid=379173).
   Instead, install into `C:\GnuWin32`.
</li><li>Add `C:\GnuWin32\bin` to your system `PATH` after installation.
</li></ul>
- [Strawberry perl](http://strawberryperl.com): Used to run the test suite.
  [ActiveState Perl](http://www.activestate.com/activeperl/downloads) is
  another Win32 Perl distribution and should work as well (but it is not as
  well tested). NOTE: Cygwin or mingw Perl versions <strong>will not work</strong> for testing. Use Windows native Perl, please.
- Optional: If you intend to build the MSI packages, install
  [Windows Installer XML version 3.9 or higher](http://wix.codeplex.com/releases) . If you build MSI with 10.4, 
  also modify your VS2019 installation, and add "C++ 2019 Redistributable MSMs" (see [MDEV-22555](https://jira.mariadb.org/browse/MDEV-22555))

- [Gnu Diff](http://gnuwin32.sourceforge.net/packages/diffutils.htm), needed if you run mysql-test-run.pl tests.

Verify that bison.exe, bzr.exe or git.exe, cmake.exe and perl.exe can be found in the PATH
environment variable with "`where bison`", "`where git`", "`where perl`" etc. from
the command line prompt.

## Building Windows Binaries

The above instructions assume [MariaDB 5.5](/kb/en/what-is-mariadb-55/) or higher.

Branch the MariaDB bzr repository, or unpack the source archive. On the command
prompt, switch to your source directory, then execute:

```sql
mkdir bld
cd bld
cmake ..
cmake --build . --config RelWithDebInfo
```

The above example builds a release configured for 64 bit systems in a
subdirectory named `bld`. "`cmake ...`"  is the configuration step,
"`cmake --build . --config Relwithdebinfo`" is the build step.

## Build Variations

### Debug Builds

Building Debug version is done with:

```sql
cmake --build . --config Debug
```

### 32bit  and 64 bit Builds

#### Build 64 bit binary

Visual Studio 2019  cmake generator will use host architecture  by default, that is, with the steps above, cmake will build x64 binaries on x64 machine.

Visual Studio 2017 cmake generator will create 32 bit projects by default. For 64 bit, you must pass CMake the "generator" parameter using -G  in the configuration step, e.g.:

```sql
cmake .. -G "Visual Studio 15 2017 Win64"
```

#### Build 32 bit binary

With Visual Studio 2019, pass -A Win32  parameter for CMake, like this

```sql
cmake .. -A Win32
```

With Visual Studio 2017, use corresponding 32bit generator

```sql
cmake .. -G "Visual Studio 15 2017"
```

For a complete list of available generators, call "cmake" without any parameters.

### IDE Builds

Instead of calling "`cmake --build`" as above, open `MySQL.sln`. When Visual Studio starts, choose Build/Compile.

## Building the ZIP Package

```sql
cmake --build . --config relwithdebinfo --target package
```

This is how it is "done by the book",  standard cmake target.

MariaDB however uses  non-standard target "win_package" for the packaging for its releases, it generates 2 ZIPs, a slim one with executables, and another one with debuginfo (.PDB files). The debuginfo is important to be able to debug released binaries, and to analyze crashes.

```sql
cmake --build . --config relwithdebinfo --target win_package
```

## Building the MSI Package

```sql
cmake --build . --config relwithdebinfo --target MSI
```

## Including HeidiSQL in the MSI Installer

Starting with [MariaDB 5.2.7](/kb/en/mariadb-527-release-notes/), it is possible to build an installer which
includes 3rd party products, as described in
[MWL#200](http://askmonty.org/worklog/Other/?tid=200). Currently only
[HeidiSQL](http://www.heidisql.com) support is implemented; it is also
included in the official builds. Use the CMake parameter
`-DWITH_THIRD_PARTY=HeidiSQL` to include it in the installer.

## Code Signing Support for MariaDB Release Processing

MariaDB builds optionally support authenticode code signing with an optional
parameter `SIGNCODE`. Use <code class="fixed" style="white-space:pre-wrap">cmake -DSIGNCODE=1</code> during the
configuration step to sign the binaries in the `ZIP` and `MSI` packages.

<strong>Important:</strong> for `SIGNCODE=1` to work, the user that runs the build needs to
install a valid authenticode digital certificate into their certificate store,
otherwise the packaging step will fail.

## Building Packages for MariaDB Releases

The full script to create the release in an out-of-source build with Visual
Studio with signed binaries might look like:

```sql
mkdir bld
cd bld
cmake .. -DSIGNCODE=1 -DWITH_THIRD_PARTY=HeidiSQL
cmake --build . --config relwithdebinfo --target win_package
cmake --build . --config relwithdebinfo  --target MSI
```

This command sequence will produce a ZIP package (e.g mariadb-5.2.6-win32.zip)
and MSI package (e.g mariadb-5.2.6-win32.msi) in the `bld` directory.

## Running Tests

- <strong>Important: Do not use Cygwin</strong> bash, MinGW bash, Git bash, WSL bash, or any other bash when running the test suite. You will then very likely use the wrong version of Perl too (a "Unix-flavoured" one on Windows), and spend a lot of time trying to figure out why this version of Perl does not work for the test suite. Use native perl, in cmd.exe , or powershell instead,

- Switch mysql-test subdirectory of the build directory

```sql
cd C:\server\bld\mysql-test
```

- Run the test suite

```sql
perl mysql-test-run.pl --suite=main --parallel=auto
```

### Running a Test Under Debugger

Assuming VS is installed on the machine

```sql
perl mysql-test-run.pl  <test_name> --debugger=vsjitdebugger
```

If vsjitdebugger does not start, you can edit AeDebug registry key as mentioned in

[https://docs.microsoft.com/en-us/visualstudio/debugger/debug-using-the-just-in-time-debugger?view=vs-2019#jit_errors](https://docs.microsoft.com/en-us/visualstudio/debugger/debug-using-the-just-in-time-debugger?view=vs-2019#jit_errors)

Alternatively:

```sql
perl mysql-test-run.pl  <test_name> --debugger=devenv 
```

(devenv.exe needs to be in PATH)

or, if you prefer WinDBG

```sql
perl mysql-test-run.pl  <test_name> --debugger=windbg
```
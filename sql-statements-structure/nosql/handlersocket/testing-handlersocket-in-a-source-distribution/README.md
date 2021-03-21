# Testing HandlerSocket in a Source Distribution

## [MariaDB 5.5](/kb/en/what-is-mariadb-55/)

In [MariaDB 5.5](/kb/en/what-is-mariadb-55/), which is built using `cmake`, `Makefile.PL` is not generated automatically. If you want to run the perl tests, you will need to create it manually from `Makefile.PL.in`. It is fairly easy to do by replacing `LIB` and `INC` values with the correct ones. Also, `libhsclient.so` is not built by default; `libhsclient.a` can be found in `plugin/handler_socket` folder.

## [MariaDB 5.3](/kb/en/what-is-mariadb-53/)

If you want to test or use handlersocket with a source installation of [MariaDB 5.3](/kb/en/what-is-mariadb-53/),
here is one way to do this:

1 Compile with one of the build scripts that has the `-max` option,
  like `BUILD/compile-pentium64-max` or `BUILD/compile-pentium64-debug-max`
2 Start mysqld with the test framework<pre class="fixed"><span class="nb">cd</span> mysql-test
<span class="nv">LD_LIBRARY_PATH</span><span class="o">=</span>../plugin/handler_socket/libhsclient/.libs <span class="se">\</span>
<span class="nv">MTR_VERSION</span><span class="o">=</span><span class="m">1</span> perl mysql-test-run.pl --start-and-exit 1st <span class="se">\</span>
--mysqld<span class="o">=</span>--plugin-dir<span class="o">=</span>../plugin/handler_socket/handlersocket/.libs <span class="se">\</span>
--mysqld<span class="o">=</span>--loose-handlersocket_port<span class="o">=</span><span class="m">9998</span> <span class="se">\</span>
--mysqld<span class="o">=</span>--loose-handlersocket_port_wr<span class="o">=</span><span class="m">9999</span> <span class="se">\</span>
--master_port<span class="o">=</span><span class="m">9306</span> --mysqld<span class="o">=</span>--innodb
</pre>
3 This will end with:<pre class="fixed">Servers started, exiting
</pre>
4 Load handlersocket<pre class="fixed">client/mysql -uroot --protocol<span class="o">=</span>tcp --port<span class="o">=</span><span class="m">9306</span> <span class="se">\</span>
-e <span class="s1">'INSTALL PLUGIN handlersocket soname "handlersocket.so"'</span>
</pre>
5 Configure and compile the handlersocket perl module<pre class="fixed"><span class="nb">cd</span> plugin/handler_socket/perl-Net-HandlerSocket
perl Makefile.PL
make
</pre>
6 If you would like to install the handlersocket perl module permanently, you
should do:<pre class="fixed">make install
</pre>
If you do this, you don't have to set `PERL5LIB` below.
7 Run the handlersocket test suite<pre class="fixed"><span class="nb">cd</span> plugin/handler_socket/regtest/test_01_lib
<span class="nv">MYHOST</span><span class="o">=</span>127.0.0.1 <span class="nv">MYPORT</span><span class="o">=</span><span class="m">9306</span> <span class="nv">LD_LIBRARY_PATH</span><span class="o">=</span>../../libhsclient/.libs/ <span class="se">\</span>
<span class="nv">PERL5LIB</span><span class="o">=</span>../common:../../perl-Net-HandlerSocket/lib:../../perl-Net-HandlerSocket/blib/arch/auto/Net/HandlerSocket/ ./run.sh
</pre>
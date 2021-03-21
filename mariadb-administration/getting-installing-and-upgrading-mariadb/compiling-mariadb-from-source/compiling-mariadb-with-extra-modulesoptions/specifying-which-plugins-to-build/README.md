# Specifying Which Plugins to Build

By default all plugins are enabled and built as dynamic `.so` (or `.dll`) modules. If a plugin does not support dynamic builds, it is not built at all.

##### MariaDB [5.5](/kb/en/what-is-mariadb-55/) - [10.0](/kb/en/what-is-mariadb-100/)

To specify that a specific plugin should be enabled and built statically into the server executable, use the `-DWITH_xxx=1` cmake option (where `xxx` is the plugin name). Of course, static linking only works if the plugin itself supports it — some plugins can only be built as dynamic modules.

A plugin will automatically be skipped by cmake if it cannot be built; for example, if some required libraries are missing or if you want to statically link a plugin that only supports dynamic linking.

If you want to avoid  building some specific plugin, specify the `-DWITHOUT_xxx=1` cmake option.

##### MariaDB starting with [10.1](/kb/en/what-is-mariadb-101/)

Use `PLUGIN_xxx` cmake variables. They can be set on the command line with `-DPLUGIN_xxx=<em>value</em>` or in the cmake gui. Supported values are

<table><tbody><tr><th>Value</th><th>Effect</th></tr>
<tr><td><strong>NO</strong></td><td>the plugin will be not compiled at all</td></tr>
<tr><td><strong>STATIC</strong></td><td>the plugin will be compiled statically, if supported. Otherwise it will be not compiled.</td></tr>
<tr><td><strong>DYNAMIC</strong></td><td>the plugin will be compiled dynamically, if supported. Otherwise it will be not compiled. This is the default behavior.</td></tr>
<tr><td><strong>AUTO</strong></td><td>the plugin will be compiled statically, if supported. Otherwise it will be compiled dynamically.</td></tr>
<tr><td><strong>YES</strong></td><td>same as <strong>AUTO</strong>, but if plugin prerequisites (for example, specific libraries) are missing, it will not be skipped, it will abort cmake with an error.</td></tr>
</tbody></table>

Note that unlike autotools, cmake tries to configure and build incrementally. You can modify one configuration option and cmake will only rebuild the part of the tree affected by it. For example, when you do `cmake -DWITH_EMBEDDED_SERVER=1` in the already-built tree, it will make libmysqld to be built, but no other configuration options will be changed or reset to their default values.

In particular this means that if you have run, for example

##### MariaDB [5.5](/kb/en/what-is-mariadb-55/) - [10.0](/kb/en/what-is-mariadb-100/)

<code class="fixed" style="white-space:pre-wrap">cmake -DWITHOUT_OQGRAPH=1</code>

##### MariaDB starting with [10.1](/kb/en/what-is-mariadb-101/)

<code class="fixed" style="white-space:pre-wrap">cmake -DPLUGIN_OQGRAPH=NO</code>

and later you want to restore the default behavior (with OQGraph being built) in the same build tree, you would need to run

##### MariaDB [5.5](/kb/en/what-is-mariadb-55/) - [10.0](/kb/en/what-is-mariadb-100/)

<code class="fixed" style="white-space:pre-wrap">cmake -UWITHOUT_OQGRAPH</code>

##### MariaDB starting with [10.1](/kb/en/what-is-mariadb-101/)

<code class="fixed" style="white-space:pre-wrap">cmake -DPLUGIN_OQGRAPH=DYNAMIC</code>

Alternatively, you might simply delete the `CMakeCache.txt` file — this is the file where cmake stores current build configuration — and rebuild everything from scratch.
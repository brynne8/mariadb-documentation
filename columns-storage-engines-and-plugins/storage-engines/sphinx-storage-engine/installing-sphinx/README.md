# Installing Sphinx

In order to use the [Sphinx Storage Engine](/kb/en/sphinxse/), it is necessary to install the Sphinx daemon.

Many Linux distributions have Sphinx in their repositories. These can be used to install Sphinx instead of following the instructions below, but these are usually quite old versions and don't all include API's for easy integration. Ubuntu users can use the updated repository at [https://launchpad.net/~builds/+archive/sphinxsearch-rel21](https://launchpad.net/~builds/+archive/sphinxsearch-rel21) (see instructions below). Alternatively, download from [http://sphinxsearch.com/downloads/release/](http://sphinxsearch.com/downloads/release/)

## Debian and Ubuntu

Ubuntu users can make use of the repository, as follows:

```sql
sudo add-apt-repository ppa:builds/sphinxsearch-rel21
sudo apt-get update
sudo apt-get install sphinxsearch
```

Alternatively, install as follows:

- The Sphinx package and daemon are named `sphinxsearch`.
- `sudo apt-get install unixodbc libpq5 mariadb-client`
- `sudo dpkg -i sphinxsearch*.deb`
- [Configure Sphinx](/columns-storage-engines-and-plugins/storage-engines/sphinx-storage-engine/configuring-sphinx) as required
- You may need to check `/etc/default/sphinxsearch` to see that `START=yes`
- Start with `sudo service sphinxsearch start` (and stop with `sudo service sphinxsearch stop`)

## Red Hat and CentOS

- The package name is `sphinx` and the daemon `searchd`.
- `sudo yum install postgresql-libs unixODBC`
- `sudo rpm -Uhv sphinx*.rpm`
- [Configure Sphinx](/columns-storage-engines-and-plugins/storage-engines/sphinx-storage-engine/configuring-sphinx) as required
- `service searchd start`

## Windows

- Unzip and extract the downloaded zip file
- Move the extracted directory to `C:\Sphinx`
- [Configure Sphinx](/columns-storage-engines-and-plugins/storage-engines/sphinx-storage-engine/configuring-sphinx) as required
- Install as a service: 
<ul><li>`C:\Sphinx\bin&gt; C:\Sphinx\bin\searchd --install --config C:\Sphinx\sphinx.conf.in --servicename SphinxSearch`
</li></ul>

Once Sphinx has been installed, it will need to be [configured](/columns-storage-engines-and-plugins/storage-engines/sphinx-storage-engine/configuring-sphinx).

Full instructions, including details on compiling Sphinx yourself, are available at [http://sphinxsearch.com/docs/current.html](http://sphinxsearch.com/docs/current.html).
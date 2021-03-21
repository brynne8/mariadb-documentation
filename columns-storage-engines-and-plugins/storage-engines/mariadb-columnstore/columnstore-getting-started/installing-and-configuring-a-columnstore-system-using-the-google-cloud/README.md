# Installing and Configuring a ColumnStore System using the Google Cloud

### Introduction

MariaDB ColumnStore can be used a Single Node or a Multi-Node system in the Google Cloud environment. You would follow the setup procedure from the Setup document. This document shows how to set Google Cloud Instances.

You will need to have an Google Google account setup to access plus some basic knowledge of accessing and launching Google Cloud Instances.

These are the support configuration options:

1. Single-Node install

2. Multi-Node Install

- Setup with User Module and Performance Module functionality on the same
          Instance(s)

- Setup with User Module and Performance Module functionality on different
          Instance(s)

### Google Cloud Platform page

1. Create a Project

2. Go to Compute Engine Page

3. Go to Images page and select and Create an image, recommend Centos 7 is you don't have a preference. If planning a multi-node system, create # number of instances you need for your system. A 1um and 2 pm (3 instances) is a typical setup.

a. Machine type - Minimum for testing would be 8 vCPU's, the more cpu and 
        memory you have the better the performance will be.

b. SSD disk, minimum 100gb. But this again is based on the size you would need 
         for your database

### SSH into Instance(s)

Check the Google Cloud Document on the different ways to access the instances using ssh.

[https://cloud.google.com/compute/docs/instances/connecting-to-instance](https://cloud.google.com/compute/docs/instances/connecting-to-instance)

Here is a link to the initial setup document that you will need to follow:

[https://mariadb.com/kb/en/mariadb/preparing-for-columnstore-installation](https://mariadb.com/kb/en/mariadb/preparing-for-columnstore-installation)

1. Setup on each instance

a. It is recommended that ssh-keys be setup for multi-node installs, so that would need
        to be done on all nodes. 'ssh' is used during the installation process.

b. Installed MariaDB ColumnStore dependent packages

c. make sure the firewalls are disabled as stated in the Prepare Document.

2. On just one of the instance, which will be used as pm1

a. Installed MariaDB ColumnStore

b. Run postConfigure and configured pm1

NOTE: Can you follow the postConfigure install instructions from the Single-Server Install document and the Multi-server Install Document.

[https://mariadb.com/kb/en/mariadb/installing-and-configuring-mariadb-columnstore](https://mariadb.com/kb/en/mariadb/installing-and-configuring-mariadb-columnstore)

[https://mariadb.com/kb/en/mariadb/installing-and-configuring-a-multi-server-columnstore-system](https://mariadb.com/kb/en/mariadb/installing-and-configuring-a-multi-server-columnstore-system)

NOTE: When configuring the nodes, use the Internal IP addresses.
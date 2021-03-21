# Installing and Configuring a ColumnStore System using the Amazon AMI

## MariaDB ColumnStore AMI

The MariaDB ColumnStore AMI is CentOS 7 based with MariaDB ColumnStore GA packages.

The MariaDB Columnstore CentOS 7 binary package is installed in the user account:.

- ‘mariadb-user’ on MariaDB ColumnStore 1.2.0 and before releases
- 'mysql' on MariaDB ColumnStore 1.2.1 and later releases

The user account will be referred to as &lt;USER&gt; within this document.

This is a non-root install where the user will access and run all the commands via the user &lt;USER&gt;. The &lt;USER&gt; has been given 'sudo' privileges to run root level commands, which is required due to the fact that the MariaDB ColumnStore applications run some of these commands internally.

MariaDB ColumnStore can be used a Single Node or a Multi-Node system. And it supports using both local storage and EBS storage's.

You will need to have an AWS account setup to access and use the MariaDB ColumnStore AMI, plus some basic knowledge of accessing and launching AMI's and Instances.

These are the support configuration options:

1. Single-Node install with or without EBS Storages

2. Multi-Node Install with or without EBS Storages

- Setup with User Module and Performance Module functionality on the same
          Instance(s)

- Setup with User Module and Performance Module functionality on different
          Instance(s)

Also of note, MariaDB ColumnStore product also allows the user to install the RPMs and Binary packages on Amazon EC2 instances to create a system that performs like a standard hardware install. It's not required to run MariaDB ColumnStore via the use of this AMI. To do so, just follow the install instructions from the Single Serve and Multi Server install guides.

### New Installation and upgrades

The AMI is used for creating a new MariaDB ColumnStore. It will either create the Instances and EBS storages needed during startup, or use ones that you have already created and want to utilize.

On doing upgrades, you would follow the procedure that is shown in the associated Upgrade Document.

### Amazon AWS-CLI Tool Set

The MariaDB ColumnStore AMI comes installed with the Amazon AWS-CLI Tool set package, version 1.11.36. The Amazon AWS-CLI Tools provides the capability to the MariaDB ColumnStore to create EC2 Instances and EBS storages. It allows allows for EC2 Instance/node failover, meaning if an EC2 Instance goes down or stops communicating, MariaDB ColumnStore will handle that problem keeping the system functional. This might consist of re-attaching an EBS storage device from the problem EC2 Performance Module to an another Performance Module in the system. Also if a EC2 Instance was to terminate, MariaDB ColumnStore will launch another EC2 Instance in its place.

Even though this AMI has the Amazon AWS-CLI Tool set installed, the user has the option to Setup and Configure a MariaDB ColumnStore System using this AMI without the use of these tools and having them disabled. That is discussed later.

Here is a complete list of these commands:

[http://docs.aws.amazon.com/cli/latest/reference/ec2/index.html#cli-aws-ec2](http://docs.aws.amazon.com/cli/latest/reference/ec2/index.html#cli-aws-ec2)

Here is the EC2 API Tools commands that the MariaDB ColumnStore Software calls:

1 ec2-describe-instances
2 ec2-run-instances
3 ec2-terminate-instances
4 ec2-stop-instances
5 ec2-start-instances
6 ec2-associate-address
7 ec2-disassociate-address
8 ec2-create-volume
9 ec2-describe-volumes
10 ec2-detach-volume
11 ec2-attach-volume
12 ec2-delete-volume
13 ec2-create-tags

Features in the AMI using MariaDB ColumnStore install script (postConfigure) 
    and mcsadmin

- Both can create the additional Instances and EBS storage's that are used in 
         the system. mcsadmin via the addModule/addDbroot commands.

- Both will allow user to enter EC2 Instance IDs and EBS Volume IDs, if
         you want to utilize AWS devices you have already created.

- You can also install via postConfigure and have the Amazon AMI tools 
          functionality disabled, meaning you can install like like a regular hardware
          setup by providing host-names and IP Addresses during the configuration
          process.

To do this, you would enter 'n' at this prompt:

```sql
Do you want to have ColumnStore use the Amazon AWS CLI Tools [y,n] (y) >
```

### Amazon AWS setup and AMI Launching

The MariaDB ColumnStore AMI Instance uses 'iops' EBS storage for improved performance over the standard storage type. But it also means that based on the number of Instances/Module you plan to launch, you will need to check your account to make sure you have the permissions to create enough 'iops' storages. Amazon does have a limit on this.

Recommend use Instance Type of "m4.4xlarge" or large. Need to have high performance network.

#### AWS Certifications Keys - Access and Secret Keys

If you are doing a Amazon Multi-Node install and you want the ColumnStore processes to utilize the Amazon API Tool, you will need to either Provide an IAM role that has the Certifications Keys associated with it or you can update a file locally in the AMI Instance.
To add it in the local file, you will need to do the following once the AMI Instance is launched and you are logged in using the &lt;USER&gt; user.

```sql
# cd .aws
# mv credentials.template credentials
// edit the file and add in the your access and secret key that was created for your User account
   aws_access_key_id = xxxxxxxxxxxxxx
   aws_secret_access_key = xxxxxxxxxxx
```

These keys aren't required for a single-node install.

[https://aws.amazon.com/developers/access-keys/](https://aws.amazon.com/developers/access-keys/)

### Amazon IAM Role

If you want to setup an IAM role where it passes in the credentials, here is some information on how to do that.
Part of setting up the IAm role is you will be required also to Grant permissions to allow this role access to the EC2 command set.

Here is some links to Amazon on what the IAM is and how to set it up.

[http://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html](http://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)

[http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-policies-for-amazon-ec2.html](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-policies-for-amazon-ec2.html)

### AWS Management Console page

1. Create and download a Key-Pair, this is used to log into the AMI Instance
2. Create a VPC Security Group with the following Inbound Rules.

NOTE: XXX.XXX.XXX.XXX is the VPC is the default subnet used in your account. So you will need to put the VPC IP Address for your account. The IP Address can be found via the VPC Dashboard

- SSH - Source = Your Public IP address
- SSH - AMI subnet XXX.XXX.XXX.XXX/16
- All ICMP
- Custom TCP rule - port range 8600-8800, subnet XXX.XXX.XXX.XXX/16
- MYSQL TCP (port 3306) - subnet XXX.XXX.XXX.XXX/16
- MYSQL TCP (port 3306) - Any other Public IP where you what to access the console
3. Select and Launch the MariaDB ColumnStore AMI:
- From AWS Console on the Instance page:
<ul start="1"><li>Press 'Launch Instance' button
</li><li>Select "Community AMIs" and search for "MariaDB-ColumnStore-1.0.7" and select
</li><li>Select Size : Recommend using Instance type of "m4.2xlarge" type to get optimal
       performance.
</li><li>Press "Next: Configure Instance Details" Button
<ul start="1"><li>Select VPC/Subnet and optional provide an "IAM role" 
</li></ul>
</li><li>Press "Next: Add Storage" button
</li><li>Storage size = default is 100 gb, can be increased if using local storage
</li><li>Press "Next: Add Tags"
</li><li>Value = Name of Instance, which is systemName-pm1. I.E. columnstore-1-pm1
</li><li>Press "Next: Configure Security Group" button
</li><li>Either select Security Group or create one with the ColumnStore setup
</li><li>Press "Review and Launch" button
</li><li>Select you Key Pair name and Launch Instance
</li></ul>

#### Amazon AMI EC2 Instance user and password setup

The MariaDB ColumnStore AMI is setup to be accessed and run from the &lt;USER&gt; account

It comes with ssh-keys setup and these used during the install process.

### Access to Launched AMI Instance

Log into the instance as user &lt;USER&gt; or 'mysql'. If the connection link has 'root' user, you will need to change that to &lt;USER&gt; or 'mysql'. Here is an example:

Link from the AWS Console:

```sql
ssh -i "xxxxx.pem" root@xxxxxxxxxxx.us-west-2.compute.amazonaws.com
```

Change to on MariaDB ColumnStore 1.2.0 and before releases

```sql
ssh -i "xxxxx.pem" mariadb-user@xxxxxxxxxxx.us-west-2.compute.amazonaws.com
```

Change to on MariaDB ColumnStore 1.2.1 and later releases

```sql
ssh -i "xxxxx.pem" mysql@xxxxxxxxxxx.us-west-2.compute.amazonaws.com
```

### MariaDB ColumnStore System installation

There are 2 methods for perform the System Configuration and Installation:

- MariaDB ColumnStore One Step Quick Installer Script - Release 1.1.6 and later
- MariaDB ColumnStore Post Install Script

#### MariaDB ColumnStore One Step Quick Installer Script, quick_installer_amazon.sh

MariaDB ColumnStore One Step Quick Installer Script, quick_installer_amazon.sh, is used to perform a simple 1 command run to perform the configuration and startup of MariaDB ColumnStore package on a Aamazon AMI system setup. Available in Release 1.1.6 and later.

The script has 4 parameters.

- --pm-count=x Number of pm instances to create
- --um-count=x Number of um instances to create, optional
- --system-name=nnnn System Name, optional

It will perform an install with these defaults:

- System-Name = columnstore-1, when not specified
- Multi-Server Install
<ul start="1"><li>with X Number of PMs when only PM IP count are provided
</li><li>with X Number of UMs when UM IP count are provided and X Number of UMs when PM IP count are provided
</li></ul>
- Storage - Internal
- DBRoot - 1 DBroot per 1 Performance Module
- Local Query is disabled on um/pm install
- MariaDB Replication is enabled

##### Running quick_installer_multi_server.sh help as non-root &lt;USER&gt; user

```sql
/home/<USER>/mariadb/columnstore/bin/quick_installer_amazon.sh --help
Usage ./quick_installer_amazon.sh [OPTION]

Quick Installer for an Amazon MariaDB ColumnStore Install
This requires to be run on a MariaDB ColumnStore AMI

Performace Module (pm) number is required
User Module (um) number is option
When only pm counts provided, system is combined setup
When both pm/um counts provided, system is seperate setup

--pm-count=x Number of pm instances to create
--um-count=x Number of um instances to create, optional
--system-name=nnnn System Name, optional
```

##### Running quick_installer_multi_server.sh 1um/2pm setup as non-root &lt;USER&gt; user

```sql
$ ./mariadb/columnstore/bin/quick_installer_amazon.sh --pm-count=2 --um-count=1

NOTE: Performing a Multi-Server Seperate install with um and pm running on seperate servers

Run post-install script

The next steps are:

If installing on a pm1 node:

export COLUMNSTORE_INSTALL_DIR=/home/<USER>/mariadb/columnstore
export LD_LIBRARY_PATH=:/home/<USER>/mariadb/columnstore/lib:/home/<USER>/mariadb/columnstore/mysql/lib:/home/<USER>/mariadb/columnstore/lib:/home/<USER>/mariadb/columnstore/mysql/lib:/home/<USER>/mariadb/columnstore/lib:/home/<USER>/mariadb/columnstore/mysql/lib:/home/<USER>/mariadb/columnstore/lib:/home/<USER>/mariadb/columnstore/mysql/lib:/home/<USER>/mariadb/columnstore/lib:/home/<USER>/mariadb/columnstore/mysql/lib
/home/<USER>/mariadb/columnstore/bin/postConfigure -i /home/<USER>/mariadb/columnstore -d

If installing on a non-pm1 using the non-distributed option:

export COLUMNSTORE_INSTALL_DIR=/home/<USER>/mariadb/columnstore
export LD_LIBRARY_PATH=:/home/<USER>/mariadb/columnstore/lib:/home/<USER>/mariadb/columnstore/mysql/lib:/home/<USER>/mariadb/columnstore/lib:/home/<USER>/mariadb/columnstore/mysql/lib:/home/<USER>/mariadb/columnstore/lib:/home/<USER>/mariadb/columnstore/mysql/lib:/home/<USER>/mariadb/columnstore/lib:/home/<USER>/mariadb/columnstore/mysql/lib:/home/<USER>/mariadb/columnstore/lib:/home/<USER>/mariadb/columnstore/mysql/lib
/home/<USER>/mariadb/columnstore/bin/columnstore start

Run postConfigure script


This is the MariaDB ColumnStore System Configuration and Installation tool.
It will Configure the MariaDB ColumnStore System and will perform a Package
Installation of all of the Servers within the System that is being configured.

IMPORTANT: This tool requires to run on the Performance Module #1

With the no-Prompting Option being specified, you will be required to have the following:

 1. Root user ssh keys setup between all nodes in the system or
    use the password command line option.
 2. A Configure File to use to retrieve configure data, default to Columnstore.xml.rpmsave
    or use the '-c' option to point to a configuration file.


===== Quick Install Amazon Configuration =====



===== Setup System Module Type Configuration =====

There are 2 options when configuring the System Module Type: separate and combined

  'separate' - User and Performance functionality on separate servers.

  'combined' - User and Performance functionality on the same server

Select the type of System Module Install [1=separate, 2=combined] (1) > 

Seperate Server Installation will be performed.

NOTE: Local Query Feature allows the ability to query data from a single Performance
      Module. Check MariaDB ColumnStore Admin Guide for additional information.

Enable Local Query feature? [y,n] (n) > 

NOTE: The MariaDB ColumnStore Schema Sync feature will replicate all of the
      schemas and InnoDB tables across the User Module nodes. This feature can be enabled
      or disabled, for example, if you wish to configure your own replication post installation.

MariaDB ColumnStore Schema Sync feature is Enabled, do you want to leave enabled? [y,n] (y) > 

NOTE: Configured to have ColumnStore use the Amazon AWS CLI Tools


NOTE: MariaDB ColumnStore Replication Feature is enabled

Enter System Name (columnstore-1) > 


===== Setup Storage Configuration =====

----- Setup User Module MariaDB ColumnStore Data Storage Mount Configuration -----

There are 2 options when configuring the storage: internal and external

  'internal' -    This is specified when a local disk is used for the Data storage.

  'external' -    This is specified when the MariaDB ColumnStore Data directory is externally mounted.

Select the type of Data Storage [1=internal, 2=external] (1) > 

----- Setup Performance Module DBRoot Data Storage Mount Configuration -----

There are 2 options when configuring the storage: internal or external

  'internal' -    This is specified when a local disk is used for the DBRoot storage.
                  High Availability Server Failover is not Supported in this mode

  'external' -    This is specified when the DBRoot directories are mounted.
                  High Availability Server Failover is Supported in this mode.

Select the type of Data Storage [1=internal, 2=external] (1) > 

===== Setup Memory Configuration =====

NOTE: Setting 'NumBlocksPct' to 70%
      Setting 'TotalUmMemory' to 50%

===== Setup the Module Configuration =====

Amazon Install: For Module Configuration, you have the option to provide the
existing Instance IDs or have the Instances created.
You will be prompted during the Module Configuration setup section.

----- User Module Configuration -----

Enter number of User Modules [1,1024] (1) > 

*** User Module #1 Configuration ***


Launched Instance for um1: i-0eac6a122c640e46c
Getting Private IP Address for Instance i-0eac6a122c640e46c, please wait...
Private IP Address of i-0eac6a122c640e46c is 172.31.33.235


----- Performance Module Configuration -----

Enter number of Performance Modules [1,1024] (2) > 

*** Parent OAM Module Performance Module #1 Configuration ***

EC2 Instance ID for pm1: i-0b309a68398cf9302
Getting Private IP Address for Instance i-0b309a68398cf9302, please wait...
Private IP Address of i-0b309a68398cf9302 is 172.31.44.176

Enter the list (Nx,Ny,Nz) or range (Nx-Nz) of DBRoot IDs assigned to module 'pm1' (1) > 

*** Performance Module #2 Configuration ***


Launched Instance for pm2: i-088f06a60d267e517
Getting Private IP Address for Instance i-088f06a60d267e517, please wait...
Private IP Address of i-088f06a60d267e517 is 172.31.40.50

Enter the list (Nx,Ny,Nz) or range (Nx-Nz) of DBRoot IDs assigned to module 'pm2' (2) > 

Next step is to enter the password to access the other Servers.
This is either your password or you can default to using a ssh key
If using a password, the password needs to be the same on all Servers.

Enter password, hit 'enter' to default to using a ssh key, or 'exit' > 

===== System Installation =====

System Configuration is complete.
Performing System Installation.

Performing a MariaDB ColumnStore System install using a Binary package
located in the /home/<USER> directory.



----- Performing Install on 'um1 / i-0eac6a122c640e46c' -----

Install log file is located here: /tmp/um1_binary_install.log


----- Performing Install on 'pm2 / i-088f06a60d267e517' -----

Install log file is located here: /tmp/pm2_binary_install.log


MariaDB ColumnStore Package being installed, please wait ...  DONE

===== Checking MariaDB ColumnStore System Logging Functionality =====

The MariaDB ColumnStore system logging is setup and working on local server

===== MariaDB ColumnStore System Startup =====

System Configuration is complete.
Performing System Installation.

----- Starting MariaDB ColumnStore on local server -----

MariaDB ColumnStore successfully started

MariaDB ColumnStore Database Platform Starting, please wait ........... DONE

System Catalog Successfully Created

Run MariaDB ColumnStore Replication Setup..  DONE

MariaDB ColumnStore Install Successfully Completed, System is Active

Enter the following command to define MariaDB ColumnStore Alias Commands

. /etc/profile.d/columnstoreEnv.sh
. /etc/profile.d/columnstoreAlias.sh

Enter 'mcsmysql' to access the MariaDB ColumnStore SQL console
Enter 'mcsadmin' to access the MariaDB ColumnStore Admin console

NOTE: The MariaDB ColumnStore Alias Commands are in /etc/profile.d/columnstoreAlias.sh
```

#### MariaDB ColumnStore Post Install Script, postConfigure

Follow the instructions from the README file to setup and install MariaDB Columnstore

The Instance that was launched is ColumnStore Performance Module #1. This is the primary "pm" where the install process will always start. The install and configuration script, postConfigure is run to initiate the install process.

If you are installing as a Single-Node system, you can follow install instructions on how to run postConfigure in the follow document for non-root user:

[Installing and Configuring MariaDB ColumnStore](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-getting-started/preparing-and-installing-mariadb-columnstore-10x/installing-and-configuring-mariadb-columnstore)

The following is a transcript of a typical run of the MariaDB ColumnStore configuration script. Plain-text formatting indicates output from the script and bold text indicates responses to questions. After each question there is a short discussion of what the question is asking and what some typical answers might be. You will not see these discussions the running the actual configuration script.

###### Common Installation Examples

During postConfigure, there are 2 questions that are asked where the answer given determines the path
that postConfigure takes in configuring the system. Those 2 questions are as follows:

```sql
Select the type of server install [1=single, 2=multi] (2) >
```

and

```sql
Select the Type of Module Install being performed:
1. Separate - User and Performance functionalities on separate servers
2. Combined - User and Performance functionalities on the same server
Enter Server Type ID [1-2] (1) >
```

The following examples illustrates some common configurations and helps to provide answers to the above
questions:

- Single Node - User and Performance running on 1 server - single / combined
- Mutli-Node #1 -  User and Performance running on some server - multi / combined
- Mutli-Node #2 -  User and Performance running on separate servers - multi / separate

Run the postConfigure script first:

```sql
/home/<USER>/mariadb/columnstore/bin/postConfigure -i /home/<USER>/mariadb/columnstore -d
```

```sql
This is the MariaDB Columnstore System Configuration and Installation tool.
It will Configure the MariaDB Columnstore System and will perform a Package
Installation of all of the Servers within the System that is being configured.

IMPORTANT: This tool should only be run on the Parent OAM Module
           which is a Performance Module, preferred Module #1

Prompting instructions:

	Press 'enter' to accept a value in (), if available or
	Enter one of the options within [], if available, or
	Enter a new value


===== Setup System Server Type Configuration =====

There are 2 options when configuring the System Server Type: single and multi

  'single'  - Single-Server install is used when there will only be 1 server configured
              on the system. It can also be used for production systems, if the plan is
              to stay single-server.

  'multi'   - Multi-Server install is used when you want to configure multiple servers now or
              in the future. With Multi-Server install, you can still configure just 1 server
              now and add on addition servers/modules in the future.

Select the type of System Server install [1=single, 2=multi] (2) > <Enter>


===== Setup System Module Type Configuration =====

There are 2 options when configuring the System Module Type: separate and combined

  'separate' - User and Performance functionality on separate servers.

  'combined' - User and Performance functionality on the same server

Select the type of System Module Install [1=separate, 2=combined] (2) > 1

Separate Server Installation will be performed.

NOTE: Local Query Feature allows the ability to query data from a single Performance
      Module. Check MariaDB Columnstore Admin Guide for additional information.

Enable Local Query feature? [y,n] (n) > <Enter>

NOTE: The MariaDB ColumnStore Schema Sync feature will replicate all of the
      schemas and InnoDB tables across the User Module nodes. This feature can be enabled
      or disabled, for example, if you wish to configure your own replication post installation.

MariaDB ColumnStore Schema Sync feature, do you want to enable? [y,n] (y) > 

NOTE: Amazon AWS CLI Tools are installed and allow MariaDB ColumnStore to create Instances and ABS Volumes

Do you want to have ColumnStore use the Amazon AWS CLI Tools [y,n] (y) >

Enter System Name (columnstore-1) > mycs1
```

Notes: You should give this system a name that will appear in various Admin utilities, etc. The name can be composed of any number of printable characters and spaces.

```sql
===== Setup Storage Configuration =====

----- Setup User Module MariaDB Columnstore Data Storage Mount Configuration -----

There are 2 options when configuring the storage: internal and external

  'internal' -    This is specified when a local disk is used for the Data storage.

  'external' -    This is specified when the MariaDB Columnstore Data directory is externally mounted.

Select the type of Data Storage [1=internal, 2=external] (1) > <Enter>
```

Notes: Enter 2 if you want to create a EBS storage for the mysql schema's to be stored.

```sql
----- Setup Performance Module DBRoot Data Storage Mount Configuration -----

There are 2 options when configuring the storage: internal or external

  'internal' -    This is specified when a local disk is used for the DBRoot storage.
                  High Availability Server Failover is not Supported in this mode

  'external' -    This is specified when the DBRoot directories are mounted.
                  High Availability Server Failover is Supported in this mode.

Select the type of Data Storage [1=internal, 2=external] (1) > 2
```

Notes: Enter 2 if you want to create a EBS storage for the MariaDB ColumnStore DBRoot Data to be stored. When using option 2, this will allow this EBS volume to be reattached to other Performance Modules during failover situations

```sql
Enter EBS Volume Type (standard, gp2, io1) : (standard) > <Enter>

Enter EBS Volume storage size in GB: [1,16384] (100) > <Enter>

===== Setup Memory Configuration =====

NOTE: Setting 'NumBlocksPct' to 70%
      Setting 'TotalUmMemory' to 50%

===== Setup the Module Configuration =====

Amazon Install: For Module Configuration, you have the option to provide the
existing Instance IDs or have the Instances created.
You will be prompted during the Module Configuration setup section.

----- User Module Configuration -----

Enter number of User Modules [1,1024] (1) > <Enter>

*** User Module #1 Configuration ***

Create Instance for um1 [y,n] (y) > <Enter>
```

Notes: By entering 'y', Instances will automatically be created. If there are Instances already created that you want to use, then enter 'n' and you will be prompted for the Instance ID.

```sql
Launched Instance for um1: i-05ab2990
Getting Private IP Address for Instance i-05ab2990, please wait...
Private IP Address of i-05ab2990 is 172.30.0.110

----- Performance Module Configuration -----

Enter number of Performance Modules [1,1024] (1) > 2

*** Parent OAM Module Performance Module #1 Configuration ***

EC2 Instance ID for pm1: i-a3a82a36
Getting Private IP Address for Instance i-a3a82a36, please wait...
Private IP Address of i-a3a82a36 is 172.30.0.107

Enter the list (Nx,Ny,Nz) or range (Nx-Nz) of DBRoot IDs assigned to module 'pm1' (1) > <Enter>

*** Setup External EBS Volume for DBRoot #1 ***

*** NOTE: You have the option to provide an
          existing EBS Volume ID or have a Volume created

Create a new EBS volume for DBRoot #1 ?  [y,n] (y) > <Enter>
  Create AWS Volume for DBRoot #1
  Formatting DBRoot #1, please wait...
```

Notes: By entering 'y', EBS Volumnes will automatically be created. If there are EBS Volumnes already created that you want to use, then enter 'n' and you will be prompted for the Volumne ID.

```sql
*** Performance Module #2 Configuration ***

Create Instance for pm2 [y,n] (y) > <Enter>

Launched Instance for pm2: i-1ca82a89
Getting Private IP Address for Instance i-1ca82a89, please wait...
Private IP Address of i-1ca82a89 is 172.30.0.238

Enter the list (Nx,Ny,Nz) or range (Nx-Nz) of DBRoot IDs assigned to module 'pm2' () > 2

*** Setup External EBS Volume for DBRoot #2 ***

*** NOTE: You have the option to provide an
          existing EBS Volume ID or have a Volume created

Create a new EBS volume for DBRoot #2 ?  [y,n] (y) > <Enter>
  Create AWS Volume for DBRoot #2
  Formatting DBRoot #2, please wait...

===== System Installation =====

System Configuration is complete.
Performing System Installation.

Performing an MariaDB Columnstore System install using a Binary package located in the /home/<USER> directory.

Next step is to enter the password to access the other Servers.
This is either your password or you can default to using a ssh key
If using a password, the password needs to be the same on all Servers.

Enter password, hit 'enter' to default to using a ssh key, or 'exit' > <Enter>
```

Notes: Hit Enter since ssh-keys are setup

```sql

----- Performing Install on 'um1 / i-05ab2990' -----

Install log file is located here: /tmp/um1_binary_install.log


----- Performing Install on 'pm2 / i-1ca82a89' -----

Install log file is located here: /tmp/pm2_binary_install.log


MariaDB Columnstore Package being installed, please wait ...  DONE

===== Checking MariaDB Columnstore System Logging Functionality =====

The MariaDB Columnstore system logging is setup and working on local server

MariaDB Columnstore System Configuration and Installation is Completed

===== MariaDB Columnstore System Startup =====

System Installation is complete. If any part of the install failed,
the problem should be investigated and resolved before continuing.

Would you like to startup the MariaDB Columnstore System? [y,n] (y) > <Enter>

----- Starting MariaDB Columnstore on 'um1' -----

MariaDB Columnstore successfully started

----- Starting MariaDB Columnstore on 'pm2' -----

MariaDB Columnstore successfully started

----- Starting MariaDB Columnstore on local server -----

MariaDB Columnstore successfully started

MariaDB Columnstore Database Platform Starting, please wait ....... DONE

System Catalog Successfully Created

MariaDB Columnstore Install Successfully Completed, System is Active

Enter the following command to define MariaDB Columnstore Alias Commands

. /home/<USER>/mariadb/columnstore/bin/columnstoreAlias

Enter 'mcsmysql' to access the MariaDB Columnstore SQL console
Enter 'mcsadmin' to access the MariaDB Columnstore Admin console

```

## MariaDB Columnstore Memory Configuration

During the installation process, postConfigure will set the 2 main Memory configuration settings based on the size of memory detected on the local node.

```sql
'NumBlocksPct'   - Performance Module Data cache memory setting

TotalUmMemory  - User Module memory setting, used as temporary memory for joins
```

On a system that has the Performance Module and User Module functionality combined on the same server, this is the default settings:

```sql
NumBlocksPct    - 50% of total memory

TotalUmMemory - 25% of total memory, default maximum the percentage equal to 16G
```

On a system that has the Performance Module and User Module functionality on different servers, this is the default settings:

```sql
NumBlocksPct    - This setting is NOT configured, and the default that the applications will then use is 70%

TotalUmMemory - 50% of total memory 
```

The user can choose to change these settings after the install is completed, if for instance they want to setup more memory for Joins to utilize. On a single server or combined UM/PM server, it is recommended to not
have the combination of these 2 settings over 75% of total memory

## MariaDB Columnstore System Configuration settings

### vm.swappiness

The default setting for vm.swappiness is set to 30. This setting can be overridden by user on each instance.

To change the system swappiness value, open /etc/sysctl.conf as root. Then, change or add this line to the file:

vm.swappiness = 1

Reboot for the change to take effect

You can also change the value while your system is still running

sysctl vm.swappiness=1

#### Set the user file limits (by root user)

ColumnStore needs the open file limit to be increased for the specified user. To do this edit the /etc/security/limits.conf file and make the following additions at the end of the file:

```sql
<USER> hard nofile 65536
<USER> soft nofile 65536
```
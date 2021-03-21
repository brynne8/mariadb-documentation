# Running mysqld as root

MariaDB should never normally be run as the system's root user (this is unrelated to the MariaDB root user). If it is, any user with the FILE privilege can create or modify any files on the server as root.

MariaDB will normally return the error <strong> Fatal error: Please read "Security" section of the manual to find out how to run mysqld as root!</strong> if you attempt to run mysqld as root. If you need to override this restriction for some reason, start mysqld with the <a undefined>user=root</a> option.

Better practice, and the default in most situations, is to use a separate user, exclusively used for MariaDB. In most distributions, this user is called `mysql`.
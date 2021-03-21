# Machine Learning with MindsDB

## Overview

[MindsDB](https://docs.mindsdb.com/databases/) is a third-party application that interfaces with MariaDB Server to provide Machine Learning capabilities through SQL. The interface is done via the [Connect Storage Engine](/columns-storage-engines-and-plugins/storage-engines/connect).

## Installation

To get a functional MariaDB - MindsDB installation, one needs to install the following components:

- <strong>MindsDB</strong>: follow the instructions in the project's [official documentation](https://docs.mindsdb.com/installation/Installing/).
- <strong>Connect Storage Engine</strong> must be enabled for the integration to work. See [installing the connect storage engine](/columns-storage-engines-and-plugins/storage-engines/connect/installing-the-connect-storage-engine).

MindsDB connects to MariaDB Server via a regular user to setup a dedicated database called `mindsdb`. Which user will be used is specified within MindsDB's [configuration file](https://docs.mindsdb.com/databases/MariaDB/).

For example, if MindsDB is installed locally, one can create a user called `mindsdb@localhost`. MindsDB only authenticates via the [mysql_native_password](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-mysql_native_password) plugin, hence one must set a password for the user:

```sql
CREATE USER mindsdb@localhost;
SET PASSWORD for mindsdb@localhost=PASSWORD("password");
```

The user must be granted the global [FILE](/kb/en/grant/#file) privilege and all privileges on the `mindsdb` database.

```sql
GRANT FILE on *.* to mindsdb@localhost;
GRANT ALL on mindsdb.* to mindsdb@localhost;
```

Assuming MindsDB is in the python path one can start up MindsDB with the following parameters:

```sql
python -m mindsdb --config=$CONFIG_PATH --api=http,mysql
```

Make sure `$CONFIG_PATH` points to the appropriate MindsDB configuration file.

## Usage

Always consult the project's [official documentation](https://docs.mindsdb.com/installation/Installing/) for up-to-date usage scenarios as MindsDB is an actively developed project.

For a step-by-step example, you can consult the following [blog post](https://mariadb.org/machine-learning-sql/).

If the connection between MindsDB and MariaDB is successful, you should see the `mindsdb` database present and two tables within it: `commands` and `predictors`.

MindsDB, as an AutoML framework does all the work when it comes to training the AI model. What is necessary is to pass it the initial data, which MindsDB retrieves via a SELECT statement. This can be done by inserting into the `predictors` table.

```sql
INSERT INTO `predictors`
       (`name`, `predict`, `select_data_query`)
VALUES ('bikes_model', 'count', 'SELECT * FROM test.bike_data');
```

The values inserted into predictors act as a command instructing MindsDB to:

1 Train a model called 'bikes_model'
2 From the input data, learn to predict the 'count' column.
3 The input data is generated via the select statement 'SELECT * FROM test.bike_data'.
The `select_data_query` should be a valid select that MindsDB can run against MariaDB.
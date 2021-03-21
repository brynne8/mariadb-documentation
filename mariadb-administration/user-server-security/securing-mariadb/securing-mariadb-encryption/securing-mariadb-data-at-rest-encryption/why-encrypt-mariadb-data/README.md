# Why Encrypt MariaDB Data?

##### MariaDB starting with [10.1](/kb/en/what-is-mariadb-101/)

Encryption of tables and tablespaces was added in [MariaDB 10.1](/kb/en/what-is-mariadb-101/)

Nearly everyone owns data of immense value: customer data, construction plans, recipes, product designs and other information. These data are stored in clear text on your storage media. Everyone with file system access is able to read and modify the data. If this data falls into the wrong hands (criminals or competitors) this may result in serious consequences.

With encryption you protect Data At Rest (see the [Wikipedia article](http://en.wikipedia.org/wiki/Data_at_Rest)). That way, the database files are protected against unauthorized access.

## When Does Encryption Help to Protect Your Data?

Encryption helps in case of threats against the database files:

- An attacker gains access to the system and copies the database files to avoid the MariaDB authorization check.
- MariaDB is operated by a service provider who should not gain access to the sensitive data.

## When is Encryption No Help?

Encryption provides no additional protection against threats caused by authorized database users. Specifically, SQL injections aren’t prevented.

## What to Encrypt?

All data that is not supposed to fall into possible attackers hands should be encrypted. Especially information, subject to strict data protection regulations, is to be protected by encryption (e.g. in the healthcare sector: patient records). Additionally data being of interest for criminals should be protected. Data which should be encrypted are:

- Personal related information
- Customer details
- Financial and credit card data
- Public authorities data
- Construction plans and research and development results

## How to Handle Key Management?

There are currently three options for key management:

- [File Key Management Plugin](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/key-management-and-encryption-plugins/file-key-management-encryption-plugin/)
- [AWS Key Management Plugin](/kb/en/aws-key-management-encryption-plugin/)
- [eperi Gateway for Databases](/kb/en/encryption-key-management/#eperi-gateway-for-databases)

See [Encryption Key Management](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/key-management-and-encryption-plugins/encryption-key-management/) for details.
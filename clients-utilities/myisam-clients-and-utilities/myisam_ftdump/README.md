# myisam_ftdump

myisam_ftdump is a utility for displaying information about [MyISAM](/kb/en/myisam/) [FULLTEXT](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes/) indexes. It will scan and dump the entire index, and can be a lengthy process.

If the server is running, make sure you run a [FLUSH TABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/) statement first.

## Usage

```sql
myisam_ftdump <table_name> <index_num>
```

The table_name can be specified with or without the .MYI index extension.

The index number refers to the number of the index when the table was defined, starting at zero. For example, take the following table definition:

```sql
CREATE TABLE IF NOT EXISTS `employees_example` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `first_name` varchar(30) NOT NULL,
  `last_name` varchar(40) NOT NULL,
  `position` varchar(25) NOT NULL,
  `home_address` varchar(50) NOT NULL,
  `home_phone` varchar(12) NOT NULL,
  `employee_code` varchar(25) NOT NULL,
  `bio` text NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `employee_code` (`employee_code`),
  FULLTEXT (`bio`)
) ENGINE=MyISAM;
```

The fulltext index will be `2`. The primary key is index `0`, and the unique key index `1`.

You can use <em>myisam_ftdump</em> to generate a list of index entries in order of frequency of occurrence as follows:

```sql
myisam_ftdump -c mytexttable 1 | sort -r
```

## Options

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td>-h, --help</td><td>Display help and exit.</td></tr>
<tr><td>-?, --help</td><td>Synonym for -h.</td></tr>
<tr><td>-c, --count</td><td>Calculate per-word stats (counts and global weights).</td></tr>
<tr><td>-d, --dump</td><td>Dump index (incl. data offsets and word weights).</td></tr>
<tr><td>-l, --length</td><td>Report length distribution.</td></tr>
<tr><td>-s, --stats</td><td>Report global stats.</td></tr>
<tr><td>-v, --verbose</td><td>Be verbose.</td></tr>
</tbody></table>
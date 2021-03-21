# Adding DataFlex 3.1c .dat Files As An External Table Type With CONNECT

I'm using MariaDB's CONNECT engine to access / utilize a set of Visual FoxPro / dBase files as part of supporting an extremely old piece of software for a client. It's works SO well, and I'm extremely happy with it.

In fact, my client is so happy with my support that he recommended me to a friend of his. His friend's ridiculously old piece of software uses DataFlex 3.1c files as it's tables. They're simply stored as a bunch of .dat files in a folder.

I've found enough documentation to pretty much nail down the file format. The CONNECT engine recognizes them just fine as DOS or BIN types files. The one and only problem is that MariaDB expects the first record to begin at the beginning of the file, but the .dat files contain a header portion that describes the table itself. I can read that and calculate the correct CREATE TABLE statement just fine, but I can't seem to find a way to compensate for the header being where MariaDB expects records to be.

How can I tell MariaDB where the first record actually begins?

I've actually written a functional parser for the .dat files in Python based of of the notes here [here](https://hwiegman.home.xs4all.nl/fileformats/dat/DATAFLEX.txt) and [here](https://code.activestate.com/lists/perl-dbi-dev/1529/).

I also used [this](https://github.com/tforsberg/DataFlexToSQLite) GitHub project as a guide / template (being as I didn't want to convert the DataFlex files, just read and write to them in place).

I was advised by @montywi on the Freenode IRC channel that this was the best place for my question. I will happily supply reference .dat files and support their inclusion in CONNECT however I can. I'm just at a total loss for how to proceed.
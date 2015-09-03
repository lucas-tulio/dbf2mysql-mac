# dbf2mysql-mac

Some fixes to make `dbf2mysql` compilable with Mac OS X. Based on this project: <https://github.com/sonicse/dbf2mysql>

### Build instructions

1. Open `Makefile`

1. If your system doesn't find the `mysql_config` program, you may have to explicitly say where to find your MySQL installation. Replace lines 24 and 25 with your MySQL installation path, followed by `/include/mysql` and `/lib` respectively. For example, if you installed MySQL via `homebrew`, it should look something like this:

   ```
   MYSQLINC=-I/Users/username/homebrew/Cellar/mysql/5.6.17_1/include/mysql
   MYSQLLIB=-L/Users/username/homebrew/Cellar/mysql/5.6.17_1/lib
   ```

1. Run `make`

### Original README file

dbf2mysql v1.14

- Patched and enchanted to mysql by Michael Widenius
- Patched and enchanted to mysql by Alexander Anikeev

OVERVIEW:

This package now consists of two programs: the old `dbf2mysql`, and the new, alpha-code `mysql2dbf`, which makes it possible to dump an mySQL-table to a dbf-file.

dbf2msql:

This program takes an xBase-file and sends queries to an mySQL-server to insert it into an mysql-table. It takes a number of arguments to set its behaviour:

    -v  verbose:
        Produce some status-output
        Source file, Destination schema and table
        Record counts in DBF and insertions to MySQL

    -vv more verbose:
        Fields and substitutions

    -vvv even more verbose:
        Also produce progress-report.

    -vvvv way too verbose!
        Show every insert statement as it is being executed.

    -f  field-lowercase:
        translate all field-names in the xbase-file to lowercase

    -u  uppercase:
        Translate all text in the xbase-file to uppercase

    -l  lowercase:
        Translate all text in the xbase-file to lowercase

    -n  allow NULL fields:
        'NOT NULL' will be not added in table creation statement.

    -o  list fields to insert into mySQL database. Primary use is to make
        easier import of complex dbf files into mySQL where we want only few
        fields.
        NOTE: -o is processed before substitution (-s). So you have to use
        dbf field names here.

    -e  conversion file:
        Specify file for char fields conversion. File format is:
        1st line: number of characters to convert (number of lines).
        Further lines: \<char_to_convert\> \<char_after_conversion\>.

    -s  substitute:
        Takes a list of fieldname/newfieldname pairs. Primary use is to avoid
        conflicts between fieldnames and mySQL reserved keywords. When the new
        fieldname is empty, the field is skipped in both the CREATE-clause and
        the INSERT-clauses, in common words: it will not be present in the
        mySQL-table

            example:
                    -s ORDER=HORDER,REMARKS=,STAT1=STATUS1

    -i  list fields to be indexed. mySQL field names should be listed here.

    -d  database:
        Select the database to insert into. Default is 'test'

    -t  table:
        Select the table to insert into. Default is 'test'

    -c  create:
        Create table. If the table already exists, drop it first.
        The default is to insert all data into the named table, if -cc is specified then no records will be inserted.
        So -cc is "create only", -c is "create and insert", otherwise "insert only".

    -p  primary:
        Select the primary key. You have to give the exact field-name.

    -h  host:
        Select the host where to insert into. Untested.

    -F  Fixed length records (Else chars is saved as varchar)

    -n  Allow NULL values in fields

    -q  "quick" mode. Inserts data via temporary file using 'LOAD DATA INFILE'
        mySQL statement. This increased insertion speed on my PC 2-2.5 times.
        Also note that during whole 'LOAD DATA' affected table is locked.

    -r  Trim trailing and leading whitspaces from CHAR type fields data

    -C  character set:
        Set charset for mysql connection. Tables will created with specified
        charset. All character fields will filled with specified charset.

Rudimentary read-only support for Visual FoxPro, DB III+, and DB IV memo fields/files has been added.

mysql2dbf:

This takes basically the same arguments as dbf2mysql, only \[-p] has changed.

    -v  verbose

    -vv more verbose

    -u  translate all field-contents to uppercase

    -l  translate all field-contents to lowercase

    -d  select database from where to read

    -t  select table from where to read

    -q  specifies custom-query to use

    -p  specify precision to use in case of REAL-fields

THIS IS ALPHA-CODE!!!!!!!!!!

**INSTALL:**

To build it, take a look at the Makefile. You might have to change a few things.

First of all, there's the compiler you use. Then you have to tell where your `install` program is. I use the syntax as in Solaris `/usr/sbin/install`, and I don't know if this is the one with the most standard arguments, so please bear with me if you have to fiddle with it.

Then, tell what compiler-options you want (probably optimizing :). The next step is important: tell where your Minerva/mSQL is installed, since this needs include-files and libraries from there.

Next, tell where you want the binary to be installed (directory-name).

Some operating-systems require extra libraries to be linked against, Solaris for example needs `-lsocket` and `-lnsl` to compile this cleanly. Specify this in `EXTRALIBS`.

Then you're probably set to go. There are 3 more options you can change, but you only need them if you want to do a `make dist`. These are '`RM`', '`GZIP`' and '`TAR`'. Of these, probably the only one that _could_ give trouble is gzip, but you probably already have this, else you wouldn't have succeeded in reading this document :).

**COPYRIGHT:**

Use this piece of software as you want, modify it to suit your needs, but please leave my name in place ok? :)

**DISCLAIMER:**

I do not accept any responsibility for possible damage you get as result of using this program.

**KNOWN BUGS:**

-   It can't write memo-files at this time.

**OTHER BUGS:**

Possibly incorrect field-lengths for REAL-numbers.

**CHANGES**

mysql2dbf v1.14  07-Jul-2000

- William Volkman \<william_volkman at netshark dot com>
  - Updated to work with Visual FoxPro dbf files.
  - Capability to read memo files/fields for Visual FoxPro, DB III+, and DB IV has been implemented (Only tested with VFP V6.0 and Paradox 9.0 generated files, YMMV).
  - Minor bug and memory leak fixes.  Tweaked documentation.

Useful reference is Erik Bachmann's xbase page (you can find it at <http://www.e-bachmann/docs/xbase.htm>)

mysql2dbf v1.13

- Bob Schulze (<bob@yipp.de>)
  - added Date field handling to `mysql2dbf`.
  - added `-P` (passwd) and `-U` (User) options to `mysql2dbf` and `dfb2mysql`

dbf2mysql v1.12

- Patch by Gerald Clark <gerald_clark@suppliersystems.com>:
  - Change of `LOAD DATA INFILE` to `LOAD DATA LOCAL INFLE`
  - Fix of memory allocation bug

dbf2mysql v1.11

- Patch by Gerald Clark <gerald_clark@suppliersystems.com>:
  - It adds a `-x` option to start each table with `_rec` and `_timestamp` fields, adds date fields, and converts boolean to enum fields. It also fixes a problem with quick mode inserts for blank numeric fields.

dbf2mysql-1.10d: (1997.10.30)

-  Fixed insertion of characters from 2nd part of ASCII table (char codes \>127). Don't know if that's correct. Will try to find correct solution.

dbf2mysql-1.10c: (1997.10.20)

-  Corrected README about available command line options.
-  Expanded `-c` behavior. If more than one `c` is specified (`-cc`) then no data will be inserted. Only table will be created.

dbf2mysql-1.10b: (1997.06.18)

-  Still fixing bugs left after porting `dbf2sql` to mySQL. This time bug which could screw program's execution after create-clause.
-  Fixed quite fatal bug in `check_table` which could prevent existing table name detection.
   Mindaugas Riauba <minde@lvkb.lt>

dbf2mysql-1.10: (1997.06.13)

-   Added `-q` flag to use "quick" insertion mode via temporary file and `'LOAD DATA INFILE'` statement. This increased speed 2-2.5 times on my PC running Linux 2.0.29.
    Mindaugas Riauba <minde@lvkb.lt>

dbf2mysql-1.06: (1997.05.02)

-   Added `-e` flag to specify file for char fields conversion. I'm using this to convert strings from one code page to other one.
-   Found, enabled and documented `-n` flag, which allows `NULL` fields.
-   Fixed bug which prevented data insertion without table creation. You reviewed and tested this code or not Michael? ;)
    Mindaugas Riauba <minde@pub.osf.lt>

dbf2mysql-1.05: (1997.04.23)

-   Added `-o` flag to list fields to be processed. That was made to ease import of complex `.dbf` files where we want only few fields. Read note about relationship with `-s`.
-   Someone forgot to include `F` command line option to getopt ;).
-   Adjusted Makefile to Linux and new mySQL libraries (`libmysqlclient` instead of `libmysql`, etc.).
-   Added `-i` flag to specify fields to index.
-   Added `-r` flag t trim leading spaces from strings.
    Mindaugas Riauba <minde@pub.osf.lt>

dbf2msql-1.04:

-   Fixed bug introduced in version 1.03 that calculated the header-length (it was only correct by incident, when I added a pointer to a struct this broke).
-   Added a check when reading in the field-descriptions for end-of-header. Could fix some problems. Patch from Aaron Kardell <akardell@aksoft.com>.
-   Made numeric fields 10 long (4^32 has 10 chars....)

dbf2msql-1.03 (never publicly released):

-   Changed `dbf.c` to use a standard buffer to read the record in, as opposed to allocating one everytime we call `dbf_get_record()`. This will save time in reading and writing records. With `dbf2msql` and `msql2dbf` you won't notice much difference, cos the most time-consuming action is the communication with _msqld_, however, when you use these routines for something else it should make a difference.

dbf2msql-1.02b:

-   Fixed a typo in `msql2dbf.c`
-   Forgot to mention in the `README` of 1.02 that I also fixed a memory-leak, and I mean a major one..... (in `dbf_get_record()`)
-   set `*query = NULL` in `main()` in `msql2dbf.c`. OSF on Alpha's chokes if you don't do this
-   changed `strtoupper()` and `strtolower()`, explicitly make it clear to the compiler *when* to increase the pointer.

dbf2msql-1.02:

-   Added a patch from Jeffrey Y. Sue <jysue@aloha.net> to 'rename' fieldnames.
    This also makes it possible to skip fields altogether
-   Added some patches from Frank Koormann <fkoorman@usf.Uni-Osanbrueck.DE> to initialize the different data-area's to their correct values, to set the dbf-time correctly and to use the correct format for real-values in `dbf_put_record()`

dbf2msql-1.01:

-   Changed every occurence of `FILE` to `file_no`. `FILE` reportedly caused trouble on some systems.
-   Changed `dbf2msql.c` so that when inserting an empty INT-field, an NULL text is inserted in the INSERT-clause instead of nothing.
    Suggestion by Jeffrey Y. Sue (<jysue@aloha.net>).
-   Same for REAL-fields (comes automagically with the change above).
-   When an error occurs during an INSERT-query, the values of that query are displayed when `-vv` is set.
    Suggestion by Jeffrey Y. Sue (<jysue@aloha.net>).
-   Added alpha-support for writing dbf-files from msql-tables by means of the program `msql2dbf`

dbf2msql-0.5:

-   Added the `-f` flag to translate fieldnames to lowercase.
    Suggestion by David Perry (<deperry@nerosworld.com>).

dbf2msql-0.4:

-   fixed a little offset-bug. I came across a file that had an extra char inserted after the header. However, the headerlength-field was correct, so I changed the program to use this instead of calculating the headerlength ourself. Should have done this in the first place.
    Bugreport from David Perry (<deperry@nerosworld.com>).

dbf2msql-0.3:

-   moved call to `do_create()` to inside the check if to create the table or import it into an existing one

dbf2msql-0.2:

-   Reorganized the code, cleaned up `main()`, moved building and sending of 'create-clause' to `do_create()` and moved building and sending of 'insert-clauses' to `do_inserts`.
-   Changed allocation of `(char *)query` in `do_create()` from static to dynamic, thus avoiding overruns.
-   Changed allocation of `(char *)query`, `(char *)keys` and `(char *)vals` in `do_inserts()` from static to dynamic, thus preventing overruns.
-   Fixed a nasty little bug in freeing allocated memory in `dbf_free_record()`. This was stupid, and only showed up *after* the code-reorganization. Don't know why I didn't notice it before.
    If you had *very* large dbases (with large fields), this could fill up your memory completely.
-   Added the `-l` option on suggestion from David Perry (<deperry@zeus.nerosworld.com>)
-   Added the `-c` option (ditto)
-   Added the `-vv` option
-   Enhanced documentation (this little doc)

dbf2msql-0.1:

-   Initial release

Maarten Boekhold (<boekhold@cindy.et.tudelft.nl>)

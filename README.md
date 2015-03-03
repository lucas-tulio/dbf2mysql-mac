# dbf2mysql-mac

Some fixes to make dbf2mysql compilable with Mac OS X.

### Build instructions

1. Open `Makefile`

1. Replace lines 24 and 25 with your MySQL installation path, followed by `/include/mysql` and `/lib` respectively. For example, if you installed MySQL via homebrew, it should look something like this:

    ```
    MYSQLINC=-I/Users/username/homebrew/Cellar/mysql/5.6.17_1/include/mysql
    MYSQLLIB=-L/Users/username/homebrew/Cellar/mysql/5.6.17_1/lib
    ```

1. Run `make`

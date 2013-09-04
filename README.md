# goracle #
*goracle/oracle* is a package is a translated version of
[cx_Oracle](http://cx-oracle.sourceforge.net/html/index.html)
(by Anthony Tuininga) converted from C (Python module) to Go.

[goracle/godrv](godrv/driver.go) is a package which is a
[database/sql/driver.Driver](http://golang.org/pkg/database/sql/driver/#Driver)
compliant wrapper for goracle/oracle - passes github.com/bradfitz/go-sql-test
(as github.com/tgulacsi/go-sql-test).

## There ##
CHAR, VARCHAR2, NUMBER, DATETIME, INTERVAL simple AND array bind/define.
CURSOR, CLOB, BLOB

## Not working ##
Nothing I know of.

## Not working ATM ##
Nothing I know of.

## Not tested (yet) ##
LONG, LONG RAW, BFILE


# Debug #
You can build the test executable (for debugging with gdb, for example) with
go test -c

You can build a tracing version with the "trace" build tag
(go build -tags=trace) that will print out everything before calling OCI
C functions.


# Install #
It is `go get`'able iff you have
[Oracle DB](http://www.oracle.com/technetwork/database/enterprise-edition/index.html) installed
OR the Oracle's
[InstantClient](http://www.oracle.com/technetwork/database/features/instant-client/index-097480.html)
*both* the Basic Client and the SDK (for the header files), too!
- installed

## Linux ##
AND you have set proper environment variables:

    export CGO_CFLAGS=-I$(dirname $(find $ORACLE_HOME -type f -name oci.h))
    export CGO_LDFLAGS=-L$(dirname $(find $ORACLE_HOME -type f -name libclntsh.so\*))
    go get github.com/tgulacsi/goracle

For example, with my [XE](http://www.oracle.com/technetwork/products/express-edition/downloads/index.html):

    CGO_CFLAGS=-I/u01/app/oracle/product/11.2.0/xe/rdbms/public
    CGO_LDFLAGS=-L/u01/app/oracle/product/11.2.0/xe/lib

With InstantClient:

    CGO_CFLAGS=-I/usr/include/oracle/11.2/client64
    CGO_LDFLAGS=-L/usr/include/oracle/11.2/client64

## Mac OS X ##
For Mac OS X I did the following:

You have to get both the Instant Client Package Basic and the Instant Client Package SDK (for the header files).

Then set the env vars as this (note the SDK here was unpacked into the base directory of the Basic package)

    export CGO_CFLAGS=-I/Users/dfils/src/oracle/instantclient_11_2/sdk/include
    export CGO_LDFLAGS=-L/Users/dfils/src/oracle/instantclient_11_2
    export DYLD_LIBRARY_PATH=/Users/dfils/src/oracle/instantclient_11_2:$DYLD_LIBRARY_PATH

Perhaps this export would work too, but I did not try it.  I understand this is another way to do this

    export DYLD_FALLBACK_LIBRARY_PATH=/Users/dfils/src/oracle/instantclient_11_2

The DYLD vars are needed to run the binary, not to compile it.


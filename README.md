### Download Source and Binaries

The latest AAR binary package information can be [here](https://www.zetetic.net/sqlcipher/open-source), the source can be found [here](https://github.com/sqlcipher/android-database-sqlcipher).
<p><a href="https://maven-badges.herokuapp.com/maven-central/net.zetetic/android-database-sqlcipher"><img src="https://maven-badges.herokuapp.com/maven-central/net.zetetic/android-database-sqlcipher/badge.svg"></a></p>

### Compatibility

SQLCipher for Android runs on Android 4–Android 9, for `armeabi`, `armeabi-v7a`, `x86`, `x86_64`, and `arm64_v8a` architectures.
    
### Contributions

We welcome contributions, to contribute to SQLCipher for Android, a [contributor agreement](https://www.zetetic.net/contributions/) needs to be submitted. All submissions should be based on the `master` branch.

### An Illustrative Terminal Listing

A typical SQLite database in unencrypted, and visually parseable even as encoded text. The following example shows the difference between hexdumps of a standard SQLite database and one implementing SQLCipher.

```
~ sjlombardo$ hexdump -C sqlite.db
00000000 53 51 4c 69 74 65 20 66 6f 72 6d 61 74 20 33 00 |SQLite format 3.|
…
000003c0 65 74 32 74 32 03 43 52 45 41 54 45 20 54 41 42 |et2t2.CREATE TAB|
000003d0 4c 45 20 74 32 28 61 2c 62 29 24 01 06 17 11 11 |LE t2(a,b)$…..|
…
000007e0 20 74 68 65 20 73 68 6f 77 15 01 03 01 2f 01 6f | the show…./.o|
000007f0 6e 65 20 66 6f 72 20 74 68 65 20 6d 6f 6e 65 79 |ne for the money|

~ $ sqlite3 sqlcipher.db
sqlite> PRAGMA KEY=’test123′;
sqlite> CREATE TABLE t1(a,b);
sqlite> INSERT INTO t1(a,b) VALUES (‘one for the money’, ‘two for the show’);
sqlite> .quit

~ $ hexdump -C sqlcipher.db
00000000 84 d1 36 18 eb b5 82 90 c4 70 0d ee 43 cb 61 87 |.?6.?..?p.?C?a.|
00000010 91 42 3c cd 55 24 ab c6 c4 1d c6 67 b4 e3 96 bb |.B?..?|
00000bf0 8e 99 ee 28 23 43 ab a4 97 cd 63 42 8a 8e 7c c6 |..?(#C??.?cB..|?|

~ $ sqlite3 sqlcipher.db
sqlite> SELECT * FROM t1;
Error: file is encrypted or is not a database
```
(example courtesy of SQLCipher)

### Application Integration

We’ve packaged up a very simple SDK for any Android developer to add SQLCipher into their app with the following three steps:

1. Add a [reference](https://search.maven.org/search?q=a:android-database-sqlcipher) to the AAR package: `implementation 'net.zetetic:android-database-sqlcipher:Current.Version.Here@aar'`
2. Update the package import path from `android.database.sqlite.*` to `net.sqlcipher.database.*` in any source files that reference it. The original `android.database.Cursor` may still be used unchanged.
3. Initialize the database library and pass a variable argument to the open database method with a password:

```
// First initilize the native database libraries with the context
SQLiteDatabase.loadLibs(this);

// Request a database connection with passwrod
SQLiteDatabase.openOrCreateDatabase(databasePath.getAbsolutePath(), password, null);
```

An article covering both integration of SQLCipher into an Android application as well as building the source can be found [here](https://www.zetetic.net/sqlcipher/sqlcipher-for-android/).

### Building

In order to build `android-database-sqlcipher` from source you will need both the Android SDK, Gradle, and the Android NDK. We currently recommend using Android NDK version `r15c`, however we plan to update to a newer NDK release when possible. To complete the `make` command, the `ANDROID_NDK_ROOT` environment variable must be defined which should point to your NDK root. Once you have cloned the repo, change directory into the root of the repository and run the following commands:

```
# this only needs to be done once
make init

# to build the source for debug:
make build-debug
# or for a release build:
make build-release
```

### License

The Android support libraries are licensed under Apache 2.0, in line with the Android OS code on which they are based. The SQLCipher code itself is licensed under a BSD-style license from Zetetic LLC. Finally, the original SQLite code itself is in the public domain.

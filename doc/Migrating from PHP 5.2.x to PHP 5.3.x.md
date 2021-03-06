Changes are collected from
- [PHP Manual > Migrating from PHP 5.2.x to PHP 5.3.x](http://php.net/manual/en/migration53.php)

**NOT ALL** changes will be checked, because some can not be check by a program.

Lists below describes which will be check and not.


## Overview
- [x] [Backward Incompatible Changes](http://php.net/manual/en/migration53.incompatible.php)
- [~~Ignore~~] [New features](http://php.net/manual/en/migration53.new-features.php)
- [~~Ignore~~] [Changes made to Windows support](http://php.net/manual/en/migration53.windows.php)
- [x] [Changes in SAPI modules](http://php.net/manual/en/migration53.sapi.php)
- [x] [Deprecated features](http://php.net/manual/en/migration53.deprecated.php)
- [~~Ignore~~] [Undeprecated features](http://php.net/manual/en/migration53.undeprecated.php)
- [~~Ignore~~] [New Parameters](http://php.net/manual/en/migration53.parameters.php)
- [x] [New Functions](http://php.net/manual/en/migration53.functions.php)
- [~~Ignore~~] [New stream wrappers](http://php.net/manual/en/migration53.new-stream-wrappers.php)
- [~~Ignore~~] [New stream filters](http://php.net/manual/en/migration53.new-stream-filters.php)
- [~~Ignore~~] [New Class Constants](http://php.net/manual/en/migration53.class-constants.php)
- [x] [New Methods](http://php.net/manual/en/migration53.methods.php)
- [~~Ignore~~] [New Extensions](http://php.net/manual/en/migration53.new-extensions.php)
- [~~Ignore~~] [Removed Extensions](http://php.net/manual/en/migration53.removed-extensions.php)
- [x] [Other changes to extensions](http://php.net/manual/en/migration53.extensions-other.php)
- [x] [New Classes](http://php.net/manual/en/migration53.classes.php)
- [x] [New Global Constants](http://php.net/manual/en/migration53.global-constants.php)
- [~~Ignore~~] [Changes to INI file handling](http://php.net/manual/en/migration53.ini.php)
- [~~Ignore~~] [Other changes](http://php.net/manual/en/migration53.other.php)


## Backward Incompatible Changes [link](http://php.net/manual/en/migration53.incompatible.php)

Although most existing PHP 5 code should work without changes, please take note
of some backward incompatible changes:

#### {~~Ignore~~} The newer internal parameter parsing API has been applied across all the extensions bundled with PHP 5.3.x

This parameter parsing API causes functions to return `NULL` when passed
incompatible parameters. There are some exceptions to this rule, such as the
[get_class()](http://php.net/manual/en/function.get-class.php)
function, which will continue to return `FALSE` on error.

#### {**Done**} [clearstatcache()](http://php.net/manual/en/function.clearstatcache.php) no longer clears the realpath cache by default

#### {**Done**} [realpath()](http://php.net/manual/en/function.realpath.php) is now fully platform-independent

Consequence of this is that invalid relative paths such as *__FILE__ .
"/../x"* do not work anymore.

#### {*Todo*} The [call_user_func()](http://php.net/manual/en/function.call-user-func.php) family of functions now propagate *$this*

even if the callee is a parent class.

#### {**Done**} Some array functions no longer accept objects passed as arguments

- [natsort()](http://php.net/manual/en/function.natsort.php)
- [natcasesort()](http://php.net/manual/en/function.natcasesort.php)
- [usort()](http://php.net/manual/en/function.usort.php)
- [uasort()](http://php.net/manual/en/function.uasort.php)
- [uksort()](http://php.net/manual/en/function.uksort.php)
- [array_flip()](http://php.net/manual/en/function.array-flip.php)
- [array_unique()](http://php.net/manual/en/function.array-unique.php)

To apply these functions to an object, cast the object to an array first.

#### {**Done**} The behaviour of functions with by-reference parameters called by value has changed

Where previously the function would accept the by-value argument, a fatal error
is now emitted. Any previous code passing constants or literals to functions
expecting references, will need altering to assign the value to a variable
before calling the function.

#### {~~Ignore~~} The new mysqlnd library necessitates the use of MySQL 4.1's newer 41-byte password format

Continued use of the old 16-byte passwords will cause
[mysql_connect()](http://php.net/manual/en/function.mysql-connect.php) and
similar functions to emit the error, "mysqlnd cannot connect to MySQL 4.1+
using old authentication."

#### {~~Ignore~~} The new mysqlnd library does not read mysql configuration files

(my.cnf/my.ini), as the older libmysqlclient library does.  If your code relies
on settings in the configuration file, you can load it explicitly with the
[mysqli_options()](http://php.net/manual/en/mysqli.options.php) function. Note
that this means the PDO specific constants `PDO::MYSQL_ATTR_READ_DEFAULT_FILE`
and `PDO::MYSQL_ATTR_READ_DEFAULT_GROUP` are not defined if MySQL support in
PDO is compiled with mysqlnd.

#### {*Todo*} The trailing / has been removed from the [SplFileInfo](http://php.net/manual/en/class.splfileinfo.php) class and other related directory classes

#### {**Done**} The [__toString()](http://php.net/manual/en/language.oop5.magic.php#object.tostring) magic method can no longer accept arguments

#### {**Done**} The magic methods must always be public and can no longer be static

Method signatures are now enforced.

- [__get()](http://php.net/manual/en/language.oop5.overloading.php#object.get)
- [__set()](http://php.net/manual/en/language.oop5.overloading.php#object.set)
- [__isset()](http://php.net/manual/en/language.oop5.overloading.php#object.isset)
- [__unset()](http://php.net/manual/en/language.oop5.overloading.php#object.unset)
- [__call()](http://php.net/manual/en/language.oop5.overloading.php#object.call)

#### {**Done**} The [__call()](http://php.net/manual/en/language.oop5.overloading.php#object.call) magic method is now invoked on access to private and protected methods

#### {**Done**} [func_get_arg()](http://php.net/manual/en/function.func-get-arg.php), [func_get_args()](http://php.net/manual/en/function.func-get-args.php) and [func_num_args()](http://php.net/manual/en/function.func-num-args.php)

can no longer be called from the outermost scope of a file that has been
included by calling [include](http://php.net/manual/en/function.include.php) or
[require](http://php.net/manual/en/function.require.php) from within a function
in the calling file.

#### {*Todo*} s2k hashing is no longer available

An emulation layer for the MHASH extension to wrap around the Hash extension
have been added. However not all the algorithms are covered, notable the s2k
hashing algorithm. This means that s2k hashing is no longer available as of PHP
5.3.0.

#### {**Done**} The following keywords are now [reserved](http://php.net/manual/en/reserved.php)

and may not be used as names by functions, classes, etc.

- [goto](http://php.net/manual/en/control-structures.goto.php)
- [namespace](http://php.net/manual/en/language.namespaces.php)


## Changes in SAPI modules [link](http://php.net/manual/en/migration53.sapi.php)

#### {**Done**} The dl() function is now disabled by default

and is now available only under the CLI, CGI, and embed SAPIs.


## Deprecated features [link](http://php.net/manual/en/migration53.deprecated.php)

#### {~~Ignore~~} INI directives

#### {**Done**} Functions

#### {**Done**} Assigning the return value of new by reference is now deprecated

#### {**Done**} Call-time pass-by-reference is now deprecated


## Other changes to extensions [link](http://php.net/manual/en/migration53.extensions-other.php)

#### {**Done**} Image Processing and GD gd_info()

The "JPG Support" index returned from gd_info() has been renamed to "JPEG Support".

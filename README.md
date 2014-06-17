tad: Tag Attribute Database
===========================

Intro
-----

tad is a simple utility for associating hostnames (or other strings) with
tags and (item,tag) tuples with attributes.  It is primarily intended for
keeping track of which hosts are assigned to which roles, in which data 
centers, and environments (webservers, in production in the eastern U.S. 
DC vs. app servers in QA in the AWS us-west-1, for example).  

tad's primary using is to expand a tag or tag expression into a list of
matching hosts.  Basic set operations (union, difference, intersection)
and filtering by glob or regular expressions) are supported in arbitrarily
complex expressions using a terse, but relatively simple syntax.

These can be used in shell command substition expressions to execute
commands to large numbers of hosts using any parallel ssh or other
orchestration framework (such as SaltStack, or Ansible).  You can also
redirect the output into simple text files or generate hosts files
suitable for these utilities and tools to use. Also ReST-ful and
Python APIs are provided and the backend is any DBAPI compatible SQL
database (including SQLite3, MySQL and PostgreSQL, of course).  The
schema is kept intentionally simple to facilitate easy implementation
of API adapters for Ruby, PHP, Javascript, Go or other languages.

tad allows freeform tagging, making it easy to add and remove tags from
hosts, lists of hosts, and hosts matching or exclusive of matching
regular expression or globbing patterns.  These operations are fast and
cheap enough to allow tagging all hosts (optionally with an existing
tag expression) and interative removal of the "TODO" tag as each host
is upgraded or otherwise handled.

Installation and Configuration
------------------------------

To install tad simply clone its git repository with:

```
      git clone http://github.com/JimDennis/tad
```

... and run the installation script (PIP and distutils instuction
to be added later).

To configure tad run:

```
    tad --configure
```

... and provide a database DSN (data source name).  This defaults
to a local SQLite3 database file stored under ~/.tad/db (but can be
over-ridden in ~/.tad/config; and the configuration directory can
be over-ridden either in /etc/tad/ or via the TADCONFIG environment
variable).

(tad will look for `$TADCONFIG` first, `$HOME/.tad` next, then 
`/etc/tad/` and `/etc/tadrc` last).

Once you've configured tad then you can initialize a new database
using: 

```
    tad --initialize
```

... and start using it.

From there you add hostnames, tag them, and, optionally add
attributes to your tags and, as necessary, over-ride attributes
for any hostname,tag tuple.  (For example you might tag a number
of hosts as `REDIS` and associate a REDIS_PORT``  with an 
attribute of 6789 (the Redis default) ... but over-ride that
with a different port for just those Redis servers in your
QA environment).


You can initially use tad just for your own database and access
exclusively through the command line utility.  However, you'll
eventually want to run it as a centralized service (accessed
by clients ReST-fully --- over HTTP).  The client can find its
server(s) via the config file or via the `$TADURL` variable.

When run as a service tad can run standalone (as a Flask
application) or hosted under any WSGI compatible web server.



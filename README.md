# command-not-found fix
Fixed version of linux command-not-found package to work with lz4 compression

# The Problem
The debian/ubuntu package manager `apt` has an option to download compressed package lists to use less bandwidth and storage using the option `Acquire::GzipIndexes "true"`.
But `command-not-found ` package is outdated and does not support compression and expects the lists files to be be plain text.
The package is no longer maintained so I've created a simple solution to this  error.

When you try to make **`apt update`** with `Acquire::GzipIndexes "true"` option enabled, you will get error like this 

```python
Traceback (most recent call last):
  File "/usr/lib/cnf-update-db", line 26, in <module>
    col.create(db)
  File "/usr/lib/python3/dist-packages/CommandNotFound/db/creator.py", line 94, in create
    self._fill_commands(con)
  File "/usr/lib/python3/dist-packages/CommandNotFound/db/creator.py", line 138, in _fill_commands
    self._parse_single_commands_file(con, fp)
  File "/usr/lib/python3/dist-packages/CommandNotFound/db/creator.py", line 176, in _parse_single_commands_file
    suite=tagf.section["suite"]
KeyError: 'suite'
Reading package lists... Done
E: Problem executing scripts APT::Update::Post-Invoke-Success 'if /usr/bin/test -w /var/lib/command-not-found/ -a -e /usr/lib/cnf-update-db; then /usr/lib/cnf-update-db > /dev/null; fi'
E: Sub-process returned an error code
```

The solution is that is the file is compressed we just decpress it into a tmp file and use it


# More Info
* https://askubuntu.com/questions/1308270/acquiregzipindexes-causes-apt-to-fail
* https://bugs.launchpad.net/ubuntu/+source/command-not-found/+bug/1876034

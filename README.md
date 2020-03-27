SlicerDataStore
===============

This project organizes Slicer sample data, atlases and registration case library images.

To support content-based addressing, files are uploaded as assets organized in [releases](https://github.com/Slicer/SlicerDataStore/releases)
named after the hashing algorithm.

For each hashing algorithm `<HASHALGO>` (MD5, SHA256, ...), you will find:
* release named `<HASHALGO>`
* a CSV file `<HASHALGO>.csv` listing all `<hashsum>;<filename>` pairs
* markdown document `<HASHALGO>.md` with links of the form `* [<filename>](https://github.com/Slicer/SlicerDataStore/releases/download/<HASHALGO>/<checksum>)` (this file is regenerated on each upload from the CSV file, therefore files should be renamed or deleted by editing the CSV file)

_The commands reported below should be evaluated in the same terminal session. See [Documentation conventions](#documentation-conventions)_

Upload files
------------

1. Install [Prerequisites](#prerequisites)

2. Copy files to upload in `INCOMING` directory.

3. Run process_release_data.py script specifying your github token and upload command.

```
$ python process_release_data.py upload --github-token 123123...123
```

4. Optional: Clear content of `INCOMING` directory if all files have been uploaded for each `<HASHALGO>`.

Download files
--------------

1. Install [Prerequisites](#prerequisites)

2. Run process_release_data.py script specifying your github token, download command, and hash algorithm. If multiple versions of the same filename are available then the unique file names will be generated in the download folder by attaching the checksum to the end of each original filename.

```
$ python process_release_data.py download --github-token 123123...123
```

Rename a file
-------------

Download `<HASHALGO>.csv` file from the corresponding release, edit it, upload the modified version, and run `process_release_data upload`.

Note: Since each file is identified by hash sum, changing the filename does not affect the ability to find and download files. Filenames are only stored in the repository to allow generation of user-friendly file names.

Delete a file
-------------

Download `<HASHALGO>.csv` file from the corresponding release, remove the line referring to the file that should be deleted, upload the modified version, and run `process_release_data upload`. Remove the referred file from the release assets.

Note: Deleting files should be avoided (even if an old version of a file is replaced by a new one), because tests in earlier software versions may still expect to find previously uploaded files. Deleting is only recommended for immediate removal files that have just been uploaded by mistake.

Prerequisites
-------------

1. Download this project

```
$ git clone git://github.com/Slicer/SlicerDataStore
```

2. Install Python and [githubrelease](https://github.com/j0057/github-release#installing) package

```
$ pip install githubrelease
```

3. [Create a github personal access token](https://help.github.com/articles/creating-an-access-token-for-command-line-use) with "repo" scope to get read/write access to the repository


Documentation conventions
-------------------------

Commands to evaluate starts with a dollar sign. For example:

```
$ echo "Hello"
Hello
```

means that `echo "Hello"` should be copied and evaluated in the terminal.


Maintenance
-----------

Updates to [README.md](README.md) and [process_release_data.py](process_release_data.py) should first
be contributed to [Slicer/SlicerTestingData][slicertestingdata] and then backported to this repository
using a commit message similar to this one:

```
ENH: Update README and process_release_data.py based on Slicer/SlicerTestingData@abcdefghi

List of changes:

$ git shortlog 123456789..abcdefghi --no-merges README.md process_release_data.py

FirstName LastName (N):
    This is title of 1st commit
    This is title of 2nd commit
    ...
    This is title of Nth commit
```


[slicertestingdata]: https://github.com/Slicer/SlicerTestingData


History
-------

In 2013, the Slicer module called "Data Store" was created. It allowed to easily
upload, download and browse [MRB][mrb] files stored in a remote [MIDAS][midas] database that was
available at `https://slicer.kitware.com/midas3/folder/1555`. Source code of the module
was stored in the [Slicer/Slicer-DataStore][slicer-datastore-module] GitHub project.

In March 2020, the Slicer community decided (see issue [Slicer/Slicer#4602][slicer-issue-4602])
to retire the DataStore module and instead store the corresponding data files as GitHub release
assets into the [Slicer/SlicerDataStore][slicerdatastore] GitHub project.

[mrb]: https://www.slicer.org/wiki/Documentation/4.10/SlicerApplication/SupportedDataFormat
[MIDAS]: https://github.com/midasplatform/midas
[slicer-datastore-module]: https://github.com/Slicer/Slicer-DataStore
[slicer-issue-4602]: https://github.com/Slicer/Slicer/issues/4602
[slicerdatastore]: https://github.com/Slicer/SlicerDataStore

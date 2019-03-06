## EFUtool - Fast EFU indexer tool for Everything

EFUtool creates and updates EFU file indexes for loading into [Everything](https://www.voidtools.com) search tool. The main addition of EFUtool is a fast "update mode" which rescans a given volume/share for changes only, instead of doing a full scan everytime. This update mode is about 7x faster than the native EFU indexer in Everything, and greatly reduces the I/O load on the storage units.

<br>

**Benchmarks on a group of shares with about 60 TB in 12 million files, 500 thousand folders**

Tool | Operation | Runtime | Comment
:--- |:--- | --- |:---
Everything UI | Folder Index Rescan | 15:48:04 |
Everything CLI | Create EFU index | 03:40:06 | 4x faster than Folder Index
EFUtool | Create EFU index | 03:41:21 | similar to Everything CLI
EFUtool | Update EFU index | 00:29:45 | **32x faster** than Everything GUI, **7x faster** than Everything CLI

<br>

EFUtool can also take include/exclude filters to fine tune what is included in the index. This further allows removal of files/folders that do not need to be indexed, making the indexes smaller and faster to lookup by Everything.

For volumes/shares with millions of files the Folder Indexing is much slower than EFU indexing ([see this thread](https://www.voidtools.com/forum/viewtopic.php?f=6&t=7545)). This issue may be resolved in a future version, which will bring Folder Indexing speed to the same level as EFU creation. EFUtool update-mode is still much faster, and thus worthwhile.

<br>

**Download (x64 binary):** [Latest Release](https://github.com/zybexXL/EFUtool/releases/latest)

<br>

**Command line syntax**

`EFUtool <[path\]index.EFU> [<root1> ... <rootN>] [options]`

<br>

**Options**
```
    -i <mask> : include files/dir mask
    -x <mask> : exclude files/dir mask
    -f        : filter EFU file (no folder update/scan)
    -s        : print EFU file statistics/info
    -p        : suppress progress indication (for logging to file)
```

  - Multiple -i and -x switches can be used
  - mask pattern can include * and ? for regular filemask syntax
  - mask pattern can start with 'regex:' to use c# style regex matching
  - -i and -x can also be used in statististics and filter modes

<br>

**Examples**

Create a new EFU file with index of RootPath1 and RootPath2:

`EFUtool index.efu RootPath1 RootPath2`

<br>

Update an existing EFU file - rescan all included folders:

`EFUtool index.efu`

Update an existing EFU file - rescan only RootPath2, exclude EXE files:

`EFUtool index.efu RootPath2 -x *.exe`

Update an existing EFU file - add JPG files from RootPath3:

`EFUtool index.efu RootPath3 -i *.jpg`

<br>

Filter out RootPath2\ from EFU file:

`EFUtool index.efu -f -x RootPath2\`

Filter out all *.tmp files and TEMP folders from EFU file:

`EFUtool index.efu -f -x *.tmp -x \TEMP\`

Filter out all except *.jpg files of RootPath1 from EFU file:

`EFUtool index.efu -f -i RootPath1\*.jpg`

<br>

Print statistics for EFU file:

`EFUtool index.efu -s`

Print statistics for *.tmp files on EFU file:

```EFUtool index.efu -s -i *.tmp```

Print statistics for RootPath1 except *.tmp files:

```EFUtool index.efu -s RootPath1 -x *.tmp```

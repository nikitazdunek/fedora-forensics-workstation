# Case 001: Canon SD card (public test image)

## Summary

Examined a public test image of a 32 MB SD card from a Canon PowerShot
SD800 IS camera to identify live, deleted and recoverable photos. Found
33 live JPEGs, 3 deleted JPEGs with intact directory entries, and 5 files
carved from unallocated space.

## Source

| Item | Detail |
| --- | --- |
| Image | nps-2009-canon2-gen6.E01 |
| Origin | Digital Corpora, public research dataset |
| Downloaded | 2026-10-02 10:16:13 UTC |
| Format | EnCase 6 (E01), 31129600 bytes |
| SHA256 of file | 10483722d84e0cefcb693b11dea2d32dbd3ad2f06f8c9656688c8c730fe41579 |
| Stored MD5 | 750b509d8fbed37a5213480aaccfdc61, verified with ewfverify |

## Tools

Autopsy 4.22.1, The Sleuth Kit 4.15.0, ewftools 20140608, on Fedora 44.

## Evidence log

| # | Time (UTC) | Action | Tool | Result |
| --- | --- | --- | --- | --- |
| 1 | 10:16 | Downloaded image and narrative | curl | 29.7 MB E01 |
| 2 | 10:16 | Hashed image | sha256sum, md5sum | SHA256 recorded above |
| 3 | 10:16 | Verified stored hash | ewfverify | SUCCESS |
| 4 | 10:23 | Read partition table | mmls | One partition at sector 51 |
| 5 | 10:23 | Read file system details | fsstat | FAT12, label CANON_DC |
| 6 | 10:23 | Listed files including deleted | fls | 3 deleted JPEGs |
| 7 | 10:45 | Created case, ran ingest | Autopsy | Data Source Integrity passed |
| 8 | 10:55 | Reviewed live, deleted and carved files | Autopsy | See findings |
| 9 | 11:15 | Reviewed EXIF metadata | Autopsy | 37 results |
| 10 | 11:29 | Hashed image after analysis | sha256sum | MATCH, image unchanged |

## Findings

1. **File system:** the partition table labels the partition FAT16
   (type 0x04), but the boot sector shows the file system is FAT12, volume
   label CANON_DC, OEM name PwrShot. The partition type is only a label;
   the boot sector is what defines the file system.
2. **Live files:** 33 JPEGs in `DCIM/100CANON`.
3. **Deleted files:** 3, `_MG_0025.JPG`, `_MG_0030.JPG` and `_MG_0035.JPG`.
   FAT deletion overwrites the first character of the file name, which is
   why they appear with `_MG_` instead of `IMG_`. Their directory entries
   and sizes survive, so they are recoverable.
4. **Carved files:** 5 files recovered from unallocated space by PhotoRec,
   with no names or timestamps because no file system entry remained.
   One is only 3,613 bytes and is likely a thumbnail or fragment.
5. **Metadata:** EXIF shows every photo came from a Canon PowerShot
   SD800 IS. Most were taken on 23 Dec 2008 between 14:12 and 14:30.
   IMG_0044 to IMG_0051 were taken on 24 Dec 2008 between 20:21 and 20:22,
   a separate later session.
6. **Timestamps:** FAT stores local time with no time zone. Autopsy shows
   them as GMT, but the true offset is unknown. Separately, ewfinfo
   displays the acquisition time in the examiner's local time zone, so
   the same image shows different times on different machines. All times
   in this log are recorded in UTC for that reason.

## Verification

Compared my findings with the dataset's published narrative, which lists
51 photos taken over six generations of the card (IMG_0001 to IMG_0051),
each a photo of a screen showing its number.

* The 4 full size carved photos show "14", "G2-3", "G2-4" and "G3-1",
  which the narrative identifies as IMG_0014, IMG_0039, IMG_0040 and
  IMG_0041. All four open as complete images.
* The 3,613 byte carved file shows "1". It is a small preview of
  IMG_0001, not the full photo.
* In total I recovered 40 of the 51 photos as full images: 33 live,
  3 deleted and 4 carved. The other 11, including the full IMG_0001,
  were not recovered. The most likely reason is that later photos
  overwrote their data.
* The narrative notes that some photos are fragmented. Carving rebuilds
  a file from one continuous run of data, so a fragmented photo with no
  file system entry left is very hard to recover this way.

## What I learned

* Deleted is not gone. FAT keeps the directory entry until it is reused,
  and file content survives in unallocated space after that.
* Verify the image twice: once with ewfverify, once inside the tool.
* Record times in UTC and note where a tool is assuming a time zone.

## Screenshots

* [File tree](../screenshots/01-file-tree.png)
* [Deleted files](../screenshots/02-deleted-files.png)
* [EXIF metadata](../screenshots/03-exif.png)
* [Carved files](../screenshots/04-carved-files.png)


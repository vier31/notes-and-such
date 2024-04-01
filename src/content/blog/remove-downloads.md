---
author: Jan Schroeder
pubDatetime: 2024-04-01T00:00:00.00Z
title: How to remove all downloaded files from iCloud folder
description: Bash script to recursively remove files
tags:
  - bash
  - macos
---

I was looking for a solution to this, since choosing the `remove download` optin from the context menu did not work, like at all.

Through [a stackexchange discussion](https://apple.stackexchange.com/questions/429373/how-do-i-remove-icloud-downloads-on-mac-including-all-subfolders-and-files), I found a script from [Rakesh](https://rakhesh.com/mac/macos-remove-icloud-downloaded-files-from-a-folder-and-sub-folders/), that works like a charm.

```bash
cd /the/folder/you/want/to/target
find . -type f -exec brctl evict {} \;
```

So what does this do?

The `cd` takes us to the top folder whose downloaded content we want to remove. So this does not have to be the top iCloud folder, but could be any sub-folder.

Then, `find` recursively collects all the files in the directory and sub-directories. We could be more specific here and add filters for file types or names, but in this case, we just take everything. [This article](https://math2001.github.io/article/bashs-find-command/) seems like a decent starting point for further reading on the `find` command.

`-exec` let's us run a command on the found file. In our case `brctl`, which let's us [work with the iCloud files](https://man.ilayk.com/man/brctl/). `evict` removes the download. The `{}` is a placeholder from `find` and will be replaced with the individual path of each file.

`brctl` could also be used to download the file, so we could flip the script, in order to download all files recursively.

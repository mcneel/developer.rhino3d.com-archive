+++
authors = [ "giulio" ]
categories = [ "Getting Started" ]
keywords = [ "python", "commands" ]
languages = [ "Python" ]
sdk = [ "RhinoPython" ]
title = "Troubleshooting Python Install"
type = "guides"
weight = 5

[admin]
picky_sisters = ""
state = ""

[included_in]
platforms = [ "Windows", "Mac" ]
since = 0

[page_options]
byline = true
toc = true
toc_type = "single"

+++

Over the last few months, we received several reports about issues with independent IronPython installations (IIPI) and Rhino.

## Problem:

Python 2.7.7 can decompress assemblies into the GAC (Global Assembly Cache). IIPI are installations of IronPython made via MSI files, and they appear in the Control Panel like this:

{{< image url="/images/iron-features.png" alt="/images/iron-features.png" class="image_center" width="100%" >}}

It is possible to install IronPython via folder decompression, and that has no impact on Rhino.

We installed both IronPython 2.7.5 final and IronPython 2.7.7 final from the IronPython website, and checked for compatibility with running the _EditPythonScript editor, and importing the os module.

Here are the results:

|                              | | | IronPython 2.7.5 | IronPython 2.7.7 |
|:-----------------------------|-|-|:--------------------:|:------------------------:|
| Rhinoceros 5 SR12 or SR13    | | |          OK           |   OK only with no GAC* |
| Rhinoceros WIP (31 Jan 2017) | | |          OK           |           OK |


*This is what the GAC options looks like, during installation:

{{< image url="/images/gac-assembly.png" alt="/images/gac-assembly.png" class="image_center" width="80%" >}}

## Solution:

If you need version 2.7.7, and need Rhinoceros 5 SR12 and SR13 to run Python on the same system, you have a few options:

1. Reinstall with disabled GAC installation.
1. Uninstall and reinstall by decompressing the IronPython folder.
1. Switch to IronPython version 2.7.5, both with or without GAC.
1. Alternatively, uninstall IronPython.

---

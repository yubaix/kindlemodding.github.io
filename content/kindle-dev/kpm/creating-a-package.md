---
layout: default
title: Creating a Package
weight: 0
---

# Creating a Package
A KPM package can be created using the [`kpm-helper.py`](https://github.com/KindleModding/KPM/blob/main/kpm-helper.py) script:
```sh
# Step 1. Create a folder to store your package contents
mkdir example_package
# Step 2. Initialise the package folder
python kpm-helper.py package init ./example_package
```

The `example_package` folder will now look like this:
```sh
example_package
└── manifest.json

1 directory, 1 file
```

The manifest file will now be initialised to target the `kindleany` platform (see: [`targeting specific platforms`](./targeting-platforms.html))

## Hooks
You can now put any other files in the package folder, however they won't do anything by themselves, this is why we have hooks:

```sh
example_package
├── install.sh
├── launch.sh
├── manifest.json
├── startup.sh
└── uninstall.sh

1 directory, 5 files
```

| File Name    | Purpose                                                 |
|--------------|---------------------------------------------------------|
| install.sh   | Ran during package installation after it is unpacked    |
| uninstall.sh | Ran during package uninstallation after it is unmounted |
| launch.sh    | Ran when package is launched from a launcher            |
| startup.sh   | Ran when the Kindle boots and `hdnext` is initialised   |

<p class="warning">Hooks <b>MUST NOT</b> write to rootfs or remount it as rw, see below for more details</p>

Hooks are always ran from the package's folder whilst the package is in an unpacked state, this means you can reference local files with relative paths.

It is encouraged to use the `install.sh` hook to place a scriptlet in the `/mnt/us/documents` folder when appropriate.  
It is recommended that the scriptlet runs `/var/local/kmc/bin/kpm launch PACKAGENAME` to run the package's `launch.sh`

## Writing To Rootfs
Using the `install.sh` hook to modify the contents of `rootfs` is unsupported. It is recommended that you do not do this for now.  
A solution is planned to be implemented in the near future.
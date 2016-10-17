# Build Rotator Plugin

A Jenkins plugin, which allows to perform rotation of build history without high memory pressure.

Table of contents
---

1. [Overview](#overview)
1. [Building](#building)
1. [Basic Usage](#basic-usage)
1. [Authors](#authors)
1. [License](#license)

Overview
---

The reason we created the plugin was because we found that default rotation in Jenkins costs a lot. In our case 25% percent of memory footprint was related only to job history rotation.
Root cause was in principle behind default implementation: it guarantees that if you want to keep only last X builds in history, it would keep EXACTLY X builds.
The main idea of this plugin is a little bit different: it guarantees that if you want to keep only last X builds in history, it would keep MAXIMUM X builds.

It's better to show difference on basic example.
Let's assume that we have 5 builds in history and we want to keep 5 builds:
- build1
- build2
- build3
- build4
- build5

After that you want to remove build3, because it's broken. So, now you have only:
- build1
- build2
- build4
- build5

And build6 just finished:
- build1
- build2
- build4
- build5
- build6

`Log Rotator` strategy in this case would load in memory entire history and check is there 5 builds in reality or not. And will not delete anything at the end. As result list would be the same:
- build1
- build2
- build4
- build5
- build6


`Build Rotator` is checking only difference between last build number and number of builds it should keep. And if it's more than 5, it would remove first build. So, after that you will have:
- build2
- build4
- build5
- build6

But it's quite important to mention that `Build Rotator` respects all other aspects of rotation in same way as Log Rotator strategy. It doesn't remove:
- build with enabled "keep log" action
- last successful build in history
- last stable build build in history
- ongoing build

Building
---

Prerequisites:

- JDK 7 (or above)
- Apache Maven

Build process is quite simple

```Shell
mvn install
```

Basic Usage
---

To get started:

1. Install the plugin
1. Go to any job -> Configuration
1. Check on "Discard Old Builds"
1. Set "Build Rotator" strategy.

Authors
---

Alexander Akbashev - <alexander.akbashev@here.com>

License
---

Licensed under the [MIT License (MIT)](LICENSE)
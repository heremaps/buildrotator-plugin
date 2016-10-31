# Build Rotator Plugin

A Jenkins plugin which allows to perform rotation of build history with low memory pressure.

Table of contents
---

1. [Overview](#overview)
1. [Building](#building)
1. [Basic Usage](#basic-usage)
1. [Authors](#authors)
1. [License](#license)

Overview
---

The reason we created this plugin is because we found the default rotation in Jenkins to use a lot of memory. In our case 25% percent of memory footprint was related to job history rotation alone.
This is caused by an implementation detail of the default implementation: It guarantees that if you want to keep only last *X* builds in history, it would keep **exactly** *X* builds.
The main idea of this plugin is to make a little bit different guarantee: If you want to keep only last *X* builds in history, it would keep a **maximum** of *X* builds.

Here are some examples to illustrate the difference.

Let us assume that we have 5 builds in history and we want to keep 5 builds:
- build1
- build2
- build3
- build4
- build5

After that you want to remove `build3`, because it is broken. So, now you have only:
- build1
- build2
- build4
- build5

And after `build6` just finished:
- build1
- build2
- build4
- build5
- build6

The `Log Rotator` strategy in this case would load the entire history in memory to check whether there are 5 builds in reality or not, and it would not delete anything at the end. As a result the list would be the same:
- build1
- build2
- build4
- build5
- build6


The `Build Rotator` is checking only the difference between last build number and the number of builds it should keep. And if it's more than 5, it would remove the first build. So, afterwards you would have:
- build2
- build4
- build5
- build6

More importantly, `Build Rotator` respects all other aspects of rotation in same way as the `Log Rotator` strategy. IN particular, it does not remove:
- Builds that have "Keep log" enabled.
- The "Last successful build" in the history.
- The "Last stable build" in the history.
- An ongoing build.

Building
---

Prerequisites:

- JDK 7 (or above)
- Apache Maven

Build process is quite simple:

```Shell
mvn install
```

Basic Usage
---

To get started:

1. Install the plugin.
1. Go to the Jenkins Configuration.
1. Enable on "Discard Old Builds".
1. Set the "Build Rotator" strategy.

Authors
---

Alexander Akbashev - <alexander.akbashev@here.com>

License
---

Licensed under the [MIT License (MIT)](LICENSE).

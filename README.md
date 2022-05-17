# SnoopSnitch

This is the SnoopSnitch source repository. SnoopSnitch collects and analyzes
mobile radio data to make you aware of your mobile network security and to warn
you about threats like fake base stations (IMSI catchers), user tracking and
over-the-air updates.

# License

Copyright (C) 2014 - 2022 Security Research Labs

This program is free software: you can redistribute it and/or modify it under
the terms of the GNU General Public License as published by the Free Software
Foundation, either version 3 of the License, or (at your option) any later
version.

See COPYING for details.

# Resources

* Project website:       https://opensource.srlabs.de/projects/snoopsnitch
* Public Git repository: https://github.com/srlabs/snoopsnitch
* Mailing list:          https://lists.srlabs.de/cgi-bin/mailman/listinfo/gsmmap
* Email:			       snoopsnitch@srlabs.de
                       (PGP: 9728 A7F9 D457 1FBB 746F  5381 D52C AC10 634A 9561)

# Building from source

SnoopSnitch - including helper binaries - is known to build sucessfully on
Linux and OS X. When using the prebuilt helper binaries contained in the
repository, the app may also build on Windows.

To build SnoopSnitch you need the Android SDK [1] for building the actual app
and the Android NDK [2] (version 14.1) to build the native components like the Qualcomm DIAG
wrapper or the GSM parser. Download SDK and NDK and install it somewhere in
your file system. Set the environment variable NDK and SDK to the respective
paths:

```bash
$ export ANDROID_HOME=<your_sdk_dir>
$ export NDK_DIR=<your_ndk_dir>
```

IMPORTANT NOTE: `contrib/prebuilt/` contains binaries to allow e.g. windows users
to build and deploy the SnoopSnitch app easily. However, you can rebuild
everything contained in contrib/prebuilt - including the libasn1c and
libosmocore projects we depend on - from source using the contrib/compile.sh
script. The source of those projects is pulled in by the build scripts through
sub-modules.

As SnoopSnitch ships with the prebuilt binaries, the following step is optional.
To build the parser binary from source, first install:

```bash
$ sudo apt-get install default-jdk
```

In the SnoopSnitch source directory do the following (make sure you are using the version 14.1 of the NDK, as newer version of the NDK build tools are not supported yet):

```bash
$ cd contrib/
$ ./compile.sh -a -g -u
```

To build the app, in the SnoopSnitch source directory, run the gradle wrapper like this:

```bash
$ cd SnoopSnitch
```

If you want to build the Google Play version (which e.g. has a dependency to the Google SafetyNet API and the active mobile network security test feature is disabled due to Google Play sensitive permission policy changes) you need to run

```bash
$ ./gradlew assembleSafetynet
```

If you want to build the F-Droid or website version, which comes without the Google SafetyNet API dependency and comes with the active mobile network security test feature, then you need to run:

```bash
$ ./gradlew assembleNobuildcheck
```

gradlew build will not work anymore, as the sources for our internal testing build flavor *stage* are not included in the public git repository.

The compiled apk files can be found under: `SnoopSnitch/app/build/outputs/apk/`


Please consult the Android documentation on how to set up the tools and
perform a release build.


[1] https://developer.android.com/sdk/

[2] https://developer.android.com/tools/sdk/ndk/

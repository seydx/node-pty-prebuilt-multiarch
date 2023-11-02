# @homebridge/node-pty-prebuilt-multiarch

![Prebuild Binaries](https://github.com/homebridge/node-pty-prebuilt-multiarch/workflows/Build%20and%20Test/badge.svg)

This project is a parallel fork of [`node-pty`](https://github.com/Microsoft/node-pty) providing prebuilt packages for certain Node.js and Electron versions.

Inspired by [daviwil/node-pty-prebuilt](https://github.com/daviwil/node-pty-prebuilt).

## Usage

Thanks to the excellent [`prebuild`](https://github.com/prebuild/prebuild), [`prebuild-install`](https://github.com/prebuild/prebuild) modules, and [`prebuildify`](https://github.com/prebuild/prebuildify) using this module is extremely easy.  You merely have to change your `node-pty` dependency to `@homebridge/node-pty-prebuilt-multiarch` and then change any `require` statements in your code from `require('node-pty')` to `require('@homebridge/node-pty-prebuilt-multiarch')`.

> **NOTE**: We started shipping prebuilds as of node-pty version 0.8.1, no prior versions are provided!  If you were using an earlier version of `node-pty` you will need to update your code to account for any API changes that may have occurred.

## How It Works

We maintain a parallel fork of the `node-pty` codebase that will be updated as new releases are shipped.  When we merge new updates to the code into the `prebuilt-multiarch` branch, new prebuilt packages for our supported Node.js and Electron versions are updated to the corresponding [GitHub release](https://github.com/homebridge/node-pty-prebuilt-multiarch/releases).

When `@homebridge/node-pty-prebuilt-multiarch` is installed as a package dependency, the install script checks to see if there's a prebuilt package on this repo for the OS, ABI version, and architecture of the current process and then downloads it, extracting it into the module path.  If a corresponding prebuilt package is not found, `node-gyp` is invoked to build the package for the current platform.

## Prebuilt Versions

| OS              | Architectures               |
| --------------- |-----------------------------|
| macOS           | x64, arm64                  |
| Linux (glibc)   | ia32, x64, armv6, aarch64   |
| Linux (musl)    | x64, armv6, aarch64         |
| Windows         | ia32, x64                   |

*We only provide prebuilt binaries for Node.js 10 and Electron 5.0.0 or higher.

## Build / Package

Please note releasing this package uses GitHub actions.

This flows takes the branch selected from the workflow start drop down, and creates a GitHub and NPM Release containing the prebuild artifacts.  The version of the Release comes from the package.json, and in the case of a BETA release automatically appends the beta release version.  During processing it leverages a branch called `release-candidate` as a holding area for prebuilds.

When running the job, most times a couple of the instances of the sub step `Commit & Push Changes` within `Prebuild NPM and GitHub Release artifacts` fails.  When this occurs just re-run.  This is due to concurency issues between the steps and github.

1. Create branch `release-candidate` if not existing ( The script deletes it before starting and will fail if it isn't present ).
2. Start MacOS ARM 64 local runner
3. Ensure version tag within package.json reflects version you want to publish, please note beta tags are added by the action.
4. Run Action `Run prebuild's and Create GitHub and NPM release`, and select branch you wish to publish, and if it needs to be BETA tagged and versioned.
5. This will run for about an hour, and create a github release with the prebuild artifacts attached, and a npm release with the prebuild artifacts attached.

## License

* Copyright (c) 2012-2015, Christopher Jeffrey (MIT License).
* Copyright (c) 2016, Daniel Imms (MIT License).
* Copyright (c) 2018, Microsoft Corporation (MIT License).
* Copyright (c) 2018, David Wilson (MIT License).
* Copyright (c) 2018, oznu (MIT License).
* Copyright (c) 2023, Homebridge (MIT License).
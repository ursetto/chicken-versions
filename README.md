# chicken-versions

This repository contains Chicken binaries built for GitHub runners on Ubuntu 20.04 and 22.04.

You can access these through the [ursetto/chicken-toolchain](https://github.com/ursetto/chicken-toolchain) action. Direct access is not recommended.

Available versions:

- 5.3.0
- 5.2.0
- 5.1.0
- 5.0.0
- 4.13.0

## Method of operation

Pushing a tag `v<chicken-version>`, such as `v5.3.0`, to this repository will:

- Download the Chicken tarball for that version from <https://code.call-cc.org/releases>
- Prepare an empty GitHub release for this version
- Upload the Chicken tarball to the release as an asset, along with its sha256sum, for caching purposes
- Build, test and install Chicken from tarball on each supported runner
- Upload a tarball for each install directory as an asset

Chicken will be installed at `/home/runner/.local/chicken/<chicken-version>`. This full path is stored in the tarball.

If the tag contains a `-`, as in `v5.3.0-1`, the `-suffix` is removed when selecting the Chicken version to build. If it contains `-pre`, the release will be marked as a prerelease.

## New release

To generate a new release, push a new tag such as `v5.4.0`. Push a tag like `v5.4.0-pre1` to create a prerelease, or like `v5.3.0-1` if you are rebuilding an existing version.

Then, update the manifest in [ursetto/chicken-toolchain](https://github.com/ursetto/chicken-toolchain) to add or modify the pointer to the new release.

Warning: You can't push more than 3 tags at a time, for example if you are rebuilding several versions at once. If you do, GitHub will skip generating a push event, and the release workflow will not run at all. In this case, delete your pushed tags from GitHub and push individual tags instead of using `--tags`.

## TODO

- Moving or copying the build portion of the action to `ursetto/chicken-toolchain/build` might be useful for those who want to build their own Chicken.
- Building unofficial Ubuntu .debs for Chicken in `/usr/local/chicken/<version>` seems like a good idea, but this is probably best placed in a separate repo.
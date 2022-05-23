# renovate-packages-lock-bug

This repository showcases that renovate does not create correct PRs when
a dotnet project uses both central package versioning ((either [CentralPackageVersions](https://github.com/microsoft/MSBuildSdks/tree/main/src/CentralPackageVersions) or [NuGet central versioning](https://github.com/NuGet/Home/wiki/Centrally-managing-NuGet-package-versions))
and [lock files](https://devblogs.microsoft.com/nuget/enable-repeatable-package-restores-using-a-lock-file/) together.

## What happens

Only the `.props` file containing the package version is updated. The `packages.locks.json` of projects using
this packages are left as-is, which causes the command `dotnet restore --locked-mode` to fail due to improperly
updated lockfiles.

## What should happen

Along with the `.props` file version update, all affected projects should have `dotnet restore --force-evaluate` run,
and the resulting changes `packages.locks.json` files should be commited.

Example result: https://github.com/mdarocha/renovate-packages-lock-bug/pull/5/files

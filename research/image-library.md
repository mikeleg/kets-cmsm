# Image library for CMS resize and WebP

## Question

Which .NET library should the CMS use to resize uploaded images and convert them to WebP?

Compared: ImageSharp, SkiaSharp, NetVips, Magick.NET.

Criteria: (a) license for commercial use, (b) native dependencies in a Linux Docker image, (c) WebP support, (d) memory and performance.

Context: headless CMS, one installation per client site, .NET Minimal API in Docker on Linux (glibc Debian/Ubuntu base). The product is sold commercially by a small web agency.

## Recommendation

Use SkiaSharp as the default choice, with the package SkiaSharp.NativeAssets.Linux.NoDependencies on Linux. Its wrapper is MIT and the package has no third-party system dependencies [https://www.nuget.org/packages/SkiaSharp.NativeAssets.Linux.NoDependencies]. It can encode WebP through SKEncodedImageFormat.Webp [https://learn.microsoft.com/en-us/dotnet/api/skiasharp.skencodedimageformat].

Before you commit, verify two points that this research did not verify: the license of the native libSkiaSharp build, and the WebP encode call path. See Open points.

NetVips is the alternative if the LGPL native library is acceptable. It is faster on memory, but its native package license is LGPL and the license metadata conflicts.

ImageSharp is not recommended for this agency until the license terms are settled, because version 4 enforces a build-time license key and the revenue threshold may apply to the agency.

## Comparison

| Library | License for commercial use | Native deps in Linux Docker | WebP support | Memory and performance | Verdict |
|---|---|---|---|---|---|
| ImageSharp (SixLabors) | Six Labors Split License 1.0. Free only for a for-profit direct dependency under 1M USD annual gross revenue, a non-profit, or open source. Otherwise a paid Six Labors Commercial License. v4 enforces a sixlabors.lic key at build time for direct dependencies. | None (managed code). | Not verified in this run. | Not verified from primary sources. | Not recommended now. License and key risk for an agency. |
| SkiaSharp (mono) | Wrapper MIT [https://github.com/mono/SkiaSharp]. Native libSkiaSharp license not verified. | NativeAssets.Linux.NoDependencies has no NuGet dependencies. Alpine/musl not verified. | WebP enum present (SKEncodedImageFormat.Webp). Encode path not verified. | Not verified from primary sources. | Recommended default, pending two checks. |
| NetVips + NetVips.Native (libvips) | Binding MIT. NetVips.Native NuGet says LGPL-3.0-or-later. libvips repo and site say LGPL-2.1-or-later. Conflict. | None system-wide. Native package bundles libvips. glibc and musl runtimes exist. | Via libvips and libwebp (BSD-3-Clause). Bundled codec list not verified. | Not verified from primary sources. | Alternative if LGPL is acceptable. |
| Magick.NET + native ImageMagick | Wrapper Apache-2.0. ImageMagick License requires attribution to ImageMagick Studio LLC. | Linux needs OpenMP runtime (libgomp1 on Debian/Ubuntu, libgomp on Alpine via apk). Native Linux package names not verified. | Not verified in this run. | Not verified from primary sources. ImageMagick is generally heavy (not verified). | Not recommended as default. Heavier and no clear advantage. |

## Findings per library

### 1. ImageSharp (SixLabors.ImageSharp)

Package: SixLabors.ImageSharp. Latest version seen on NuGet: 4.1.4. Target frameworks: net8.0 to net10.0. [https://www.nuget.org/packages/SixLabors.ImageSharp]

License file title and version: "Six Labors Split License" "Version 1.0, June 2022". [https://raw.githubusercontent.com/SixLabors/ImageSharp/main/LICENSE]

Commercial terms, as quoted from the license file:

- Apache 2.0 applies when: "You are consuming the Work as a Direct Package Dependency in the capacity of a For-profit company/individual with less than 1M USD annual gross revenue." [https://raw.githubusercontent.com/SixLabors/ImageSharp/main/LICENSE]
- Apache 2.0 also applies to transitive package dependencies, to use in software under an Open Source or Source Available license, and to a non-profit or registered charity as a direct dependency. [https://raw.githubusercontent.com/SixLabors/ImageSharp/main/LICENSE]
- "For all other scenarios, Works in Source or Object form are licensed to You under the Six Labors Commercial License," which "may be purchased by visiting https://sixlabors.com/pricing/." [https://raw.githubusercontent.com/SixLabors/ImageSharp/main/LICENSE]

The license file does not define "gross revenue". [https://raw.githubusercontent.com/SixLabors/ImageSharp/main/LICENSE]

The pricing page uses the opposite direction for the threshold: the commercial license applies to a business "earning more than 1M USD in annual gross revenue." It also lists tiers: Boutique 799 USD/year, Agency 1299 USD/year, Enterprise 4999 USD/year. It states licensing is "Never per seat, per API call, or per deployment." It does not define revenue. [https://sixlabors.com/pricing/]

Version scope:

- The ImageSharp README names only "the Six Labors Split License, Version 1.0." [https://github.com/SixLabors/ImageSharp]
- The v4.0.0 announcement says: "ImageSharp V4.0.0 is licensed under the Six Labors Split License." It also says: "The license terms themselves are not changing." [https://sixlabors.com/posts/announcing-imagesharp-400/]
- The same announcement says: "V4 is also the first ImageSharp release to enforce a build-time license key for direct package dependencies." It adds: "This applies to direct dependencies, not transitive consumers." [https://sixlabors.com/posts/announcing-imagesharp-400/]
- The security post says v3 security support ends on 12 May 2027, and each major version gets 12 months of security updates after its successor's first stable release. [https://sixlabors.com/posts/security-support-and-license-enforcement/]
- Whether v3 (3.x) carried Apache 2.0 terms is not verified. Third-party pages claim it did, but no primary source was read for v3. The official pages name only the Split License. [https://sixlabors.com/posts/announcing-imagesharp-400/]

Key point for the agency: for a direct dependency in a for-profit business, the free grant needs annual gross revenue under 1M USD. Above that, a paid license is needed. [https://sixlabors.com/posts/announcing-imagesharp-400/] [https://raw.githubusercontent.com/SixLabors/ImageSharp/main/LICENSE]

Native dependencies: none. ImageSharp is managed code. The NuGet page lists only System.IO.Hashing as a dependency for net8.0. [https://www.nuget.org/packages/SixLabors.ImageSharp]

Debug vs Release enforcement: not verified from primary sources. A third-party search summary said a Release build without a key fails. Do not rely on this until checked. [https://sixlabors.com/posts/announcing-imagesharp-400/]

WebP support, memory, performance: not verified in this run. The ImageSharp docs were not fetched.

### 2. SkiaSharp (mono/SkiaSharp)

Wrapper license: MIT. The repo sidebar says "MIT license." The README links to LICENSE.md. [https://github.com/mono/SkiaSharp]

NuGet: SkiaSharp.NativeAssets.Linux.NoDependencies, latest stable 4.157.0 on the page (a 4.159.0-rc prerelease exists). License: MIT. Description: "includes the build of libSkiaSharp.so that does not have any dependencies on third-party libraries." The system libraries it needs are the glibc set (libpthread, libdl, libm, libc, ld-linux). Fontconfig is excluded. [https://www.nuget.org/packages/SkiaSharp.NativeAssets.Linux.NoDependencies]

NuGet: SkiaSharp.NativeAssets.Linux, same version and MIT license. Its README says it provides native assets for "this deployment target." Every framework group shows no dependencies. [https://www.nuget.org/packages/SkiaSharp.NativeAssets.Linux]

The README says that because Linux has many distributions, the project ships a separate NuGet package with binaries for a few distros. [https://github.com/mono/SkiaSharp]

Native license of libSkiaSharp (Skia itself): not verified in this run. Skia is generally BSD-3-Clause, but that is not checked here.

Alpine / musl support: not verified. The NativeAssets page has no musl note.

WebP: SKEncodedImageFormat has a Webp member (value 6). [https://learn.microsoft.com/en-us/dotnet/api/skiasharp.skencodedimageformat] The encode call (SKImage.Encode with quality) and its behavior were not fetched.

Memory and performance: not verified from primary sources.

### 3. NetVips + NetVips.Native (libvips)

Binding: NetVips. License: MIT. [https://raw.githubusercontent.com/kleisauke/net-vips/master/LICENSE] [https://www.nuget.org/packages/NetVips]

- NuGet NetVips page: latest 3.2.0. Targets net6.0 and net462. License URL https://licenses.nuget.org/MIT. Dependencies: none for net6.0. [https://www.nuget.org/packages/NetVips]
- The NetVips repo says the NuGet version "is in sync with libvips' version number," and that you need libvips 8.2 or later on the library search path. [https://github.com/kleisauke/net-vips]

Native package: NetVips.Native. License on NuGet: LGPL-3.0-or-later. Description: "This package complements the NetVips package and contains native binaries of libvips." Latest shown: 8.18.7. [https://www.nuget.org/packages/NetVips.Native]

libvips license:

- Repo sidebar: "LGPL-2.1 license." README: "libvips is licensed under the LGPL-2.1-or-later." [https://github.com/libvips/libvips]
- Website: "libvips is licensed under the LGPL-2.1-or-later." [https://www.libvips.org/]

Conflict: NetVips.Native (NuGet) says LGPL-3.0-or-later. libvips says LGPL-2.1-or-later. Which applies to the binary you ship is not verified. This needs a human check. See Open points.

Linux platforms for NetVips.Native (NuGet runtime list): linux-x64, linux-arm, linux-arm64, linux-musl-x64, linux-musl-arm64. [https://www.nuget.org/packages/NetVips.Native] The repo footnotes say glibc is used on Ubuntu and Debian, and musl on Alpine and Gentoo. [https://github.com/kleisauke/net-vips]

Native dependencies in Docker: the package bundles libvips. The repo says the libvips shared library must be on the search path. The NuGet native package supplies it. [https://github.com/kleisauke/net-vips] No Docker install step is documented in the pages read.

WebP: libvips uses libwebp. libwebp license: BSD-3-Clause (copyright Google, 2010). [https://github.com/webmproject/libwebp/blob/main/COPYING] The codecs bundled in NetVips.Native were not verified. The net-vips-native repo page could not be read (HTTP 503 and 404 on related pages).

Memory and performance: not verified from primary sources. libvips is generally known for low memory use, but this run did not check a primary benchmark.

### 4. Magick.NET + native ImageMagick

Wrapper: Magick.NET (Dlemmers / dlemstra). License: Apache-2.0. [https://github.com/dlemstra/Magick.NET] [https://www.nuget.org/packages/Magick.NET-Q16-AnyCPU]

NuGet: Magick.NET-Q16-AnyCPU latest 15.1.1, depends on Magick.NET.Core (>= 15.1.1). Target frameworks: netstandard2.0 and net10.0. [https://www.nuget.org/packages/Magick.NET-Q16-AnyCPU]

NuGet: Magick.NET.Core 15.1.1, license Apache-2.0, no dependencies. Linux native runtime packages are not named on the NuGet page. [https://www.nuget.org/packages/Magick.NET.Core]

Linux native packages named in the repo README: not verified by name. The README lists OpenMP runtime needs: Debian/Ubuntu `apt-get install libgomp1`; Fedora/RHEL `dnf install libgomp`; Alpine/musl `apk add libgomp` (x64). "This library has been tested on Windows, Linux and macOS." [https://github.com/dlemstra/Magick.NET]

Platform detail is in docs/CrossPlatform.md. That page returned HTTP 503 in this run and was not read. [https://github.com/dlemstra/Magick.NET/blob/main/docs/CrossPlatform.md]

ImageMagick license: "ImageMagick License." The page says users may "Freely download and use software for personal, internal, or commercial purposes." It requires you to "Provide clear attribution to ImageMagick Studio LLC." [https://imagemagick.org/license/] (reached via redirect from https://imagemagick.org/script/license.php)

The Magick.NET README says the native library is ImageMagick, and that the wrapper is used "without having to install ImageMagick on your server or desktop." [https://github.com/dlemstra/Magick.NET]

WebP, memory, performance: not verified in this run.

## Open points

1. ImageSharp revenue threshold: the license file says "less than 1M USD annual gross revenue" for a for-profit direct dependency. The pricing page says "more than 1M USD." Both are primary text. Which entity counts (the agency, or each client site) is not defined. A human must decide, probably with a lawyer. [https://raw.githubusercontent.com/SixLabors/ImageSharp/main/LICENSE] [https://sixlabors.com/pricing/]
2. ImageSharp "revenue" is not defined in the license or the pricing page. Not verified.
3. ImageSharp v3 license: not verified whether 3.x was Apache 2.0 or the Split License. Official pages name only the Split License. A human should check the LICENSE file at the 3.x git tag.
4. ImageSharp v4 build-time key: the official post confirms enforcement for direct dependencies. The Debug vs Release behavior is not verified from primary sources. [https://sixlabors.com/posts/announcing-imagesharp-400/]
5. NetVips LGPL version conflict: NetVips.Native (NuGet) says LGPL-3.0-or-later. libvips says LGPL-2.1-or-later. Which applies to the shipped binary needs a check. [https://www.nuget.org/packages/NetVips.Native] [https://github.com/libvips/libvips]
6. LGPL obligations for a closed-source CMS distributed in Docker images: not verified. Needs a legal review. The agency may need to allow the LGPL library to be replaced.
7. SkiaSharp native license (Skia, BSD-3 expected): not verified.
8. SkiaSharp Alpine/musl support: not verified.
9. SkiaSharp WebP encode path: not verified. Check SKImage.Encode with SKEncodedImageFormat.Webp and the quality value.
10. Memory and performance numbers for all four libraries: not verified from primary sources. No third-party benchmark was used.
11. Magick.NET native Linux package names: not verified. CrossPlatform.md returned HTTP 503.
12. NetVips.Native bundled codec list (libwebp version, HEIF/HEVC codecs if any): not verified. The net-vips-native repo could not be read.
13. Inaccessible source: https://sixlabors.com/license returned HTTP 404.
14. Inaccessible source: https://github.com/kleisauke/net-vips-native returned HTTP 503 on a later attempt, and the first read returned MIT and no codec list.

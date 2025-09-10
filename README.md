# The Harmony Community Build for VRChat
This is an experimental package for updating [`Harmony`](https://github.com/pardeike/Harmony), a library for patching, replacing and decorating .NET and Mono methods during runtime, as part of the VRChat SDK.

&nbsp;
## Installation
### Using the VCC (Preferred)
  - [Add my repo](https://misutaaasriel.github.io/Dreemurrs-Repository/) to the VCC, or your [VPM interface of choice](https://vrc-get.anatawa12.com/alcom/).
  - Add "Harmony Library - VRChat SDK Community Build" to your project
### Using the package (Advanced)
  - Extract the ZIP from the [latest release](../../releases/latest)
  - Add the package folder to the VCC as a [User Package](https://vcc.docs.vrchat.com/vpm/packages/#user-packages)
  - Add "VRChat SDK - Harmony (Community Build)" to your project

&nbsp;
## Motivation
VRChat is constantly expanding to new platforms, with iOS being its latest venture. However, its editor tooling for worlds, and many extensions to the Unity Editor as well, are stuck *on PC* due to Harmony failing to support the ARM64 architecture. Amongst other things, **this has prevented Udon Sharp & various community provided tools and utilities from running on modern Macs.** At least without patches...

### Until now.
As of [2.4.0.0](https://github.com/pardeike/Harmony/releases/tag/v2.4.0.0), Harmony **supports the ARM64 architecture, including on macOS.** This, amongst other things, allows Udon Sharp and other packages which rely on Harmony to function... but... not with their published releases.

Due to an issue with how the release packages are compiled, Harmony's public releases are **incompatible** with the **Unity Burst Compiler**, which is **required as part of the VRChat SDK**. As such, **a local solution-level build must be provided** instead. Likewise, package editing **will always be undone with updates**, so a method of providing this DLL **outside of the VRC SDK is necessary.**

This project does the following via [GitHub Actions](../../actions):
- Checks out the latest copy of [`pardeike/Harmony`](https://github.com/pardeike/Harmony)
- Builds the main library *at the solution level* using the following flags: `-c:Release -f:net452`
- Packages the library in a [VPM Package](https://vcc.docs.vrchat.com/vpm/packages/#community-packages)
- Publishes the package, and the library itself, to the [releases](../../releases)
> [!TIP]
> In testing, Unity seems to prefer the community build of Harmony over the version provided by the VRCSDK. Tools such as [`foxscore/Easy-Login`](https://github.com/foxscore/easy-login) and [`Alexejhero/Menu-Item-Overrides`](https://github.com/Alexejhero/Menu-Item-Overrides)¹ function correctly, as does Udon Sharp, running on an M2 MacBook Air with macOS Sequoia 15.6.

> [!NOTE]
> 1. When using `MENU_ITEM_OVERRIDES_DISABLE_DEPENDENCIES` in the scripting define symbols.

&nbsp;
## Additional Notes
> [!NOTE]
> ### Semantic Versioning
> The versioning scheme, `1.2.3+4.5.6`, for the VPM package is as follows:
> 1. Harmony Major Number
> 2. Harmony Minor Number
> 3. Harmony Build Number
> 4. Harmony Revision Number
> 5. Project Commit Number
> 6. GitHub Action Run Number
> 
> This is done with the *intent* that the package versions be treated in the following order:
> ```
> Harmony Major Update
>   Harmony Minor Update
>     New Harmony Build
>       New Harmony Revision
>         Change to the package or repository
>           New package build available
> ```
> This way, new builds of Harmony always take precedence, and are reflected in the package version. Following that, changes to the repository, such as changes to the package layout or metadata, will increment the version number, and lastly, a new build will also increment the counter on the end.
> 
> Thus, hopefully, versions maintain order within the VCC and other VPM utilities.

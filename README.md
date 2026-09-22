> **Note: this is a fork of Jumbo whose `Project.toml` is modified to target package specifically for nonlinear and complex systems modelling, analysis, and timeseries analysis: the JuliaDynamics ecosystem and related packages.**

# Jumbo - JuliaDynamics version

Jumbo is a Julia distribution that comes with commonly needed scientific packages out of the box. Start using Makie, DifferentialEquations, or any included package immediately - no compilation wait. Additional packages can be installed via Pkg without triggering recompilation of pre-installed packages.

This repository also serves as a template for creating custom Julia distributions tailored to your needs. Fork it, modify the `Project.toml` to include your preferred packages, and run the "Build Release Assets" GitHub Actions workflow to generate installers for Linux, macOS, and Windows. The distribution format uses a `Project.toml` with `name` and `version` fields - bundling all listed packages and dependencies into the stdlib path to prevent accidental recompilations.

## Installation

To install Jumbo, download the appropriate pre-built distribution (MSIX, Snap, or DMG) from the **Assets** section on the [releases page](https://github.com/JanisErdmanis/JuBox/releases) (you may need to expand the Assets dropdown for prerelease versions), then follow the installation instructions below for your platform:

- **MSIX (Windows)**: If self-signed, go to MSIX bundle properties and add the certificate to the trusted certificate authorities first (see https://www.advancedinstaller.com/install-test-certificate-from-msix.html). Then double-click on the installer and install the app.
- **Snap (Linux)**: The snap can be installed from a command line: `snap install --classic --dangerous MyApp.snap`
- **DMG (macOS)**: If self-signed, you need to click on the app first, then go to `Settings -> Privacy & Security`, whitelisting the launch request. Then drag and drop the application to the `Applications` folder. Launch the application and go again to `Settings -> Privacy & Security` to whitelist it.

Note that all these extra steps are avoidable with investment in Windows and macOS code signing certificates. For Snap, one can try to submit the app to a snap store so it can be installed with a GUI.

## Building locally (host platform)

To run the build, install Julia 1.13 or later.
Then, change directory into the `Jumbo` folder and execute the following commands:
```bash
julia --startup-file=no --no-init --project=meta
import Pkg; Pkg.update() # optional
Pkg.instantiate()
```

Note that you may also need to run `Pkg.build("AppBundler")` depending on the configuration and status of the `Conda` installation in your system.

Once dependencies are installed and compiled, perform the build:
```bash
julia --startup-file=no --project=meta -m AppBundler build . --build-dir=build --selfsign
```
This creates build artifacts in the `build` directory. By default, the bundle targets the host platform.

Builds can also be performed with GitHub Actions. The release workflow runs each bundle on a compatible runner and uploads the resulting artifacts. It can be started manually or runs when a release is created.

## Building for multiple platforms (GitHub CI)

Generally, the recommended steps for creating a custom Jumbo distribution are:

_Optionally:_
1. Fork the repository.
2. Update `Project.toml` and `Manifest.toml` with your own environment packages.

_Then_ publish a release on GitHub. The **Build Release Assets** GitHub action will automatically create binaries for all platforms it can.

![Betadrive](https://raw.githubusercontent.com/betaflight/.github/main/profile/images/bf_logo.svg#gh-light-mode-only)
![Betadrive](https://raw.githubusercontent.com/betaflight/.github/main/profile/images/bf_logo_dark.svg#gh-dark-mode-only)

# Betadrive Configurator

[![Latest version](https://img.shields.io/github/v/release/betaflight/betaflight-configurator)](https://github.com/betaflight/betaflight-configurator/releases)
[![Build](https://img.shields.io/github/actions/workflow/status/betaflight/betaflight-configurator/deploy.yml?branch=master)](https://github.com/betaflight/betaflight-configurator/actions/workflows/deploy.yml)
[![Crowdin](https://d322cqt584bo4o.cloudfront.net/betaflight-configurator/localized.svg)](https://crowdin.com/project/betaflight-configurator)
[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=betaflight_betaflight-configurator&metric=alert_status)](https://sonarcloud.io/dashboard?id=betaflight_betaflight-configurator)
[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
[![Join us on Discord!](https://img.shields.io/discord/868013470023548938)](https://discord.gg/n4E6ak4u3c)


Betadrive Configurator is a cross-platform configuration and management application for the Betadrive flight-control system.

This project is derived from the [original Betaflight Configurator repository](https://github.com/betaflight/betaflight-configurator).
The upstream project and its historical release links are retained where they are useful for attribution, compatibility, and reference.

## Betadrive changes

The Betadrive fork extends the original configurator for vehicle-oriented firmware targets:

- Adds CAR mixer support with Steering, Throttle, and AUX receiver presentation.
- Adds the `ST123456` channel-map option and CAR-specific failsafe handling.
- Exposes CAR PWM output, neutral pulse, reverse-drive, and 3D-mode controls.
- Adds GLTF/GLB vehicle-model loading, fixed CAR orientation, and dedicated lighting.
- Improves Firefox WebUSB/DFU error handling when WebUSB is unavailable.
- Adds development/build support for Betadrive-specific firmware options and tests.

The configurator is coupled to the [Betadrive firmware](../betadrive). UI controls, mixer identifiers, receiver maps, PWM settings, model behavior, and build-option checks must remain compatible with the firmware implementation. Changes to one repository may therefore require coordinated changes in the other.

Betadrive Configurator is a Progressive Web Application (PWA). The upstream Betaflight application is available at [app.betaflight.com](https://app.betaflight.com).

NOTE: If you are considering testing the latest unstable release then it is at [master.app.betaflight.com](https://master.app.betaflight.com)

Various aircraft and vehicle types are supported by the tool and firmware, including quadcopters, hexacopters, octocopters, fixed-wing aircraft, and CAR targets.

## Historical Releases

These are still available under different operating systems and can be used to configure compatible Betadrive/Betaflight targets. [Historical downloads are available in the original releases.](https://github.com/betaflight/betaflight-configurator/releases)

## Installation

### Standalone

We provide a standalone program for Windows, Linux, Mac and Android.

Download historical upstream installers from the [original releases.](https://github.com/betaflight/betaflight-configurator/releases)

### Notes

#### Windows users

The minimum required version of windows is Windows 8.

#### MacOS X users

Changes to the security model used in the latest versions of MacOS X 10.14 (Mojave) and 10.15 (Catalina) mean that the operating system may show an error message when trying to install the application. To work around this, run the following command in a terminal after installing: `sudo xattr -rd com.apple.quarantine /Applications/Betadrive\ Configurator.app`.

#### Linux users

First step is to download the installer and keep it in your working directory, which can be done with the following command:
```
wget https://github.com/betaflight/betaflight-configurator/releases/download/10.10.0/betaflight-configurator_10.10.0_amd64.deb
```

In most Linux distributions your user won't have access to serial interfaces by default. To add this access right type the following command in a terminal, log out your user and log in again:

```
sudo usermod -aG dialout ${USER}
```

Post-installation errors can be prevented by making sure the directory `/usr/share/desktop-directories` exists. To make sure it exists, run the following command before installing the package:

```
sudo mkdir /usr/share/desktop-directories/
```

The `libatomic` library must also be installed before installing Betadrive Configurator. (If the library is missing, the installation will succeed but the application will not start.) Some Linux distributions (e.g. Fedora) will install it automatically. On Debian or Ubuntu you can install it as follows:

```
sudo apt install libatomic1
```

On Ubuntu 23.10 please follow these alternative steps for installation:

```
sudo echo "deb http://archive.ubuntu.com/ubuntu/ lunar universe" > /etc/apt/sources.list.d/lunar-repos-old.list
sudo apt update
sudo dpkg -i betaflight-configurator_10.10.0_amd64.deb
sudo apt-get -f install
```

On Ubuntu 24.10 and above, please follow these steps, as some deprecated modules are no longer available through apt on this distro:
```
sudo apt update
wget http://archive.ubuntu.com/ubuntu/pool/universe/g/gconf/libgconf-2-4_3.2.6-4ubuntu1_amd64.deb
wget http://archive.ubuntu.com/ubuntu/pool/universe/g/gconf/gconf2-common_3.2.6-4ubuntu1_all.deb
sudo dpkg -i gconf2-common_3.2.6-4ubuntu1_all.deb
sudo dpkg -i libgconf-2-4_3.2.6-4ubuntu1_amd64.deb
sudo dpkg -i betaflight-configurator_10.10.0_amd64.deb
sudo apt-get -f install
```

#### Graphics Issues

If you experience graphics display problems or smudged/dithered fonts in Betadrive Configurator, try invoking the application with the `--disable-gpu` command-line switch. This disables hardware graphics acceleration.

### Unstable Testing Versions

The Betadrive Configurator is developed as a PWA (Progressive Web Application), making it easier to support desktops, tablets, and phones. Development snapshots may be incomplete and should be used for testing only.

- Latest PWA master snapshot of the App: [https://master.app.betaflight.com/](https://master.app.betaflight.com/)

**Be aware that this version is intended for testing / feedback only, and may be buggy or broken, and can cause flight controller settings to be corrupted. Caution is advised when using this version.**

## Languages

**Please do not submit pull requests for translation changes, but read and follow the instructions below!**

Betadrive Configurator has been translated into several languages. The application will try to detect and use your system language if a translation is available.

If you prefer to have the application in English or any other language, you can select your desired language in the first screen of the application.

## Build and Development

### Technical details

The next versions of the App will be a modern tool that based on PWA (Progressive Web Application) and uses principally Node, npm, Vite and Vue for development and building. For Android we use Capacitor as wrapper over the PWA. To build and develop over it, follow the instructions below.

### Prepare your environment

1. Install [node.js](https://nodejs.org/) — use the exact version in [.nvmrc](./.nvmrc)
   (`nvm use` if you have [nvm](https://github.com/nvm-sh/nvm)). The bundled npm must be
   11.6.1 or newer: npm 11.6.0 and older write `package-lock.json` in a different shape, so
   an older npm makes every commit rewrite the whole lockfile. `npm install` warns with
   `EBADENGINE` if your toolchain is too old, and CI fails the *Lockfile in sync* check.

### PWA version

#### Run development version

1. Change to project folder and run `npm install`.
2. Run `npm run dev`.

The web app will be available at http://localhost:8080 with full HMR.

On the **Modes** tab, use the search field above the list to filter mode names as you
type. Search starts at two characters, matches any part of the displayed name, and
ignores case and surrounding whitespace. Results keep the existing mode order and
respect **Hide unused modes**. Clear the field or reduce it to one character to
remove the text filter; mode ranges and links are preserved while hidden.
The no-matches message appears only after mode data has loaded successfully.

#### Run production version

1. Change to project folder and run `npm install`.
2. Run `npm run build`.
3. Run `npm run preview` after build has finished.

Alternatively you can run `npm run review` to build and preview in one step.

The web app should behave directly as in production, available at http://localhost:8080.

### Android version

The Android version uses Capacitor as the native wrapper with custom plugins to bridge the PWA to native capabilities.

#### Prerequisites

You need to install [Android Studio](https://developer.android.com/studio) as Capacitor apps are configured and managed through it.

#### Run development version

1. Change to project folder and run `npm install`.
2. Run `npm run android:run`.

The command will ask for the device to run the app. You need to have some Android virtual machine created or some Android phone [connected using ADB](https://developer.android.com/tools/adb).

As alternative to the step 2, you can execute a `npm run android:open` to open de project into Android Studio and run or debug the app from there.

#### Run development version with live reload

1. Change to project folder and run `npm install`.
2. Run `npm run dev -- --host`. It will start the vite server and will show you the IP address where the server is listening.
3. Run `npm run android:dev`

This will ask for the IP where the server is running (if there are more than one network interfaces). You need to have some Android virtual machine created or some Android phone [connected using ADB](https://developer.android.com/tools/adb).
Any change make in the code will reload the app in the Android device.

### Running tests

`npm test`

## Support and Developers Channel

There's a dedicated Discord server here:

https://discord.gg/n4E6ak4u3c

We also have a Facebook Group. Join us to get a place to talk about Betadrive, compatible firmware, or just hang out with fellow pilots.

https://www.facebook.com/groups/betaflightgroup/

Etiquette: Don't ask to ask and please wait around long enough for a reply - sometimes people are out flying, asleep or at work and can't answer immediately.

### Issue trackers

For Betadrive Configurator issues raise them here

https://github.com/betaflight/betaflight-configurator/issues

For Betadrive Firmware issues raise them in the firmware repository.

https://github.com/betaflight/betaflight/issues

## Developers

We accept clean and reasonable patches, submit them!

## Credits

For the full details of the contributions inherited from the original Betaflight Configurator, please check the [upstream contributors page](https://github.com/betaflight/betaflight-configurator/graphs/contributors).

Detox is gray box end-to-end testing and automation library for mobile apps. It has a lot of great features:

- Cross Platform
- Runs on Devices
- Automatically Synchronized
- Test Runner Independent
- Debuggable

## Setting Up Detox

### Install applesimutils (Mac only)

[applesimutils](https://github.com/wix/AppleSimulatorUtils) is a collection of utils for Apple simulators.

```sh
brew tap wix/brew
brew install applesimutils
```

### Install Jest Globally

```sh
npm install -g jest
```

### Generating Applications

By default, when creating a mobile application, Nx will use Detox to create the e2e tests project.

```shell
nx g @nx/react-native:app frontend
```

### Creating a Detox E2E project for an existing project

You can create a new Detox E2E project for an existing mobile project.

If the `@nx/detox` package is not installed, install the version that matches your `@nx/workspace` version.

{% tabs %}
{%tab label="npm"%}

```sh
# npm
npm install --save-dev @nx/detox
```

{% /tab %}
{%tab label="yarn"%}

```sh
# yarn
yarn add --dev @nx/detox
```

{% /tab %}
{% /tabs %}

Next, generate an E2E project based on an existing project.

```sh
nx g @nx/detox:app your-app-name-e2e --project=your-app-name
```

Replace `your-app-name` with the app's name as defined in your `tsconfig.base.json` file or the `name` property of your `package.json`.

In addition, you need to follow [instructions at Detox](https://github.com/wix/Detox/blob/master/docs/Introduction.Android.md) to do manual setup for Android files.

## Using Detox

### Testing Applications

- Run `nx test-ios frontend-e2e` to build the iOS app and execute e2e tests with Detox for iOS (Mac only)
- Run `nx test-android frontend-e2e` to build the Android app and execute e2e tests with Detox for Android

You can run below commands:

- `nx build-ios frontend-e2e`: build the iOS app (Mac only)
- `nx build-android frontend-e2e`: build the Android app

### Testing against Prod Build

You can run your e2e test against a production build:

- `nx test-ios frontend-e2e --prod`: to build the iOS app and execute e2e tests with Detox for iOS with Release configuration (Mac only)
- `nx test-android frontend-e2e --prod`: to build the Android app and execute e2e tests with Detox for Android with release build type
- `nx build-ios frontend-e2e --prod`: build the iOS app using Release configuration (Mac only)
- `nx build-android frontend-e2e --prod`: build the Android app using release build type

## Configuration

### Using .detoxrc.json

If you need to fine tune your Detox setup, you can do so by modifying `.detoxrc.json` in the e2e project.

#### Change Testing Simulator/Emulator

For iOS, in terminal, run `xcrun simctl list devices available` to view a list of simulators on your Mac. To open your active simulator, `run open -a simulator`. In `frontend-e2e/.detoxrc.json`, you could change the simulator under `devices.simulator.device`.

For Android, in terminal, run `emulator -list-avds` to view a list of emulators installed. To open your emulator, run `emulator -avd <your emulator name>`. In `frontend-e2e/.detoxrc.json`, you could change the simulator under `devices.emulator.device`.

In addition, to override the device name specified in a configuration, you could use `--device-name` option: `nx test-ios <app-name-e2e> --device-name "iPhone 11"`. The `device-name` property provides you the ability to test an application run on specific device.

```shell
nx test-ios frontend-e2e --device-name "iPhone 11"
nx test-android frontend-e2e --device-name "Pixel_4a_API_30"
```

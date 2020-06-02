---
title: Multiplatform
category: clients
permalink: /clients/http-client/multiplatform.html
caption: Multiplatform Http Client
ktor_version_review: 1.2.0
---

The HTTP Client supports several platforms, using the experimental [multiplatform support](https://kotlinlang.org/docs/reference/multiplatform.html)
that was introduced in [Kotlin 1.2](https://blog.jetbrains.com/kotlin/2017/11/kotlin-1-2-released/).

Right now, the supported platforms are JVM, Android, iOS, Js and native.

## Common

For [multiplatform projects](https://kotlinlang.org/docs/reference/multiplatform.html) that for example
share code between multiple platforms, we can create a common module.
That common module can only access APIs that are available on all the targets.
The Ktor HTTP Client exposes a common module that can be used for such projects:

```kotlin
dependencies {
    implementation("io.ktor:ktor-client-core:$ktor_version")
    // ...
}
```

## JVM

To use Ktor on the JVM, you have to include [one of the supported JVM Engines](https://ktor.io/clients/http-client/engines.html#jvm) to your  `build.gradle`(`build.gradle.kts`).

## Android

To use the Android engine you have to add the dependency in your `build.gradle`(`build.gradle.kts`):

```kotlin
dependencies {
    implementation("io.ktor:ktor-client-android:$ktor_version")
    // ...
}
```

You can then use Android Studio, or Gradle to build your project.

## iOS

On iOS, you have to use [Kotlin/Native](https://github.com/JetBrains/kotlin-native), and analogously
to Android, you have to include this artifact as part of the `dependencies` block.

```kotlin
dependencies {
    implementation("io.ktor:ktor-client-ios:$ktor_version")
    // ...
}
```

In the case of iOS, we usually create a `.framework`, and the application project is a regular XCode project written either in Swift or Objective-C that includes that framework. 
So you first have to build the framework using the Gradle tasks exposed by Kotlin/Native, and then open the XCode project.

## Javascript

In the case of a browser or node-js application, you have to use [Kotlin/Js](https://kotlinlang.org/docs/tutorials/javascript/kotlin-to-javascript/kotlin-to-javascript.html).

```kotlin
dependencies {
    implementation("io.ktor:ktor-client-js:$ktor_version")
    // ...
}
```

## Posix compatible desktops: MacOS, Linux, Windows

As an alternative to JVM-compatible engines, you also could use a Ktor client with the [Kotlin/Native](https://github.com/JetBrains/kotlin-native)using the `curl` backend.

```kotlin
dependencies {
    implementation("io.ktor:ktor-client-curl:$ktor_version")
    // ...
}
```

## Samples

There is a full sample using the common client in the `ktor-samples` repository [mpp/client-mpp](https://github.com/ktorio/ktor-samples/tree/master/mpp/client-mpp).

You can use this project as a reference. This project also expose some experimental Gradle tasks to build, install and run the
Android and iOS applications directly from Gradle.

### Android tasks

* `:client-mpp-android:emulatorList` - lists all the available emulators
* `:client-mpp-android:emulatorStart` - starts the emulator (this blocks Gradle for now, so it's better to do this in a separate terminal)
* `:client-mpp-android:emulatorInstall` - install the application inside the emulator
* `:client-mpp-android:emulatorRun` - executes the application inside the emulator

### iOS tasks

* `:client-mpp-ios:startSimulator` - starts the simulator
* `:client-mpp-ios:shutdownSimulator` - shutdowns the simulator
* `:client-mpp-ios:buildXcode` - builds the XCode project that uses the .framework from Kotlin/Native
* `:client-mpp-ios:installSimulator` - installs the application inside the simulator
* `:client-mpp-ios:launchSimulator` - executes the application inside the simulator

Since those tasks are experimental, they might fail with your specific setup. Please let us know so we can improve them.
Or help us with [the iOS tasks](https://github.com/ktorio/ktor-samples/blob/master/mpp/client-mpp/ios/build.gradle){:target="_blank"},
and [the Android ones](https://github.com/ktorio/ktor-samples/blob/master/mpp/client-mpp/android/build.gradle){:target="_blank"}.

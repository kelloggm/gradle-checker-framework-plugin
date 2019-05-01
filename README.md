# Gradle Checker Framework Plugin

[![License](https://img.shields.io/badge/license-apache%202.0-blue.svg)](http://www.apache.org/licenses/LICENSE-2.0)
[![Build Status](https://travis-ci.org/kelloggm/gradle-checker-framework-plugin.svg?branch=master)](https://travis-ci.org/jaredsburrows/gradle-checker-framework-plugin)
[![Twitter Follow](https://img.shields.io/twitter/follow/jaredsburrows.svg?style=social)](https://twitter.com/jaredsburrows)

This plugin configures `JavaCompile` tasks to use the [Checker Framework](https://checkerframework.org).
It is a fork of the original plugin built by [jaredsburrows](https://github.com/jaredsburrows/gradle-checker-framework-plugin).

## Download

NOTE: these instructions apply to the original plugin. To use this version, you must [build
the plugin from source](#using-a-locally-built-plugin) (for now).

**Release:**
```groovy
buildscript {
  repositories {
    jcenter()
  }

  dependencies {
    classpath 'com.jaredsburrows:gradle-checker-framework-plugin:0.2.2'
  }
}

apply plugin: 'com.jaredsburrows.checkerframework'
```
Release versions are available in the [JFrog Bintray repository](https://jcenter.bintray.com/).

**Snapshot:**
```groovy
buildscript {
  repositories {
    maven { url 'https://oss.jfrog.org/artifactory/oss-snapshot-local/' }
  }

  dependencies {
    classpath 'com.jaredsburrows:gradle-checker-framework-plugin:0.2.3-SNAPSHOT'
  }
}

apply plugin: 'com.jaredsburrows.checkerframework'
```
Snapshot versions are available in the [JFrog Artifactory repository](https://oss.jfrog.org/artifactory/libs-snapshot/).

## Using a locally-built plugin

To build the plugin from source, run `./gradlew build`.

If you want to use a locally-built version of the plugin, you can publish the plugin to your
local Maven repository by running `./gradlew publish`. In the `build.gradle` file for each
project for which you want to use the locally-built plugin, make sure that `mavenLocal()`
is the first entry in the `repositories` block within the `buildscript` block. A full example
will look like this:

```groovy
buildscript {
  repositories {
    mavenLocal()
  }
  
  dependencies {
    classpath 'com.jaredsburrows:gradle-checker-framework-plugin:0.2.3-SNAPSHOT'
  }
}

apply plugin: 'com.jaredsburrows.checkerframework'
```

## Configuration

### Configuring which checkers to use

The `checkerFramework.checkers` property lists which checkers will be run.

For example:

```groovy
checkerFramework {
  checkers = [
    'org.checkerframework.checker.nullness.NullnessChecker',
    'org.checkerframework.checker.units.UnitsChecker'
  ]
}
```

For a list of checkers, see the [Checker Framework Manual](https://checkerframework.org/manual/#introduction).

### Providing checker-specific options to the compiler

You can set the `checkerFramework.extraJavacArgs` property in order to pass additional options to the compiler when running
a typechecker.

For example, to use a stub file:

```groovy
checkerFramework {
  extraJavacArgs = [
    '-Astubs=/path/to/my/stub/file.astub'
  ]
}
```

### Configuring third-party checkers

To use a third-party typechecker (i.e. one that is not distributed with the Checker Framework),
add a dependency to the `checkerFramework` dependency configuration.

For example, to use the [Glacier](http://mcoblenz.github.io/Glacier/) immutability checker:

```groovy
dependencies {
  ...
  checkerFramework 'edu.cmu.cs.glacier:glacier:0.1'
}
```

### Specifying a Checker Framework version

To change the version of the Checker Framework used by the plugin (by default version 2.8.0),
you should add `checkerFramework` entries to your `dependencies` block. 
We recommend **explicitly** specifying the latest Checker Framework version, and updating
often. You can find the latest version of the Checker Framework 
[here](https://github.com/typetools/checker-framework/releases).
For example, to use version 2.8.1 of the Checker Framework:

```groovy
dependencies {
  checkerFramework 'org.checkerframework:checker-qual:2.8.1'
  checkerFramework 'org.checkerframework:checker:2.8.1'
  checkerFramework 'org.checkerframework:jdk8:2.8.1'
}
```

You can also use a locally-built version of the Checker Framework:

```groovy
cfHome = String.valueOf(System.getenv("CHECKERFRAMEWORK"))

dependencies {
  checkerFramework files(cfHome + "/checker/dist/checker.jar")
  checkerFramework files(cfHome + "/checker/dist/checker-qual.jar")
  checkerFramework files(cfHome + "/checker/dist/jdk8.jar")
}
```

### Other options

By default, the plugin applies the selected checkers to all `JavaCompile` targets.
The plugin includes a rudimentary option for preventing checkers from being applied
to test targets. To use it, add the following to the `checkerFramework` block:

```groovy
checkerFramework {
  excludeTests = true
}
```

The check for test targets is entirely syntactic: this option will not apply the checkers
to any task whose name includes "test", ignoring case. The default value is `false`.

## License

    Copyright (C) 2017 Jared Burrows

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.

# Gradle Experiments

Main idea/problem is to troubleshoot `com.github.hannesa2:paho.mqtt.android:4.2.2` dependency.

OS: Windows mainly.

## Init

First you have to have Java.

Gradle 7.2 supports max Java 16, and Gradle 7.5 already supports Java 18.

Latest Gradle version is 8.10, but even Gradle 8.6 seems to be referring to Gradle 9 (in regards future deprecation maybe).

If you download Gradle v8.10 for example, and then used executable to init project:

```sh
../gradle-8.10/bin/gradle.bat init --type java-application
```

(creates `settings.gradle` and `app/build.gradle`), when selecting Groovy.

or

```shell
../gradle-8.10/bin/gradle.bat init --type kotlin-application
```

(creates `settings.gradle.kts` and `app/build.gradle.kts` and `App.kt`)

it create locally:

- `./gradle/wrapper` folder and files inside:
  - `gradle-wrapper.jar` will be new
  - `gradle-wrapper.properties` will be updated also
- executable `./gradlew` for Unix
- executable `./gradlew.bat` for Windows

Then:

```sh
./gradlew.bat --version
```

will give you 8.10

## Build

```sh
./gradlew.bat build
```

or

```sh
./gradlew.bat build  --refresh-dependencies
```

```sh
 ./gradlew.bat build  --refresh-dependencies --warning-mode all
```

Note that `BUILD SUCCESSFUL` but I can't verify if

```sh
./gradlew.bat clean
```

```sh
./gradlew.bat app:dependencies
```

Gives:

```
\--- com.google.guava:guava:33.2.1-jre
     +--- com.google.guava:failureaccess:1.0.2
     +--- com.google.guava:listenablefuture:9999.0-empty-to-avoid-conflict-with-guava
     +--- com.google.code.findbugs:jsr305:3.0.2
     +--- org.checkerframework:checker-qual:3.42.0
     \--- com.google.errorprone:error_prone_annotations:2.26.1
```

## Run

```sh
./gradlew.bat run
```

```sh
./gradlew.bat run --warning-mode all
```

=>

```sh
> Task :app:run
Hello World!
```

Gives more logs...

## Project Gradle Upgrade

after Project init it creates `gradle/wrapper` and it seems hardcoded version for local project usage.

```shell
./gradlew.bat wrapper --gradle-version 8.10
```

If some errors occur, and you need to suppress/force, run this:

```shell
./gradlew.bat wrapper --gradle-version 8.10 --distribution-type all
```

Then:

```sh
./gradlew.bat --version
```

## Troubleshooting

```sh
./gradlew.bat --status
```

=>

```
   PID STATUS   INFO
 17128 IDLE     8.6
 19404 STOPPED  (stop command received)
  5696 STOPPED  (by user or operating system)

Only Daemons for the current Gradle version are displayed. For more on this, please refer to https://docs.gradle.org/8.6/userguide/gradle_daemon.html#sec:status in the Gradle documentation.
```


Actually, when above happens, it means there are t least two conflicting Gradle daemons (maybe you used Gradle 7.2 version and then switched to another project and used Gradle 8.6 version).

Then you better do:

```shell
./gradlew.bat --stop
```

And then do `clean` or `build` or `run`...

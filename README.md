
# Weave Gradle RunMod Task

This task implementation allows for you to build and run your mod with one gradle task utilising [Youded's LCQT](https://github.com/Youded-byte/lunar-client-qt)

## How to use / Requirements
You need to get the latest release of [Youded's LCQT](https://github.com/Youded-byte/lunar-client-qt) and place the `Lunar Client QT` folder in your projects root directory and rename it to `lcqt`. If you want to use this on linux go on the linuxtask.txt file on the repo and use that code instead, steps should be the same.

Add the following code into your build.gradle.kts
```
tasks {
    val buildAndCopyMod by registering(Copy::class) {
        group = "minecraft"
        description = "Builds the mod and copies it into the mods folder for Lunar Client"

        from("$buildDir/libs")
        into(System.getProperty("user.home") + "/.weave/mods")
    }

    val startLunarClient by registering(Exec::class) {
        group = "minecraft"
        description = "Starts Lunar Client from Lunar Client QT by YouDed via CLI"

        commandLine("$projectDir\\lcqt\\lunar-client-qt.exe", "--nogui")

        mustRunAfter(buildAndCopyMod)
    }

    val runMod by registering {
        group = "minecraft"
        description = "Builds the mod, copies it into the mods folder for Lunar Client, and starts Lunar Client"

        dependsOn(buildAndCopyMod, startLunarClient)
    }
}
```

And now there should be a gradle task under the `minecraft` group called `runMod` just simply press on it to start it

## How it works
This task simply builds the mod and copies it into the mods folder for weave that LCQT uses which is `%userprofile%/.weave/mods`and then starts Lunar Client from Lunar Client QT by YouDed via CLI `"$projectDir\\lcqt\\lunar-client-qt.exe", "--nogui"`
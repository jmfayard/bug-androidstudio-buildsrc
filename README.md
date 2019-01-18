# Android Studio and buildSrc integration

The IDE support for the buildSrc module is broken in Android Studio 3.3

This project is a minimal example that demonstrate the issue.

See [bug 123032843  IDE integration with the Gradle buildSrc module is broken in Android Studio 3.3](https://issuetracker.google.com/issues/123032843)


## Longer description ##

The user experience of editing a Gradle build in Android Studio is in general not good at all.

<img src="https://github.com/jmfayard/buildSrcVersions/raw/master/doc/try-again.png">

Then Gradle gave us the `buildSrc` module

> The directory buildSrc is treated as an included build. Upon discovery of the directory, Gradle automatically compiles and tests this code and puts it in the classpath of your build script. For multi-project builds there can be only one buildSrc directory, which has to sit in the root project directory. buildSrc should be preferred over script plugins as it is easier to maintain, refactor and test the code.
> buildSrc uses the same source code conventions applicable to Java and Groovy projects. It also provides direct access to the Gradle API. Additional dependencies can be declared in a dedicated build.gradle under buildSrc.

And Jetbrains Intellij built a very nice integration of it.
It was backported and well and good in Android Studio 3.2.

When you edit your `build.gradle` files, you can jump to its definition and have auto-completion. A game changer.

<img height="300" src="https://user-images.githubusercontent.com/459464/50812046-661c1900-1311-11e9-8bbb-4ac1e0ec1437.png">


## Minimal example

Alas, this IDE integration broken in the newly released Android Studio 3.3


```groovy
// buildSrc/src/main/kotlin/Config.kt
object Config {

    const val buildSrcHelp = """
The Gradle buildSrc is a special module available in all your builds.
        """
}

// build.gradle
tasks.register("run") {
    println(Config.buildSrcHelp)
}

```

Gradle understands it

```
$ ./gradlew run
The Gradle buildSrc is a special module available in all your builds.
```

Jetbrains IntelliJ and Android Studio 3.2 provide IDE support for this.

This is broken in Android Studio 3.3

Auto-completion do not work. You cannot jump to the definition of `Config`


![image](https://user-images.githubusercontent.com/459464/51384200-e4e83180-1b1b-11e9-9fd1-7e83deacb014.png)

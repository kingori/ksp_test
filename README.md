# ksp_test

Sample project to reproduce an issue where Autoservice fails to find classes in a specific sourceset.

## how to reproduce

Run gradle command

```
$ ./gradlew app:assembleAlphaDebug
```

and check service file is generated correctly. You need jdk 17 to run build.

```
$ cat app/build/generated/ksp/alphaDebug/resources/META-INF/services/mypkg.MyService
mypkg.ServiceA
mypkg.ServiceB
mypkg.ServiceC
```

In this case, `mypkg.ServiceD` is omiitted.

## how to fix problem

Just update android gradle plugin to latest version in build.gradle like below.
```
// Top-level build file where you can add configuration options common to all sub-projects/modules.
plugins {
    id 'com.android.application' version '8.1.0-beta04' apply false
//    id 'com.android.application' version '7.3.1' apply false
    id 'org.jetbrains.kotlin.android' version '1.8.21' apply false
    id 'com.google.devtools.ksp' version '1.8.21-1.0.11' apply false
}
```

then run gradle and check service file.
```
$ cat app/build/generated/ksp/alphaDebug/resources/META-INF/services/mypkg.MyService 
mypkg.ServiceA
mypkg.ServiceB
mypkg.ServiceC
mypkg.ServiceD
```

You can see `mypkg.ServiceD` now.


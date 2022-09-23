# Kodein JIT Bindings
An extension for Kodein DI that can resolve bindings at runtime.

This library was based on Kodein-jxInject but specialized for actual runtime instantiation instead of complying with javax.inject.

**Warning**: The version v2.0 had a lot of breaking changes.
Be sure to correctly migrate to it by checking the end of this document.

Licensed under the [MIT License](https://github.com/notjustanna/kodein-jit-bindings/blob/master/LICENSE).

### Installation

![Latest Version](https://api.bintray.com/packages/notjustanna/kotlin/kodein-jit-bindings/images/download.svg)

Using in Gradle:

```gradle
repositories {
  jcenter()
}

dependencies {
    compile 'org.kodein.di:kodein-di-generic-jvm:6.2.0'
    // OR
    compile 'org.kodein.di:kodein-di-erased-jvm:6.2.0'

    compile 'net.notjustanna.libs:kodein-jit-bindings:LATEST' // replace LATEST with the version above
}
```

Using in Maven:

```xml
<repositories>
  <repository>
    <id>central</id>
    <name>bintray</name>
    <url>http://jcenter.bintray.com</url>
  </repository>
</repositories>

<dependencies>
  <dependency>
    <groupId>net.notjustanna.libs</groupId>
    <artifactId>kodein-jit-bindings</artifactId>
    <version>LATEST</version> <!-- replace LATEST with the version above -->
  </dependency>
</dependencies>
```

### Usage

First, import the module to the Kodein:
```kotlin 
val kodein = Kodein {
    installJit()
    /* Other bindings */
}
```

Creating new instances in Kotlin:
```kotlin
val controller = kodein.jitInstance<MyJavaController>()
```

Creating new instances in Java:
```java
MyJavaController controller = KodeinJIT.newInstance(kodein, MyJavaController.class);
```

## About KodeinJIT 2.0

- KodeinJIT got a full rewrite, in order to fix circular dependencies.
- You can make KodeinJIT recursively instantiate just-in-time all the required dependencies.
- **`NEW`** Multiple constructors? Annotate the right one with the **@JIT** annotation to make sure it's the right one!
- **`NEW`** Annotate your class with the **@Singleton** annotation to make the JIT instance a singleton!
  - **Note**: A singleton-annotated class trying to instantiate another singleton-annotated class will throw a circular dependency exception.

# EventBus Adapter for Java

The EventBus Adapter wraps various EventBus implementations for Java and Android. It uses a uniform
interface (`com.cookingfox.eventbus.EventBus`) to type your classes to, so that it's possible to
change the implementation when required.

## Download

[![Download](https://api.bintray.com/packages/cookingfox/maven/eventbus-adapter-java/images/download.svg) ](https://bintray.com/cookingfox/maven/eventbus-adapter-java/_latestVersion)

The distribution is hosted on [Bintray](https://bintray.com/cookingfox/maven/eventbus-adapter-java/view).
To include the package in your projects, you can add the jCenter repository.

### Gradle

Add jCenter to your `repositories` block:

```groovy
repositories {
    jcenter()
}
```

and add the project to the `dependencies` block in your `build.gradle`:

```groovy
dependencies {
    compile 'com.cookingfox:eventbus-adapter-java:1.0.0'
}
```

### Maven

Add jCenter to your repositories in `pom.xml` or `settings.xml`:

```xml
<repositories>
    <repository>
        <id>jcenter</id>
        <url>http://jcenter.bintray.com</url>
    </repository>
</repositories>
```

and add the project declaration to your `pom.xml`:

```xml
<dependency>
    <groupId>com.cookingfox</groupId>
    <artifactId>eventbus-adapter-java</artifactId>
    <version>1.0.0</version>
</dependency>
```

## Features

Currently the library has adapters for:

- [GreenRobot EventBus](https://github.com/greenrobot/EventBus) (tested with version 2.4.0)
- [Google Guava EventBus](https://github.com/google/guava) (tested with version 18.0)

The main `EventBus` interface actually inherits from the `EventBusPublisher` and
`EventBusSubscriber` interfaces. This allows the user to restrict the available functionality of the
consuming class.

## Usage

Include in your project's dependencies:

1. This wrapper library. (see "Download")
2. The library you want to wrap. (see "Features")

Example for the GreenRobot EventBus:

```java
class ExampleEvent {}

class ExampleSubscriber {
    com.cookingfox.eventbus.EventBus eventBus;

    ExampleSubscriber(com.cookingfox.eventbus.EventBus eventBus) {
        this.eventBus = eventBus;
    }

    void onCreate() {
        eventBus.register(this);
    }

    void onEvent(ExampleEvent event) {
        // handle event
    }

    void onDestroy() {
        eventBus.unregister(this);
    }
}

// create real EventBus and adapter
de.greenrobot.event.EventBus realEventBus = new de.greenrobot.event.EventBus();
com.cookingfox.eventbus.EventBus eventBusAdapter = new GreenRobotEventBusAdapter(realEventBus);

// create and register subscriber
ExampleSubscriber subscriber = new ExampleSubscriber(eventBusAdapter);
subscriber.onCreate();

// post an event
eventBusAdapter.post(new ExampleEvent());

// unregister subscriber
subscriber.onDestroy();
```

If you want to restrict your consumer classes' EventBus capabilities, use the following interfaces:

- `EventBusPublisher`: the class will only be able to use the `post` method.
- `EventBusSubscriber`: the class will only be able to use the `register` and `unregister` methods.

## F.A.Q.

#### _Is it possible to support EventBus library X?_

If you make an issue for it, we'll take a look at it! :)

## Copyright and license

Code and documentation copyright 2015 Cooking Fox. Code released under the Apache 2.0 license.
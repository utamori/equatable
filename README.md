# Equatable

[![Build Status](https://travis-ci.org/felangel/equatable.svg?branch=master)](https://travis-ci.org/felangel/equatable)
[![codecov](https://codecov.io/gh/felangel/equatable/branch/master/graph/badge.svg)](https://codecov.io/gh/felangel/equatable)
[![Pub Package](https://img.shields.io/pub/v/equatable.svg)](https://pub.dartlang.org/packages/equatable)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Gitter](https://img.shields.io/badge/gitter-equatable-yellow.svg)](https://gitter.im/equatable_package/community)

## Overview

Being able to compare objects in `Dart` often involves having to override the `==` operator as well as `hashCode`.

Not only is it verbose and tedious, but failure to do so can lead to inefficient code which does not behave as we expect.

By default, `==` returns true if two objects are the same instance.

Let's say we have the following class:

```dart
class Person {
    final String name;

    const Person(this.name);
}
```

We can create create instances of `Person` like so:

```dart
void main() {
    final Person bob = Person("Bob");
}
```

Later if we try to compare two instances of `Person` either in our production code or in our tests we will run into a problem.

```dart
print(bob == Person("Bob")); // false
```

For more information about this, you can check out the official [Dart Documentation](https://www.dartlang.org/guides/language/effective-dart/design#equality).

In order to be able to compare two instances of `Person` we need to change our class to override `==` and `hashCode` like so:

```dart
class Person {
    final String name;

    const Person(this.name);

    @override
    bool operator ==(Object other) =>
        identical(this, other) ||
        other is Person &&
        runtimeType == other.runtimeType &&
        name == other.name;

    @override
    int get hashCode => name.hashCode;
}
```

Now if we run the following code again:

```dart
print(bob == Person("Bob")); // true
```

it will be able to compare different instances of `Person`.

You can see how this can quickly become a hassle when dealing with complex classes. This is where `Equatable` comes in!

## What does Equatable do?

`Equatable` overrides `==` and `hashCode` for you so you don't have to waste your time writing lots of boilerplate code.

There are other packages that will actually generate the boilerplate for you; however, you still have to run the code generation step which is not ideal.

With `Equatable` there is no code generation needed and we can focus more on writing amazing applications and less on mundane tasks.

## Usage

First, we need to do add `equatable` to the dependencies of the `pubspec.yaml`

```yaml
dependencies:
  equatable: ^0.1.0
```

Next, we need to install it:

```sh
# Dart
pub get

# Flutter
flutter packages get
```

Lastly, we need to extend `Equatable`

```dart
import 'package:equatable/equatable.dart';

class Person extends Equatable {
    final String name;

    Person(this.name) : super([name]);
}
```

We can now compare instances of `Person` just like before without the pain of having to write all of that boilerplate.

# Specify class fields inline as part of JavaScript class constructor

Being able to declaratively add fields to a class that are initalised with values passed to the constructor

```js
class Rectangle {
  constructor(this height, this width) {}
}
```

would be equivalent to

```js
class Rectangle {
  height;
  width;
  constructor(height, width) {    
    this.height = height;
    this.width = width;
  }
}
```

## Alternative Syntax

### this.

```js
class Rectangle {
  constructor(this.height, this.width) {}
}
```

This would be similar to existing destructing syntax `[this.height, this.width] = arguments` but unclear if the behaviour would be different in a constructor where it might also add the named bindings of `height` and `width` to the constructor's scope.

### public keyword
  
```js
class Rectangle {
  constructor(public height, public width) {}
}
```

Using the `public` keyword could lead people to think that `private` should also work, when private class fields are currently written with `#<identifier>` syntax not the keyword.

## Prior art

TypeScript:

```ts
class Rectangle {
  constructor(
    public height: number,
    public width: number
  ) {}
}
```

Kotlin:

```kt
class Rectangle(
    val height: Double,
    val width: Double,
) {}
```

Java 14:

```java
record Rectangle (Double height, Double width) {}
```

Python:

```py
@dataclass
class Rectangle:
     width: float
     height: float
```

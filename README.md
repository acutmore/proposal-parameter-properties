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

### Private fields

```js
class Rectangle {
  constructor(this #height, this #width) {}
  
  get height() { return this.#height; }
  get width() { return this.#width; }
}
```

would be equivalent to

```js
class Rectangle {
  #height;
  #width;

  constructor(height, width) {
    this.#height = height;
    this.#width = width;
  }
  
  get height() { return this.#height; }
  get width() { return this.#width; }
}
```

### Inheritance

It is currently a ReferenceError to access `this` in the constructor of class with an extends clause before the super constructor has returned.
Therefore the indetifier binding would be created first, and only after the super constructor would the arguments be assigned to the instance.

```js
class Shape {
  constructor(this area) {}
}

class Rectangle extends Shape {
  constructor(this height, this width) {
    super(height * width);
  }
}
```

would be equivalent to:

```js
class Shape {
  area;
  constructor(area) {
    this.area = area;
  }
}

class Rectangle extends Shape {
  height;
  width;
  constructor(height, width) {
    super(height * width);
    this.height = height;
    this.width = width;
  }
}
```

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
     height: float
     width: float
```

## Alternative Syntax

### this.identfier

```js
class Rectangle {
  constructor(this.height, this.width) {}
}
```

This would be similar to existing destructing syntax `[this.height, this.width] = arguments` but unclear if the behaviour would be different in a constructor where it would also add the named bindings of `height` and `width` to the constructor's scope.

This could be combined with the [proposal-as-patterns](https://github.com/zkat/proposal-as-patterns)

```js
class Rectangle {
  constructor(this.height as height, this.width as width) {}
}
```

### public keyword
  
```js
class Rectangle {
  constructor(public height, public width) {}
}
```

This would match TypeScript's syntax. A downside to using the `public` keyword could lead people to think that `private` should also work, when private class fields are currently written with `#<identifier>` syntax not the keyword.

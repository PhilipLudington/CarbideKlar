# Generics Patterns

This document covers common patterns for using generics in Klar.

## Basic Generic Functions

Generic functions work with any type that satisfies their bounds:

```klar
// Identity function - works with any type
fn identity[T](value: T) -> T {
    return value
}

// Usage
let x: i32 = identity(42)
let s: string = identity("hello")
```

## Trait-Bounded Generics

Constrain types to those implementing specific traits:

```klar
// Single bound
fn max[T: Ordered](a: T, b: T) -> T {
    if a > b {
        return a
    }
    return b
}

// Multiple bounds
fn sort_and_print[T: Ordered + Printable](items: List[T]) {
    let sorted: List[T] = sort(items)
    for item in sorted {
        item.print()
    }
}
```

## Generic Structs

Define data structures that work with any type:

```klar
// Simple generic struct
struct Pair[A, B] {
    first: A
    second: B
}

// Usage
let point: Pair[i32, i32] = Pair { first: 10, second: 20 }
let named: Pair[string, f64] = Pair { first: "pi", second: 3.14159 }

// Generic struct with methods
impl Pair[A, B] {
    fn swap(self) -> Pair[B, A] {
        return Pair { first: self.second, second: self.first }
    }
}
```

## Generic Enums

Enums with type parameters for flexible data modeling:

```klar
// Option type - value that may or may not exist
enum Option[T] {
    Some(T)
    None
}

// Result type - success or error
enum Result[T, E] {
    Ok(T)
    Err(E)
}

// Usage with match (statement-based)
let maybe: Option[i32] = Some(42)
var value: i32
match maybe {
    Some(v) => { value = v }
    None => { value = 0 }
}
```

## Where Clauses

Use where clauses for complex bounds:

```klar
fn merge_maps[K, V](a: Map[K, V], b: Map[K, V]) -> Map[K, V]
where
    K: Hashable + Eq
    V: Clone
{
    var result: Map[K, V] = a.clone()
    for (key, value) in b {
        result.insert(key.clone(), value.clone())
    }
    return result
}
```

## Monomorphization

Klar compiles generic code through monomorphization - creating specialized versions for each concrete type used:

```klar
fn add[T: Add](a: T, b: T) -> T {
    return a + b
}

// Calling with different types creates specialized versions:
let i: i32 = add(1, 2)      // Creates add$i32
let f: f64 = add(1.0, 2.0)  // Creates add$f64
```

Benefits:
- No runtime overhead
- Full type safety
- Optimized code for each type

## Generic Method Patterns

### Factory Methods

```klar
impl List[T] {
    fn new() -> List[T] {
        return List { data: [], len: 0 }
    }

    fn with_capacity(cap: i32) -> List[T] {
        return List { data: alloc(cap), len: 0 }
    }
}

// Usage
let numbers: List[i32] = List.new()
let strings: List[string] = List.with_capacity(100)
```

### Transform Methods

```klar
impl List[T] {
    fn map[U](self: &Self, f: fn(T) -> U) -> List[U] {
        var result: List[U] = List.with_capacity(self.len)
        for item in self {
            result.push(f(item))
        }
        return result
    }
}

// Usage
let numbers: List[i32] = List.from([1, 2, 3])
let doubled: List[i32] = numbers.map(|x: i32| -> i32 { return x * 2 })
```

### Filter Methods

```klar
impl List[T] {
    fn filter(self: &Self, pred: fn(&T) -> bool) -> List[T]
    where T: Clone
    {
        var result: List[T] = List.new()
        for item in self {
            if pred(&item) {
                result.push(item.clone())
            }
        }
        return result
    }
}
```

## Trait Objects vs Generics

### Static Dispatch (Generics)

```klar
// Generic - resolved at compile time
fn draw_shape[T: Drawable](shape: T) {
    shape.draw()
}

// Each call with different types creates separate code
draw_shape(circle)    // Calls draw_shape$Circle
draw_shape(rectangle) // Calls draw_shape$Rectangle
```

### Dynamic Dispatch (Trait Objects)

```klar
// Trait object - resolved at runtime (future feature)
fn draw_shapes(shapes: List[dyn Drawable]) {
    for shape in shapes {
        shape.draw()  // Virtual dispatch
    }
}
```

Use generics when:
- Types are known at compile time
- Performance is critical
- You need full type information

Use trait objects when:
- You need heterogeneous collections
- Types are only known at runtime
- Code size matters more than speed

## Common Generic Patterns

### Newtype Pattern

```klar
struct UserId(i64)
struct OrderId(i64)

// Type safety - can't mix them up
fn get_user(id: UserId) -> User { ... }
fn get_order(id: OrderId) -> Order { ... }

// These would be compile errors:
// get_user(order_id)  // Error: expected UserId, got OrderId
```

### Builder Pattern

```klar
struct RequestBuilder[S] {
    method: Method
    url: string
    headers: Map[string, string]
    body: ?string
    _state: S
}

struct NoUrl {}
struct HasUrl {}

impl RequestBuilder[NoUrl] {
    fn new() -> RequestBuilder[NoUrl] {
        return RequestBuilder {
            method: Method.Get
            url: ""
            headers: Map.new()
            body: None
            _state: NoUrl {}
        }
    }

    fn url(self: Self, url: string) -> RequestBuilder[HasUrl] {
        return RequestBuilder {
            method: self.method
            url: url
            headers: self.headers
            body: self.body
            _state: HasUrl {}
        }
    }
}

impl RequestBuilder[HasUrl] {
    fn build(self: Self) -> Request {
        return Request {
            method: self.method
            url: self.url
            headers: self.headers
            body: self.body
        }
    }
}

// Type-safe builder - can only build after setting URL
let request: Request = RequestBuilder.new()
    .url("https://api.example.com")
    .build()
```

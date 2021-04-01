c++ new features

# c++20

## language features

### coroutine:
Coroutines are special functions that can have their execution suspended and resumed. To define a coroutine, the co_return, co_await, or co_yield keywords must be present in the function's body. C++20's coroutines are stackless; unless optimized out by the compiler, their state is allocated on the heap.

```
generator<int> range(int start, int end) {
  while (start < end) {
    co_yield start; //give out the result and yield
    start++;
  }

  // Implicit co_return at the end of this function:
  // co_return;
}

for (int n : range(0, 10)) {
  std::cout << n << std::endl;
}
```

another example:

```
task<void> echo(socket s) {
  for (;;) {
    auto data = co_await s.async_read();
    co_await async_write(s, data);
  }

  // Implicit co_return at the end of this function:
  // co_return;
}
```
wait for read and then wait for write.

### concepts
concepts: named compile time predicates
```
template <typename T>
concept signed_integral = integral<T> && std::is_signed_v<T>;
```

used to ensure type requirements et al.

### designated initializers

struct A {
  int x;
  int y;
  int z = 123;
};

A a {.x = 1, .z = 2}; //x=1,y=0,z=2;

### Template syntax for lambdas
```
auto f = []<typename T>(std::vector<T> v) {
  // ...
};
```

### Range-based for loop with initializer
```
for (std::vector v{1, 2, 3}; auto& e : v) {
  std::cout << e;
}
```

### likely and unlikely attributes
to give compiler hint to optimized
```
int random = get_random_number_between_x_and_y(0, 3);
[[likely]] if (random > 0) {
  // body of if statement
  // ...
}

[[unlikely]] while (unlikely_truthy_condition) {
  // body of while statement
  // ...
}
```

### capture this
using [=,this] or [=,*this] instead of [=]

### Class types in non-type template parameters
Classes can now be used in non-type template parameters. Objects passed in as template arguments have the type const T, where T is the type of the object, and has static storage duration.

```
struct foo {
  foo() = default;
  constexpr foo(int) {}
};

template <foo f> //pass a foo object f.
auto get_foo() {
  return f;
}

get_foo(); // uses implicit constructor
get_foo<foo{123}>(); //pass a foo object 123

```

### constexpr virtual functions
constexpr can override virtual function, evaluated at compile time. and vice versa

```
struct X1 {
  virtual int f() const = 0;
};

struct X2: public X1 {
  constexpr virtual int f() const { return 2; }
};

struct X3: public X2 {
  virtual int f() const { return 3; }
};

struct X4: public X3 {
  constexpr virtual int f() const { return 4; }
};
```

### explicit(bool)
to specify if shall be explicit.

```
struct foo {
  // Specify non-integral types (strings, floats, etc.) require explicit construction.
  template <typename T>
  explicit(!std::is_integral_v<T>) foo(T) {}
};
```

### immediate function: consteval

```
consteval int sqr(int n) {
  return n * n;
}

constexpr int r = sqr(100); // OK
int x = 100;
int r2 = sqr(x); // ERROR: the value of 'x' is not usable in a constant expression
                 // OK if `sqr` were a `constexpr` function
```
only can use const

### enum 
```
enum class rgba_color_channel { red, green, blue, alpha };

std::string_view to_string(rgba_color_channel my_channel) {
  switch (my_channel) {
    using enum rgba_color_channel;
    case red:   return "red";
    case green: return "green";
    case blue:  return "blue";
    case alpha: return "alpha";
  }
}				 
```

### lambda capture of parameter pack

```
template <typename... Args>
auto f(Args&&... args){
    // BY VALUE:
    return [...args = std::forward<Args>(args)] {
        // ...
    };
}
Capture parameter packs by reference:

template <typename... Args>
auto f(Args&&... args){
    // BY REFERENCE:
    return [&...args = std::forward<Args>(args)] {
        // ...
    };
}
```

### char8_t for utf8 string_view

## c++20 library features
same_as
derive_from
convertible_to
common_with
integral
default_constructible

boolean: if the type can be used as a bool
equality_comparable

object:
movable
copyable
semiregular: 
regular

callable:
invocable: 
predicate:

synchronized buffered ostream
osyncstream (no interleaving of output)

span: 
bit operations: 
popcount: get number of bit 1 in x.

math constants

is_constant_evaluated
make_shared
starts_with/ends_with for strings

associative container: contains similar to count

bit_cast similar to reinterpret.

mid_point: calculate the mid of two integers (without overflow)
to_array: convert to array

# c++17

## language features
template argument deduction for class templates
Declaring non-type template parameters with auto
New rules for auto deduction from braced-init-list
constexpr lambda
Lambda capture this by value

stl:
variant
optional
any
string_view
invoke
apply
filesystem
byte
splicing map and sets

parallel algorithm: copy, find, sort ...

## c++14

### language features
binary literals
generiic lambda expressions

lambda capture initializers
return type deduction
decltype: 

stl:
chrono
compile time integer sequences
make_unique

# c++11
move semantics
rvalue references &&
variadic templates
initializer list
static_assert
auto
lambda
type alias
nullptr
constexpr
override
final: (cannot override in derrved class)
delete
range based for loops
thread
smart pointer
array

https://github.com/AnthonyCalandra/modern-cpp-features#concepts

















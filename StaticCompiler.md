Static Compiler
===============

JRuby plans to support a static AOT compiler in the future, which will produce compiled Java classes that are actually implemented with Ruby code behind the scenes. This document discusses possible ways this compiler will function.

Given the following simple method:

```ruby
class MyClass
  def foo(a, b); end
end
```
We would expect the compiler to produce the following code:

```java
public class MyClass extends IRubyObject {
  public Object foo(Object a, Object b) {
    Object result = callMethod("foo", JavaUtil.convertJavaToRuby(a), JavaUtil.convertJavaToRuby(b));
    return JavaUtil.convertRubyToJava(result);
  }
}
```

This is a simple place to start.

Questions, concerns
-------------------

* Should underscore_case methods be compiled as camelCase? configurable?
* We need to clearly specify what `JavaUtil` will do for all sorts of types.

Primary requirements
--------------------

* No modification of Ruby's grammar should be necessary; the code should be runnable in interpreted mode.
* To the greatest extent possible, support all of Ruby's grammar being compiled into a static set of classes and methods.
* Whether it must be a complete no-op or whether a small library of no-op functions would be needed in interpreted mode is up for debate.

### Specifying types

A few possible syntaxes:

```ruby
{[:String, :Integer] => :int}
def foo(a, b) ...

[:String, :Integer] << :int
def foo(a, b) ...

java_method {
  params String, int
  return int
}
def foo(a, b) ...

def foo(a, b) ...
...
java_method :foo, [:String, :Integer] => :int
```

Those with a method call would require some noop library to run correctly in interpreted mode.

### Annotations

A few possible syntaxes

```ruby
@MyAnno = {:baz => "blah"}
def foo() ...

annotate { MyAnno(baz = "blah") }
def foo() ...

def foo() ...
...
annotate :foo { MyAnno(bar = "blah") }
```

### Attributes

It's mostly a no-brainer that attributes should get compiled into Java get/set methods.

The following class
```ruby
class Blah
  attr_accessor :foo
end
```

should produce the following in the resulting Java class:
```java
public Object getFoo();
public void setFoo(Object foo);
```
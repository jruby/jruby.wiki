Sometimes it's not enough to be able to call Java classes from Ruby, embed Ruby via a scripting API, or implement Java interfaces or extend Java classes at runtime. There are many APIs and services that require you to have a real Java .class file on disk, callable from Java code, visible to Java compilers, and configurable in various types of configuration files. To participate in these services, you need a "real" Java class.

Starting with JRuby 1.5, the jrubyc command now has two new flags: --java and --javac. The --java flag generates .java source from Ruby scripts that have classes in them. --javac does that and also compiles the sources for you.

Let's take a simple example class:

```ruby
 class Foo
   def bar(a, b)
     puts a + b
   end
 end
```

If you wanted to expose this as a Java class, you can just run jrubyc --java(c) to produce a class

```bash
    ~/jrubyc_demo ➔ jrubyc --javac my_foo.rb 
    Generating Java class Foo to Foo.java
    javac -d /Users/headius/jrubyc_demo -cp /Users/headius/projects/jruby/lib/jruby.jar:. Foo.java
```

You see here that jrubyc generates a .java file and then compiles it with the 'javac' command. Here's what the resulting class looks like, using javap:

```bash
    ~/jrubyc_demo ➔ javap Foo
    Compiled from "Foo.java"
```
```java
    public class Foo extends org.jruby.RubyObject{
        public Foo();
        public java.lang.Object bar(java.lang.Object, java.lang.Object);
        static {};
    }
```

Normally, jrubyc --java(c) will include the complete source of your script in the compiled class, but often you may want to keep the source on disk or generate multiple classes that all come from the same file. To do this, add a java_require line (plus a require 'java' line to enable Java support in the Ruby code) that specifies what filename to load. The filename specified is loaded using normal Ruby 'require' semantics.

```ruby
 require 'java'
 java_require 'my_foo'
 class Foo
   def bar(a,b)
     puts a + b
   end
 end
```

You'll notice that the "bar" method was generated with Object arguments and an Object return type, since no types were specified (this is Ruby, after all). But what if you want a specific Java signature? It's easy, just add a java_signature line:

```ruby
 require 'java'
 java_require 'my_foo'
 class Foo
   java_signature 'void bar(int, int)'
   def bar(a,b)
     puts a + b
   end
 end
```

Now the bar method gets generated with int arguments and a void return type.

Static methods can be done basically like you'd do them in Ruby. Let's make bar static:

```ruby
 require 'java'
 java_require 'my_foo'
 class Foo
   java_signature 'void bar(int, int)'
   def self.bar(a,b)
     puts a + b
   end
 end
```

And now add a "main" method:

```ruby
 require 'java'
 java_require 'my_foo'
 class Foo
   java_signature 'void main(String[])'
   def self.main(args)
     bar(args[0], args[1])
   end
 
   java_signature 'void bar(int, int)'
   def self.bar(a,b)
     puts a + b
   end
 end
```

And now it's time to try running our little Java-class-written-in-Ruby:

```bash
 ~/jrubyc_demo ➔ java Foo 15 75
 1575
```

Woah, wait a minute. What happened here?

The answer is actually pretty simple. The "main" method takes an array of String, so the arguments here become the strings "15", and "75". Since "main" is implemented in Ruby, and calls "bar", which is also implemented in Ruby, those strings are passed on to the "bar" method...as Strings. The "bar" method is then happy to add them, which in Ruby means concatenation.

This example illustrates that while you can generate Java classes with all the usual static signatures, your Ruby code remains the same dynamic Ruby you've grown to love. But to close out this example, we'll modify "bar" to convert its arguments to Fixnums before adding them, to give us the numeric result:

```ruby
 require 'java'
 java_require 'my_foo'
 class Foo
   java_signature 'void main(String[])'
   def self.main(args)
     bar(args[0], args[1])
   end
 
   java_signature 'void bar(int, int)'
   def self.bar(a,b)
     puts a.to_i + b.to_i
   end
 end
```

And now the result is as expected:

```bash
 ~/jrubyc_demo ➔ java Foo 15 75
 90
```

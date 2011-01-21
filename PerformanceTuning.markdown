Performance Tuning
==================
JRuby supports a number of options to help you tune performance. They range from turning on experimental features to turning off inefficient emulations of Ruby features.This document describes these options and their effects.<br/><br/>

Tuning the compiler
-------------------
JRuby's compiler can be enabled in JIT mode or specified to run before execution.

* See [[JRuby Compiler|JRubyCompiler]] for more details.
* See below for [Compiler Runtime Properties](#compiler_rt_props).
* See below for [JIT Runtime Properties](#jit_rt_props).

Disabling ObjectSpace
---------------------
ObjectSpace has been disabled by default since version 1.1b1. To reenable ObjectSpace, use the `+O` flag. The following description applies tor previous versions of JRuby.

ObjectSpace is a feature in Ruby that allows you to enumerate all objects of a given type in the current runtime. In the C implementation, this is easy to provide, since ObjectSpace is basically a thin wrapper around the memory manager. Under JRuby, however, where we can't enumerate objects managed by Java's memory model, we have to provide a separate structure that governs a collection of references to such objects. This results in substantial overhead per object when run under JRuby, since we have to create two or three times as many objects as are needed to run, just to implement ObjectSpace.

JRuby can be told to run without ObjectSpace by specifying the `-O` flag as follows:

 jruby -O bin/gem install rake

Obviously, any programs that depend on ObjectSpace will not run correctly with the `-O` flag, but this is generally limited to a few of Rails' own development-time scripts and test unit test runners.

There is also a property you can use to disable ObjectSpace:

 jruby -J-Djruby.objectspace.enabled=false

Enabling ObjectSpace Dynamically
--------------------------------
A program that knows it needs full ObjectSpace support can dynamically turn ObjectSpace on and off itself. For example,  jirb uses this feature to turn on ObjectSpace so it can have fully functional code completion. To enable ObjectSpace support in a ruby program, add the following code:

  require 'jruby'
  JRuby.objectspace=true

Enabling Thread Pooling
-----------------------
Ruby scripts frequently take advantage of the C implementation's lightweight (green) threading by spinning up hundreds or thousands of threads during a run. Under JRuby, this can often mean that hundreds or thousands of native threads are spun up and thrown away, which is inefficient in many cases and on many platforms. 

As an alternative to straight-up 1:1 threading in JRuby, you can enable ''thread pooling''. When pooling is enabled, JRuby starts only as many threads as needed for concurrent tasks. Repeatedly launching short-lived Ruby threads then reuses native threads. This can improve performance in many cases, especially when libraries like `timeout` are employed that spin up a thread per call.

To enable thread pooling, set the Java system property `jruby.thread.pooling` to `true`:

 jruby -J-Djruby.thread.pooling=true myscript.rb

For a list of all the thread pooling parameters, see [Thread Pooling Runtime Properties](#thread_pool_rt_props).

Using JRuby's Fast Mode
-----------------------
JRuby 1.2 and later ships with a `--fast` flag that turns on a number of runtime features to provide optimal performance without breaking Ruby features needed by common applications. It enables the following modes:

* '''Frameless compilation:''' Avoids using heap-based frames to track cross-call data like `backref`, `lastline`, and `visibility` when it's not needed.
* '''Fast math operations on Fixnum:''' When the target of a binary operator (`+`, `-`, and so on) is `Fixnum`, dispatches directly rather than through JRuby's dynamic call logic.
* '''Positionless compilation:'''  To avoid introducing overhead, uses Java's stack tracing mechanisms rather than JRuby's artificial mechanisms.
* '''Precompile all code''' Because we no longer maintain artificial heap frames and artificial traces, code must be compiled before execution to use Java's backtrace logic. This can impact startup time.
* '''Fast `__send__`:'''  When calling `__send__` with a literal symbol, compiles it as a call to the method named by the symbol instead. Useful for meta-programmed logic looking to do a visibility-ignoring call but trying to avoid the `__send__` overhead.

To make these settings ''safe'', a few assumptions are made:

* Core methods that require access to the caller's frame must not be aliased to other names. This includes methods like `eval` and `binding`, several `String`, `Regexp`, and IO methods that access `$~` and `$_`, any methods that mutate visibility, and so on. In general, aliasing these methods is only useful if you intend to wrap them with a new piece of code, which breaks the frame-based logic for callers anyway.
* Rigid adherence to Ruby's backtrace format and content must not be a requirement. Backtraces will generally include both Ruby and Java calls, and are formatted somewhat differently. The information is still there, but it can be a bit trickier to interpret. This will be cleaned up in future revisions.
* Because all code is compiled before execution, it should be expected that startup performance degrades when using `--fast`. 

Java Virtual Machine (JVM) Settings
-----------------------------------
Except for the JRuby convenience parameter `--server`, all JVM runtime parameters use the `-J` option, followed by the specific JVM setting. For example:

* Heap space settings: `jruby -J-X&lt;heap-space-setting&gt;`
* JRuby runtime settings: `jruby -J-D&lt;runtime-setting&gt;`

All the settings described in the following sections are JVM settings.

### Using the Java Server Virtual Machine
JRuby benefits greatly from running under the Java '''Server''' VM, which trades startup and early run performance for a much higher level of long term optimization. JRuby will use the server VM of whatever Java version you're running if you use the `--server` parameter, which passes the `-server` flag to the underlying JVM. For example:

 jruby --server myscript.rb

Combining use of the server VM with using the compiler and disabling ObjectSpace generally results in the fastest performance.

'''Note:''' The `--server` parameter is a convenient shorthand for the JVM parameter  `-J-server`.

### Setting Heap Space Parameters for JRuby
* Maximum heap space:

    jruby -J-Xmx512m

* Initial heap space:

    jruby -J-Xms512m

* Heap space for Young/Eden Garbage Collection:

    jruby -J-Xmn128m

**All together now**

First some suggestions:
* Set the minimum `-Xms` and maximum `-Xmx` heap sizes to the same value.
* Set the `-Xmn` value lower than the `-Xmx` value.

    jruby -J-Xmn512m -J-Xms2048m -J-Xmx2048m -J-server

### Setting JRuby Runtime Properties
To see all the Java system runtime properties for JRuby, enter the following command in the Command window or Terminal window:
 jruby --properties

All these properties can be used to alter runtime behavior for performance or compatibility. Specify them by passing `-J-Dproperty=value` on the command line. For example:

    jruby -J-Djruby.thread.pooling=true myscript.rb

<a name="compiler_rt_props"/>
### Compiler Runtime Properties
JRuby 1.3.1 properties. Specify these properties by passing `-J-Dproperty=value` on the command line.

    jruby.compile.mode=JIT|FORCE|OFF
       Set compilation mode. JIT is default; FORCE compiles all, OFF disables.
    jruby.compile.fastest=true|false
       (EXPERIMENTAL) Turn on all experimental compiler optimizations.
    jruby.compile.frameless=true|false
       (EXPERIMENTAL) Turn on frameless compilation where possible.
    jruby.compile.positionless=true|false
       (EXPERIMENTAL) Turn on compilation that avoids updating Ruby position info.
       Default is false
    jruby.compile.threadless=true|false
       (EXPERIMENTAL) Turn on compilation without polling for "unsafe" thread events. 
       Default is false.
    jruby.compile.fastops=true|false
       (EXPERIMENTAL) Turn on fast operators for Fixnum. Default is false.
    jruby.compile.fastcase=true|false
       (EXPERIMENTAL) Turn on fast case/when for all-Fixnum whens. Default is false.
    jruby.compile.chainsize=<line count>
       Set the number of lines at which compiled bodies are "chained". Default is 500.
    jruby.compile.lazyHandles=true|false
       Generate method bindings (handles) for compiled methods lazily. Default is false.
    jruby.compile.peephole=true|false
       Enable or disable peephole optimizations. Default is true (on).

<a name="jit_rt_props"/>
### JIT Runtime Properties
JRuby 1.3.1 properties. Specify these properties by passing `-J-Dproperty=value` on the command line.

    jruby.jit.threshold=<invocation count>
       Set the JIT threshold to the specified method invocation count. Default is 50.
    jruby.jit.max=<method count>
       Set the max count of active methods eligible for JIT-compilation.
       Default is 4096 per runtime. A value of 0 disables JIT, -1 disables max.
    jruby.jit.maxsize=<jitted method size (full .class)>
       Set the maximum full-class byte size allowed for jitted methods. 
       Default is Integer.MAX_VALUE.
    jruby.jit.logging=true|false
       Enable JIT logging (reports successful compilation). Default is false.
    jruby.jit.logging.verbose=true|false
       Enable verbose JIT logging (reports failed compilation). Default is false.
    jruby.jit.logEvery=<method count>
       Log a message every n methods JIT compiled. Default is 0 (off).
    jruby.jit.exclude=<ClsOrMod,ClsOrMod::method_name,-::method_name>
       Exclude methods from JIT by class/module short name, c/m::method_name,
       or -::method_name for anon/singleton classes/modules. Comma-delimited.

### Native Support Runtime Properties
JRuby 1.3.1 properties. Specify these properties by passing `-J-Dproperty=value` on the command line.

    jruby.native.enabled=true|false
       Enable/disable native extensions (like JNA for non-Java APIs). Default is true.
       (This affects all JRuby instances in a given JVM.)
    jruby.native.verbose=true|false
       Enable verbose logging of native extension loading. Default is false.
    jruby.fork.enabled=true|false
       (EXPERIMENTAL, maybe dangerous) Enable fork(2) on platforms that support it.

<a name="thread_pool_rt_props"/>
### Thread Pooling Runtime Properties
JRuby 1.3.1 properties. Specify these properties by passing `-J-Dproperty=value` on the command line.

    jruby.thread.pool.enabled=true|false
       Enable reuse of native backing threads via a thread pool. Default is false.
    jruby.thread.pool.min=<min thread count>
       The minimum number of threads to keep alive in the pool. Default is 0.
    jruby.thread.pool.max=<max thread count>
       The maximum number of threads to allow in the pool. Default is unlimited.
    jruby.thread.pool.ttl=<time to live, in seconds>
       The maximum number of seconds to keep alive an idle thread. Default is 60.

### Miscellaneous Runtime Properties
JRuby 1.3.1 properties. Specify these properties by passing `-J-Dproperty=value` on the command line.

    jruby.compat.version=RUBY1_8|RUBY1_9
       Specify the major Ruby version to be compatible with; Default is RUBY1_8.
    jruby.objectspace.enabled=true|false
       Enable or disable ObjectSpace.each_object. Default is disabled (false).
    jruby.launch.inproc=true|false
       Set in-process launching of e.g. system('ruby ...'). Default is true
    jruby.bytecode.version=1.5|1.6
       Set bytecode version for JRuby to generate. Default is current JVM version.
    jruby.management.enabled=true|false
       Set whether JMX management is enabled. Default is true.
    jruby.debug.fullTrace=true|false
       Set whether full traces are enabled (c-call/c-return). Default is false.

You can see some GC tunable parameters [here](http://blade.nagaokaut.ac.jp/cgi-bin/scat.rb/ruby/ruby-core/27550).
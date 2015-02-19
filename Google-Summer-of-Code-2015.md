
# Synopsis
This page hosts the ideas for Google Summer of Code 2015! Add your ideas here, improve others, and if you're a student, perhaps something on this list will interest you!

Ideas
=====

Note that there's also a [general "Ruby" GSoC ideas page](https://github.com/rubygsoc/rubygsoc/wiki/Ideas-List), many/most of which are relevant to JRuby.

### JRuby IR-based projects (Compilers, optimization, JVM bytecode)

JRuby currently has an intermediate representation (IR) that attempts to capture high-level Ruby semantics via instructions and operands. This IR will be the basis of an updated JRuby VM. There are lots of opportunities for improving on these and implementing additional optimizations. A student interested in interpreters, compilers, virtual machines would work with the JRuby team to expand on the capabilities of this VM -- projects could include new performance optimizations (offline or profile-guided runtime), implementing new backends (ex: Android VM or native compilation, other languages).

* Small optimizations: There are many small optimizations in the old (JRuby 1.7) JIT compiler that have not yet been added to the new IR runtime. Some examples: constant-time homoegeneous case/when, fast numeric operators, allocation reduction for unused arrays/hashes, etc. Most of these would be changes to the IR compiler or could be added to a post-compile pass.
* Inlining: We have an old working prototype of IR inlining, that can inline Ruby methods and blocks. A good summer project would be to get it updated and working again in both the interpreter and the JIT.
* Gradual and implicit typing: We also have a working prototype of a numeric unboxing pass, which optimistically converts dynamic Fixnum and Float operations into low-level long and double operations. This project would be to get the pass working and explore specializing code to more types.
* Prototype type specialization using Nashorn, vmboiler, or other projects: Nashorn is an invokedynamic-based JavaScript that has a very rich JVM bytecode specialization framework. They are working to make it live indepedently of Nashorn, so we can use it for JRuby. vmboiler is a similar framework that lives atop the ASM bytecode library and makes it possible to generate specialized logic with fallback to general types. This project would be to explore these libraries and attempt to wire them into JRuby's JIT.

### Truffle

JRuby has a backend to use the Truffle language implementation framework from Oracle Labs. There are multiple projects to improve the implementation of Ruby using Truffle, or to develop new tools using Truffle. Students would need to have some experience in language development and would need to learn enough about Truffle to submit their own proposal. Contacts are chrisseaton, eregon and nirvdrum.

### Improving Ruby's concurrency features ###

* concurrent-ruby: Many contributors have been working on the concurrent-ruby library, a collection of concurrency utilities like futures, actors, and atomic references. There's always more to add and improvements to be made to the existing features. This project would involve testing, benchmarking, and studying other threading libraries to continue improving the concurrent-ruby toolbox. It could also include working with ruby-core (MRI) developers to push some low-level features emulated by the library into Ruby's standard features.

* Concurrency improvement of standard libraries and popular gems: The existing Ruby standard libraries have never received a good thread-safety analysis, and most thread-safety testing has only happened via other libraries and applications. This project would work down the list of standard libraries and do hand-analysis, tool-driven analysis, purpose-built concurrency tests, and changes necessary to be usable in highly-concurrent environments. Given time, this could also expand into popular gems.

### Utilizing new JVM features ###

* Java 8 lambdas and streams: Java 8 included features for lambdas (Java-based closures) and streaming closure-based algorithms transparently parallelized across threads using fork/join. A student could explore how best to expose Java 8's collections and streams APIs to Ruby and how best to leverage Ruby's libraries and features on a Java 8 platform.

* Java 9 modularity (jigsaw): Project Jigsaw will bring modularity to the JVM. This includes the ability to package a Java application with only the JVM/JDK bits it needs, pre-compile a Java application into some binary format (ideally speeding startup and warmup), and interesting opportunities for JRuby's Java integration (isolation of subsystems, modularity support within JRuby itself). This project would explore how Jigsaw could be leveraged in JRuby.

* Azul's Zing VM: The Zing VM is an OpenJDK-based JVM that includes various compiler and GC optimizations not found in the stock codebase. It is a commercial VM, but free copies are available for testing and exploration in development and on open-source projects. This project would be to explore Zing's unique features on JRuby and prepare a report examining performance, garbage collection, and tooling opportunities for Ruby users running JRuby on Zing.

* Oracle's Graal VM: In addition to enabling the Truffle language framework, the 100%-Java Graal compiler may provide a better VM going forward (e.g. optimizations missing in Hotspot), an interesting target for our IR (skipping JVM bytecode and going straight to JIT), and other useful features for optimizing Ruby code and applications.

### Improving JRuby Tools ###

There are many tools for the JVM and a few for Ruby. We can do better...we need profilers (performance and memory), leak detectors, thread-safety analyzers, system analyzers. Many possible projects here:

* Visualize code or program execution: This could be accomplished by instrumenting our new IR runtime or by leveraging JVM tools, but with the new IR runtime we also have the ability to gather type and branch profiles, full-system call maps, and more.

* Analyze and profile memory use. This would involve improving or enhancing tools like JVisualVM, jhat, [Alienist](https://github.com/enebo/alienist) to make memory profiling, leak detection, object race detection and more possible on JRuby.

* Better utilization of JVM security subsystem: Many unsecure environments use JRuby because of the JVM security model, which is difficult to provide in C Ruby. However JRuby does not take good advantage of this subsystem by providing its own permissions and configuation. Improve JRuby's integation with the JVM security model.

* Startup mitigation projects: We have been promoting Drip as a way to mitigate JRuby's startup, but it's only a half measure. This project would work to analyze exactly what is slow during JRuby's startup (and warmup, perhaps) and explore tools like Drip, Nailgun, Project Jigsaw, alternative JVMs, and others (along with JRuby improvements) to make JRuby a more useful command-line tool.

### Better Android and Mobile Support ###

JRuby has worked on Android for a long time, but we've never been happy with the performance, runtime size, and level of integration with the rest of the Android platform. There are multiple possible projects here.

* JRuby 9000 on Android: Explore what we can do to shrink the JRuby runtime (now that it only supports one language mode) and improve performance and startup time of JRuby 9000 on Android.

* The Android Runtime: Android 5+ replaces the old Dalvik VM with the new "ART" (Android Runtime) VM that compiles code to native when it is installed. If we could leverage this better in JRuby, it could lead to smaller applications and better performance and startupt time. Explore how ART precompiles Dalvik bytecode and how we can do a better job of leveraging that in JRuby.

* iOS support via RoboVM: RoboVM is a project to make JVM-based apps run on iOS by including a runtime and precompiling as much as possible to native. This has great potential for JRuby, since we are a full-featured Ruby implementation...we could bring all Ruby libraries out there to iOS developers. This project would explore getting JRuby to run on RoboVM and study how effective it would be to write Ruby-based applications on iOS this way.

### Celluloid "Turbo Mode" for JRuby

[Celluloid](http://celluloid.io) is an actor-based concurrent object framework (somewhat similar to Akka) written in pure Ruby. It presently uses Ruby Mutexes and ConditionVariables for synchronization. However, the JVM has many, many other options which could provide better performance.

Celluloid provides an [ActorSystem](https://github.com/celluloid/celluloid/blob/master/lib/celluloid/actor_system.rb) abstraction for supporting multiple different platform-specific backends, and we'd love to have one specific to JRuby.

The goal of this project would be to implement a Celluloid `ActorSystem` which is a better fit with JRuby. Some options to consider:

* ***[LMAX Disruptor](http://lmax-exchange.github.io/disruptor/)***: Disruptor is a library which supports a number of different patterns for multithreaded execution. [Some work has already been done to implement Celluloid Mailboxes in terms of Disruptor](https://github.com/celluloid/celluloid/issues/342)
* ***[ArrayBlockingQueue](http://docs.oracle.com/javase/6/docs/api/java/util/concurrent/ArrayBlockingQueue.html)***: These are fast, fixed-sized data structures built atop arrays.
* ***[LinkedTransferQueue](http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/LinkedTransferQueue.html)***: Introduced in Java 7, LinkedTransferQueues are one of the most adaptable concurrency primitives available on the JVM today.
* ***[Fork/Join](http://docs.oracle.com/javase/tutorial/essential/concurrency/forkjoin.html)***: a framework introduced in Java 7 for abstracting multicore execution on the JVM

### Ports of popular C extensions ###

Many Ruby libraries are only available as C extensions, and as a result they're not usable on JRuby. The more of these libraries we have ports for, the less pain JRuby users suffer during migration.

This list is not all-inclusive, but these are some C extension-only gems that are in common use and which represent frequent migration stumbling blocks:

* https://github.com/brianmario/mysql2 - MySQL bindings.
* https://github.com/taf2/curb - A libcurl wrapper. Could be redone in FFI or as an API-compatible wrapper around a Java HTTP client.
* https://github.com/ohler55/oj - A very fast JSON parser. The standard 'json' library uses a Ragel-generated parser that's not as fast as it could be.
* https://github.com/nixme/pry-debugger/issues/26 - pry-debugger. Step in/over debugging with Pry.

### Rsense

Rsense is an awesome type-inference tool for ruby source code, built on top of JRuby-Parser.  A lot of work went into it as part of a previous GSoC, and for small projects its working.  Take it to the next level and implement caching so that it works with those mono-rail projects so many of us work with. Build the Vim and Emacs plugins. Make awesome tooling on top of it.  The sky is the limit.

### Native coroutine support

Implement some kind of native coroutine support for JRuby. Bonus points for an implementation which is compatible with the Fiber API. One potential approach:

The [Continuations Library](http://www.matthiasmann.de/content/view/24/26/) by Matthias Mann provides the basis for lightweight coroutines on the JVM used by the [Pulsar](https://github.com/puniverse/pulsar) and [Quasar](https://github.com/puniverse/quasar) libraries to achieve feats like [10,000 actors on the JVM](http://blog.paralleluniverse.co/post/64210769930/spaceships2).

It would be great if this library could be leveraged from JRuby, either with a proprietary API, or with an implementation of Fibers which is backed by this library.

Note that this project has run into challenges before due to these frameworks' requirement of static typing throughout the coroutine call path. This complicates things for us because of Ruby's dynamic nature.

### Java ByteBuffer support for nio4r

Ruby has no native ByteArray or ByteBuffer types, [unlike Java](http://docs.oracle.com/javase/7/docs/api/java/nio/ByteBuffer.html). While it'd be great to have something like this in core Ruby, the next best place would be in the [New IO for Ruby](https://github.com/celluloid/nio4r) project, which provides a thin wrapper around Java NIO.

The goal of this project would be to wrap Java ByteBuffers (particularly direct ByteBuffers) in a Ruby class that can also be implemented in pure Ruby which hooks into nio4r and can be used directly with nio4r's Java NIO backend.
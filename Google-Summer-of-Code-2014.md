This page hosts the ideas for Google Summer of Code 2014! Add your ideas here, improve others, and if you're a student, perhaps something on this list will interest you!

Ideas
=====

### Native coroutine support

Implement some kind of native coroutine support for JRuby. Bonus points for an implementation which is compatible with the Fiber API. One potential approach:

The [Continuations Library](http://www.matthiasmann.de/content/view/24/26/) by Matthias Mann provides the basis for lightweight coroutines on the JVM used by the [Pulsar](https://github.com/puniverse/pulsar) and [Quasar](https://github.com/puniverse/quasar) libraries to achieve feats like [10,000 actors on the JVM](http://blog.paralleluniverse.co/post/64210769930/spaceships2).

It would be great if this library could be leveraged from JRuby, either with a proprietary API, or with an implementation of Fibers which is backed by this library.

### Java ByteBuffer support for nio4r

Ruby has no native ByteArray or ByteBuffer types, [unlike Java](http://docs.oracle.com/javase/7/docs/api/java/nio/ByteBuffer.html). While it'd be great to have something like this in core Ruby, the next best place would be in the [New IO for Ruby](https://github.com/celluloid/nio4r) project, which provides a thin wrapper around Java NIO.

The goal of this project would be to wrap Java ByteBuffers (particularly direct ByteBuffers) in a Ruby class that can also be implemented in pure Ruby which hooks into nio4r and can be used directly with nio4r's Java NIO backend.

### Celluloid "Turbo Mode" for JRuby

[Celluloid](http://celluloid.io) is an actor-based concurrent object framework (somewhat similar to Akka) written in pure Ruby. It presently uses Ruby Mutexes and ConditionVariables for synchronization. However, the JVM has many, many other options which could provide better performance. Celluloid provides an [ActorSystem](https://github.com/celluloid/celluloid/blob/master/lib/celluloid/actor_system.rb) abstraction for supporting multiple different platform-specific backends, and we'd love to have one specific to JRuby.

The goal of this project would be to implement a duck type of the `Celluloid::Mailbox` class that leverages native JVM facilities to improve performance. Some options to consider:

* ***[LMAX Disruptor](http://lmax-exchange.github.io/disruptor/)***: Disruptor is a library which supports a number of different patterns for multithreaded execution. [Some work has already been done to implement Celluloid Mailboxes in terms of Disruptor](https://github.com/celluloid/celluloid/issues/342)
* ***[ArrayBlockingQueue](http://docs.oracle.com/javase/6/docs/api/java/util/concurrent/ArrayBlockingQueue.html)***: These are fast, fixed-sized data structures built atop arrays.
* ***[LinkedTransferQueue](http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/LinkedTransferQueue.html)***: Introduced in Java 7, LinkedTransferQueues are one of the most adaptable concurrency primitives available on the JVM today.

### Truffle

JRuby now has a work-in-progress [Truffle] backend, to use two powerful new JVM technologies - the Truffle AST interpreter framework, and the new Graal JIT compiler. Together these are achieving peak performance well beyond anything currently possible with the JVM.

There is plenty of low hanging fruit in the Truffle backend to make good GSoC projects, and you will be working on genuinely research-level technology that may be the future of all JVM languages. The team working on Truffle are enthusiastic about mentoring and we can help an enthusiastic person get going right now if they want, so they have their own ideas and a track record when it comes to GSoC.

Talk to chrisseaton.
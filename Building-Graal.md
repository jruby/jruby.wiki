JRuby+Truffle is designed to be run with a JVM that has the Graal compiler. The easiest way to get this is by downloading the [GraalVM](Downloading GraalVM), but for some special cases you may want to build Graal yourself.

Follow the instructions in the Graal core repository.

https://github.com/graalvm/graal-core

The easiest way to run with the version of Graal that you've just build is using the `jt` tool and the `GRAAL_HOME` environment variable.

```
$ GRAAL_HOME=..../graal-core jt run --graal ...normal Ruby arguments here...
```

You can also use the same command line instruction as if you were using a (JDK 9 EA build)[Using Graal in JDK 9 EA Builds].

Some more help on building on different distributions of Linux is available at http://mail.openjdk.java.net/pipermail/graal-dev/2015-December/004050.html.
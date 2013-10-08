Configuring JRuby
=================

As of version 1.6.5, JRuby now supports a .jrubyrc file in either the current directory or the user's home directory. The format of this file is a series of key/value pairs, with the key being the property name from ```--properties``` and the value being the value you'd pass on the command line for the same property.

For example, to set the default Ruby version mode to 1.9, you'd use the following line:

```
compat.version=1.9
```

A configuration file that sets 1.9 mode, disables C extensions, and generates full backtraces for the EAGAIN errno would look like:

```
compat.version=1.9
cext.enabled=false
errno.backtrace=true
```

Most properties should be documented in the ```jruby --properties``` output, but for a complete listing look at src/org/jruby/RubyInstanceConfig.java.

A listing from JRuby 1.7.5 follows. Some of these are not available on JRuby 1.6.x.

```
# These properties can be used to alter runtime behavior for perf or compatibility.
# Specify them by passing -X<property>=<value>
#   or if passing directly to Java, -Djruby.<property>=<value>
#   or put <property>=<value> in .jrubyrc
#
# This dump is a valid .jrubyrc file of current settings. Uncomment and modify
# settings to customize.

################################################################################
# compiler
################################################################################

# Set the number of lines at which compiled bodies are "chained".
# Options: [Integer], Default: 500.

#compile.chainsize=500

# Dump to console all bytecode generated at runtime.
# Options: [true, false], Default: false.

#compile.dump=false

# Return true from multiple assignment instead of a new array.
# Options: [true, false], Default: false.

#compile.fastMasgn=false

# Compile with all "mostly harmless" compiler optimizations.
# Options: [true, false], Default: false.

#compile.fastest=false

# Turn on fast operators for Fixnum and Float.
# Options: [true, false], Default: true.

#compile.fastops=true

# Compile obj.__send__(<literal>, ...) as obj.<literal>(...).
# Options: [true, false], Default: false.

#compile.fastsend=false

# Use invokedynamic for optimizing Ruby code
# Options: [true, false], Default: false.

#compile.invokedynamic=false

# Generate method bindings (handles) for compiled methods lazily.
# Options: [true, false], Default: false.

#compile.lazyHandles=false

# Set compilation mode. JIT = at runtime; FORCE = before execution.
# Options: [JIT, FORCE, OFF, OFFIR], Default: JIT.

#compile.mode=JIT

# Compile calls without guards, for experimentation.
# Options: [true, false], Default: false.

#compile.noguards=false

# Enable or disable peephole optimizations.
# Options: [true, false], Default: true.

#compile.peephole=true

# (EXPERIMENTAL) Turn on compilation without polling for "unsafe" thread events.
# Options: [true, false], Default: false.

#compile.threadless=false


################################################################################
# invokedynamic
################################################################################

# Enable all possible uses of invokedynamic.
# Options: [true, false], Default: false.

#invokedynamic.all=false

# Use invokedynamic to load cached values like literals and constants.
# Options: [true, false], Default: true.

#invokedynamic.cache=true

# Use invokedynamic to load constants.
# Options: [true, false], Default: true.

#invokedynamic.cache.constants=true

# Use invokedynamic to get/set instance variables.
# Options: [true, false], Default: true.

#invokedynamic.cache.ivars=true

# Use invokedynamic to load literals.
# Options: [true, false], Default: true.

#invokedynamic.cache.literals=true

# Use ClassValue to store class-specific data.
# Options: [true, false], Default: false.

#invokedynamic.class.values=false

# Maximum global cache failures after which to use slow path.
# Options: [Integer], Default: 100.

#invokedynamic.global.maxfail=100

# Use MethodHandles rather than generated code to bind Ruby methods.
# Options: [true, false], Default: false.

#invokedynamic.handles=false

# Enable invokedynamic for method invocations.
# Options: [true, false], Default: true.

#invokedynamic.invocation=true

# Bind Ruby attribue invocations directly to invokedynamic.
# Options: [true, false], Default: true.

#invokedynamic.invocation.attr=true

# Bind Fixnum and Float math using optimized logic.
# Options: [true, false], Default: true.

#invokedynamic.invocation.fastops=true

# Also bind indirect method invokers to invokedynamic.
# Options: [true, false], Default: true.

#invokedynamic.invocation.indirect=true

# Bind Ruby to Java invocations with invokedynamic.
# Options: [true, false], Default: true.

#invokedynamic.invocation.java=true

# Use SwitchPoint for class modification guards on invocations.
# Options: [true, false], Default: true.

#invokedynamic.invocation.switchpoint=true

# Log binding of invokedynamic call sites.
# Options: [true, false], Default: false.

#invokedynamic.log.binding=false

# Log invokedynamic-based constant lookups.
# Options: [true, false], Default: false.

#invokedynamic.log.constants=false

# Log invokedynamic-based global lookups.
# Options: [true, false], Default: false.

#invokedynamic.log.globals=false

# Maximum call site failures after which to inline cache.
# Options: [Integer], Default: 1000.

#invokedynamic.maxfail=1000

# Maximum polymorphism of PIC binding.
# Options: [Integer], Default: 6.

#invokedynamic.maxpoly=6

# Enable all safe (but maybe not fast) uses of invokedynamic.
# Options: [true, false], Default: false.

#invokedynamic.safe=false


################################################################################
# jit
################################################################################

# Run the JIT compiler in a background thread.
# Options: [true, false], Default: true.

#jit.background=true

# Cache jitted method in-memory bodies across runtimes and loads.
# Options: [true, false], Default: true.

#jit.cache=true

# Save jitted methods to <dir> as they're compiled, for future runs.
# Options: [dir], Default: null.

#jit.codeCache=

# Log loading of JITed bytecode.
# Options: [true, false], Default: false.

#jit.debug=false

# Enable stdout dumping of JITed bytecode.
# Options: [true, false], Default: false.

#jit.dumping=false

# Exclude methods from JIT. Comma delimited.
# Options: [ClsOrMod, ClsOrMod::method_name, -::method_name], Default: .

#jit.exclude=

# Log a message every n methods JIT compiled.
# Options: [Integer], Default: 0.

#jit.logEvery=0

# Enable JIT logging (reports successful compilation).
# Options: [true, false], Default: false.

#jit.logging=false

# Enable verbose JIT logging (reports failed compilation).
# Options: [true, false], Default: false.

#jit.logging.verbose=false

# Set the max count of active methods eligible for JIT-compilation.
# Options: [Integer], Default: 4096.

#jit.max=4096

# Set the maximum full-class byte size allowed for jitted methods.
# Options: [Integer], Default: 30000.

#jit.maxsize=30000

# Set the JIT threshold to the specified method invocation count.
# Options: [Integer], Default: 50.

#jit.threshold=50


################################################################################
# intermediate representation
################################################################################

# Debug compilation of JRuby IR.
# Options: [true, false], Default: false.

#ir.compiler.debug=false

# Debug generation of JRuby IR.
# Options: [true, false], Default: false.

#ir.debug=false

# Specify comma delimeted list of passes to run after inlining a method.
# Options: [String], Default: null.

#ir.inline_passes=

# Specify comma delimeted list of passes to run.
# Options: [String], Default: null.

#ir.passes=

# [EXPT]: Profile IR code during interpretation.
# Options: [true, false], Default: false.

#ir.profile=false

# Visualization of JRuby IR.
# Options: [true, false], Default: false.

#ir.visualizer=false


################################################################################
# native
################################################################################

# Enable or disable C extension support.
# Options: [true, false], Default: false.

#cext.enabled=false

# Dump bytecode-generated FFI stubs to console.
# Options: [true, false], Default: false.

#ffi.compile.dump=false

# Use invokedynamic to bind FFI invocations.
# Options: [true, false], Default: false.

#ffi.compile.invokedynamic=false

# Reify FFI compiled classes.
# Options: [true, false], Default: false.

#ffi.compile.reify=false

# Number of FFI invocations before generating a bytecode stub.
# Options: [Integer], Default: 100.

#ffi.compile.threshold=100

# Enable/disable native code, including POSIX features and C exts.
# Options: [true, false], Default: true.

#native.enabled=true

# Enable verbose logging of native extension loading.
# Options: [true, false], Default: false.

#native.verbose=false


################################################################################
# thread pooling
################################################################################

# Enable reuse of native threads via a thread pool.
# Options: [true, false], Default: false.

#thread.pool.enabled=false

# The maximum number of threads to allow in the pool.
# Options: [Integer], Default: 2147483647.

#thread.pool.max=2147483647

# The minimum number of threads to keep alive in the pool.
# Options: [Integer], Default: 0.

#thread.pool.min=0

# The maximum number of seconds to keep alive an idle thread.
# Options: [Integer], Default: 60.

#thread.pool.ttl=60

# The maximum number of threads to allow in the timeout pool.
# Options: [Integer], Default: 4.

#timeout.thread.pool.max=4


################################################################################
# miscellaneous
################################################################################

# Enable colorized backtraces.
# Options: [true, false], Default: false.

#backtrace.color=false

# Mask .java lines in Ruby backtraces.
# Options: [true, false], Default: false.

#backtrace.mask=false

# Set the style of exception backtraces.
# Options: [normal, raw, full, mri], Default: normal.

#backtrace.style=normal

# Specify the major Java bytecode version.
# Options: [1.5, 1.6, 1.7], Default: 1.7.

#bytecode.version=1.7

# Specify the major Ruby version to be compatible with.
# Options: [1.8, 1.9, 2.0], Default: 1.9.

#compat.version=1.9

# Generate consistent object hashes across JVMs
# Options: [true, false], Default: false.

#consistent.hashing=false

# Use lightweight Enumerator#next logic when possible.
# Options: [true, false], Default: true.

#enumerator.lightweight=true

# Use JVM coroutines for Fiber.
# Options: [true, false], Default: false.

#fiber.coroutines=false

# Use a single global lock for requires.
# Options: [true, false], Default: false.

#global.require.lock=false

# Make non-local flow jumps generate backtraces.
# Options: [true, false], Default: false.

#jump.backtrace=false

# Set in-process launching of e.g. system('ruby ...').
# Options: [true, false], Default: false.

#launch.inproc=false

# Set whether JMX management is enabled.
# Options: [true, false], Default: false.

#management.enabled=false

# Do a true process-obliterating native exec for Kernel#exec.
# Options: [true, false], Default: true.

#native.exec=true

# Use native impls for parts of net/protocol.
# Options: [true, false], Default: false.

#native.net.protocol=false

# Prefer IPv4 network stack
# Options: [true, false], Default: true.

#net.preferIPv4=true

# Enable or disable ObjectSpace.each_object.
# Options: [true, false], Default: false.

#objectspace.enabled=false

# Do not unwrap process streams (issue on some recent JVMs).
# Options: [true, false], Default: false.

#process.noUnwrap=false

# Use reflection for binding methods, not generated bytecode.
# Options: [true, false], Default: false.

#reflected.handles=false

# Before instantiation, stand up a real Java class for every Ruby class.
# Options: [true, false], Default: false.

#reify.classes=false

# Log errors during reification (reify.classes=true).
# Options: [true, false], Default: false.

#reify.logErrors=false

# Attempt to expand instance vars into Java fields
# Options: [true, false], Default: false.

#reify.variables=false

# Enable or disable SipHash for String hash function.
# Options: [true, false], Default: false.

#siphash.enabled=false

# Set the signal used for dumping thread stacks.
# Options: [USR1, USR2, etc], Default: USR2.

#thread.dump.signal=USR2


################################################################################
# debugging and logging
################################################################################

# Set whether full traces are enabled (c-call/c-return).
# Options: [true, false], Default: false.

#debug.fullTrace=false

# Log externally-launched processes.
# Options: [true, false], Default: false.

#debug.launch=false

# Log require/load file searches.
# Options: [true, false], Default: false.

#debug.loadService=false

# Log require/load parse+evaluate times.
# Options: [true, false], Default: false.

#debug.loadService.timing=false

# disables JRuby impl script loads and prints parse exceptions
# Options: [true, false], Default: false.

#debug.parser=false

# Print which script is executed by '-S' flag.
# Options: [true, false], Default: false.

#debug.scriptResolution=false

# Dump class + instance var names on first new of Object subclasses.
# Options: [true, false], Default: false.

#dump.variables=false

# Generate backtraces for heavily-used Errno exceptions (EAGAIN).
# Options: [true, false], Default: false.

#errno.backtrace=false

# Log every time an exception backtrace is generated.
# Options: [true, false], Default: false.

#log.backtraces=false

# Log every time a Kernel#caller backtrace is generated.
# Options: [true, false], Default: false.

#log.callers=false

# Log every time an exception is constructed.
# Options: [true, false], Default: false.

#log.exceptions=false

# Log every time a built-in warning backtrace is generated.
# Options: [true, false], Default: false.

#log.warnings=false

# Use specified class for logging.
# Options: [class name], Default: org.jruby.util.log.JavaUtilLoggingLogger.

#logger.class=org.jruby.util.log.JavaUtilLoggingLogger

# Generate backtraces for heavily-used Errno exceptions (EAGAIN).
# Options: [true, false], Default: false.

#stop_iteration.backtrace=false


################################################################################
# java integration
################################################################################

# Use java.lang.reflect.Proxy for interface impl.
# Options: [true, false], Default: false.

#interfaces.useProxy=false

# Use generated handles instead of reflection for calling Java.
# Options: [true, false], Default: false.

#java.handles=false

# Log whether setAccessible is working.
# Options: [true, false], Default: false.

#ji.logCanSetAccessible=false

# Extend Java classes without using a proxy object.
# Options: [true, false], Default: false.

#ji.newStyleExtension=false

# Cache Java object wrappers between calls.
# Options: [true, false], Default: true.

#ji.objectProxyCache=true

# Allow external envs to replace JI proxy class factory
# Options: [String], Default: null.

#ji.proxyClassFactory=

# Try to set inaccessible Java methods to be accessible.
# Options: [true, false], Default: true.

#ji.setAccessible=true

# Allow Capitalized Java pacakge names.
# Options: [true, false], Default: false.

#ji.upper.case.package.name.allowed=false


################################################################################
# profiling
################################################################################

# Maximum number of methods to consider for profiling.
# Options: [Integer], Default: 100000.

#profile.max.methods=100000


################################################################################
# command line options
################################################################################

# Wrap execution with a gets() loop. Same as -n.
# Options: [true, false], Default: false.

#cli.assume.loop=false

# Print $_ after each execution of script. Same as -p.
# Options: [true, false], Default: false.

#cli.assume.print=false

# Split $_ into $F for -p or -n. Same as -a.
# Options: [true, false], Default: false.

#cli.autosplit=false

# Set autosplit separator. Same as -F.
# Options: [String], Default: null.

#cli.autosplit.separator=

# Backup extension for in-place ARGV files. Same as -i.
# Options: [String], Default: null.

#cli.backup.extension=

# Print target script bytecode to stderr. Same as --bytecode.
# Options: [true, false], Default: false.

#cli.bytecode=false

# Check syntax of target script. Same as -c but runs script.
# Options: [true, false], Default: false.

#cli.check.syntax=false

# Print copyright to stderr. Same as --copyright but runs script.
# Options: [true, false], Default: false.

#cli.copyright=false

# Enable debug mode logging. Same as -d.
# Options: [true, false], Default: false.

#cli.debug=false

# Encoding name to treat external data.
# Options: [String], Default: null.

#cli.encoding.external=

# Encoding name to use internally.
# Options: [String], Default: null.

#cli.encoding.internal=

# Print command-line usage. Same as --help but runs script.
# Options: [true, false], Default: false.

#cli.help=false

# Set kcode character set. Same as -K (1.8).
# Options: [NIL, NONE, UTF8, SJIS, EUC], Default: NONE.

#cli.kcode=NONE

# Load a bundler Gemfile in cwd before running. Same as -G.
# Options: [true, false], Default: false.

#cli.load.gemfile=false

# Enable parser debug logging. Same as -y.
# Options: [true, false], Default: false.

#cli.parser.debug=false

# Enable line ending processing. Same as -l.
# Options: [true, false], Default: false.

#cli.process.line.ends=false

# Enable instrumented profiling modes.
# Options: [OFF, API, FLAT, GRAPH, HTML, JSON], Default: OFF.

#cli.profiling.mode=OFF

# Print config properties. Same as --properties but runs script.
# Options: [true, false], Default: false.

#cli.properties=false

# Default record separator.
# Options: [String], Default: "\n".

#cli.record.separator="\n"

# Enable/disable RubyGems.
# Options: [true, false], Default: true.

#cli.rubygems.enable=true

# Strip text before shebang in script. Same as -x.
# Options: [true, false], Default: false.

#cli.strip.header=false

# Verbose mode, as -w or -W2. Sets default for cli.warning.level.
# Options: [true, false], Default: false.

#cli.verbose=false

# Print version to stderr. Same as --version.
# Options: [true, false], Default: false.

#cli.version=false

# Warning level (off=0,normal=1,on=2). Same as -W.
# Options: [NIL, FALSE, TRUE], Default: FALSE.

#cli.warning.level=FALSE
```
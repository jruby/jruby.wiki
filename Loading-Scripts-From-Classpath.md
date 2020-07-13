In addition to the usual Ruby load path, JRuby also supports loading scripts from any jar file or system path added to the JVM's class path (or "classpath").

```
$ cat file.rb
puts "File is: #{__FILE__}"

$ jar cvf file.jar file.rb
added manifest
adding: file.rb(in = 14) (out= 16)(deflated -14%)

$ jruby -J-cp file.jar uri:classloader:/file.rb
File is: uri:classloader:/file.rb

$ jruby -e 'require "./file.jar"; require "uri:classloader:/file"'
File is: uri:classloader:/file.rb

$ jruby -e '$CLASSPATH << "./file.jar"; require "uri:classloader:/file.rb"'
uri:classloader:/file.rb

$ java -cp jruby-complete.jar:file.jar org.jruby.Main uri:classloader:/file.rb
File is: uri:classloader:/file.rb
```

Files in the classpath live within the "uri:classloader" URI namespace, with the entire namespace being treated as a second virtual filesystem root. Under most circumstances, you can `chdir` into these paths and JRuby will simulate running from that filesystem (process-launching and other system-level operations must still use the actual user current directory).

```
$ jruby -e 'require "./file.jar"; Dir.chdir "uri:classloader:/"; require "./file"'
File is: uri:classloader:/file.rb
```

This also affects how `require_relative` works, since a classpath resource lives under a separate hierarchy of path elements (note that we are not including `file.jar` here, so `file.rb` lives only in the normal system current directory):

```
$ cat relative.rb
require_relative 'file'

$ jar cvf relative.jar relative.rb
...

$ java -cp jruby-complete.jar:relative.jar org.jruby.Main uri:classloader:/relative.rb
LoadError: no such file to load -- uri:classloader:/file
           require at org/jruby/RubyKernel.java:974
  require_relative at org/jruby/RubyKernel.java:1002
            <main> at uri:classloader:/relative.rb:1
```

To allow `require_relative` to work from within the `uri:classloader` namespace, be sure to include the files you wish to load relatively on the classpath in some way:

```
$ java -cp lib/jruby-complete.jar:relative.jar:. org.jruby.Main uri:classloader:/relative.rb
File is: uri:classloader:/file.rb

$ java -cp lib/jruby-complete.jar:relative.jar:file.jar org.jruby.Main uri:classloader:/relative.rb
File is: uri:classloader:/file.rb
```
There is a Java-based JRuby implementation of the fast ruby debugger rdebug.

Run `jruby -S gem install ruby-debug`

Now use it:

You can call it programmatically within the script

```ruby
require 'ruby-debug'
debugger
```

and run jruby with the --debug flag:

```bash
jruby --debug my_script.rb
```

Starting the debugger on a Rails application:

```bash
cd my_rails_app
jruby --debug -S rdebug script/server
```

Starting the debugger to test a local ruby program and adding ''lib/'' to the load path:

```bash
ruby --debug -S rdebug -Ilib rubyprogram.rb 
```

just make sure to run it under a jruby with the --debug flags passed to it, or "next" will always act as if it were the "step" command.  And the command line prompt won't even appear.

External Resources:

* [trunk jruby-debug README](http://debug-commons.rubyforge.org/svn/jruby-debug/trunk/README)
* [Debugging with ruby-debug](http://bashdb.sourceforge.net/ruby-debug.html)
* [JRuby Debugger Troubleshooting - Forum Thread](http://www.intellij.net/forums/thread.jspa?messageID=5225735)

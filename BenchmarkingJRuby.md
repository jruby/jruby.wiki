Benchmarking JRuby
==================
The current popular way to benchmark JRuby performance is to perform a gem installation of Rake. The install process exercises a number of APIs and represents a fairly general-purpose application of Ruby. It's also extremely interpreter-heavy.

After getting a build of JRuby, as above, the following steps can be used to benchmark JRuby using Gem and Rake:

* Fetch the current Rake gem from [RubyForge](http://rubyforge.org/frs/?group_id=50)
* Execute the following command (putting `JRUBY_HOME/bin` in your path or referencing it directly):

    JRUBY_HOME/bin/gem install <rake gem file>

* By preceding this command line with the unix `time` command you can test end-to-end performance. Of course there are other ways to wire in profiling and performance-monitoring tools that won't be detailed here.

A sample run from a MacBook Pro under Apple's Java 6 JVM is shown below:

    Nutters-Computer:~/Documents/workspace/jruby headius$ time bin/gem install rake-0.7.1.gem 
    Successfully installed rake, version 0.7.1
    Installing ri documentation for rake-0.7.1...
    Installing RDoc documentation for rake-0.7.1...
    
    real    0m52.596s
    user    0m51.740s
    sys     0m2.146s
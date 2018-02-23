JRuby versions prior to 1.6 did not support Ruby C extensions, and even in 1.6 the support is still "in development" and considered experimental. As of 1.7, it has been disabled and will likely [be removed](https://twitter.com/headius/statuses/281091403919003649).

This page lists common C extensions and non-C alternatives you can use to replace them.

If you are interested in helping us port an extension to JRuby, this article is helpful: [Your first Ruby native extension: Java](https://blog.jcoglan.com/2012/08/02/your-first-ruby-native-extension-java/)

* **[RDiscount][]** - Use [kramdown][], [Maruku][] (pure Ruby) or [markdown_j][] (wrapper around a Java library)

* **[RedCarpet][]** - Same as with **RDiscount** use alternatives such as [kramdown][], [Maruku][] or [markdown_j][]

* **[RMagick][]** - Try [RMagick4J][] (implements ImageMagick functionality in Java) or preferably use alternatives [mini_magick][] & [quick_magick][]. For simple resizing, cropping, greyscaling, etc look at [image_voodoo][]. You can also use Java's Graphics2D.

* **[Unicorn][]** - Try any one of the following [[JRuby-based servers|Servers]]: [Trinidad][], [Mizuno][], [Kirk][] or [Puma][] (though make sure to use the JRuby-native version of the [gem](http://rubygems.org/gems/puma/versions/2.0.0.b7-java)).

* **[Thin][]** - Thin might compile and run but is not recommended. Try any one of the following [[JRuby-based servers|Servers]]: [Trinidad][], [Mizuno][], [Kirk][], [TorqueBox][] or [Puma][].

* **mysql** - Use [activerecord-jdbcmysql-adapter][] instead.

* **mysql2** - Use [activerecord-jdbcmysql-adapter][] instead.

* **sqlite3** - Use [activerecord-jdbcsqlite3-adapter][] instead.

* **pg** - Use [activerecord-jdbcpostgresql-adapter][] instead or [pg_jruby][] (drop-in replacement).

* **[Nokogiri][]** - For best results, use the pure-Java version of Nokogiri (default after v1.5).

* **[yajl-ruby][]** - Try `json` or `json_pure` instead. Unfortunately there is no known equivalent JSON stream parser.

* **[oj][]** - Try `gson`, `json` or `json_pure` instead.

* **[bson_ext][]** - `bson_ext` isn't used with JRuby. Instead, some native Java extensions are bundled with the `bson` gem.

* **[win32ole][]** - Use the `jruby-win32ole` gem (preinstalled in JRuby's Windows installer).

* **[curb][]** - [Rurl][] is an example how to implement _some_ of curb's functionality using [Apache HttpClient][]

* **[therubyracer][]** - Try using [therubyrhino][] instead (or [dienashorner][] on Java 8+).

* **[kyotocabinet][]** - Try using [kyotocabinet-java][] instead. This isn't 100% complete yet, but it covers most of the API.

* **[memcached][]** - Try using [jruby-memcached][] instead. Alternatively you can use [jruby-ehcache][], a JRuby interface to Java's (JSR-107 compliant) Ehcache.

Please add to this list with your findings.

*Note that the [JRuby-Lint][] gem parses the contents of the list above to use for its Ruby gem checker. In order for JRuby-Lint to use the information, please adhere to the `gem_name - instructions` format.*

[RDiscount]: http://dafoster.net/projects/rdiscount/
[RedCarpet]: https://github.com/vmg/redcarpet
[kramdown]: https://github.com/gettalong/kramdown
[Maruku]:https://github.com/bhollis/maruku
[markdown_j]: https://github.com/nate/markdown_j
[RMagick]: https://github.com/rmagick/rmagick
[RMagick4J]: https://github.com/Serabe/RMagick4J
[mini_magick]: https://github.com/minimagick/minimagick
[quick_magick]: https://github.com/aseldawy/quick_magick
[image_voodoo]: https://github.com/jruby/image_voodoo
[Unicorn]: http://unicorn.bogomips.org/
[Trinidad]: https://github.com/trinidad/trinidad
[Mizuno]: https://github.com/matadon/mizuno
[Kirk]: https://github.com/strobecorp/kirk
[Puma]: http://puma.io/
[Thin]: http://code.macournoyer.com/thin/
[Typhoeus]: https://github.com/dbalatero/typhoeus
[activerecord-jdbc-adapter]: https://github.com/jruby/activerecord-jdbc-adapter
[JRuby-Lint]: https://github.com/jruby/jruby-lint
[Nokogiri]: http://nokogiri.org/
[yajl-ruby]: https://github.com/brianmario/yajl-ruby
[bson_ext]: https://github.com/mongodb/mongo-ruby-driver
[Apache HttpClient]: http://hc.apache.org/httpcomponents-client-ga/
[HttpURLConnection]: http://download.oracle.com/javase/1,5.0/docs/api/java/net/HttpURLConnection.html
[win32ole]: http://www.ruby-doc.org/stdlib/libdoc/win32ole/rdoc/index.html
[Rurl]: https://github.com/rcyrus/Rurl
[curb]: https://github.com/taf2/curb
[therubyracer]: https://github.com/cowboyd/therubyracer
[therubyrhino]: https://github.com/cowboyd/therubyrhino
[dienashorner]: https://github.com/kares/dienashorner
[kyotocabinet]: http://fallabs.com/kyotocabinet/
[kyotocabinet-java]: https://github.com/csw/kyotocabinet-java
[memcached]: https://github.com/evan/memcached
[jruby-memcached]: https://github.com/aurorafeint/jruby-memcached
[TorqueBox]: http://torquebox.org/
[jruby-ehcache]: https://github.com/dylanz/ehcache
[oj]: https://github.com/ohler55/oj
[activerecord-jdbcmysql-adapter]: https://rubygems.org/gems/activerecord-jdbcmysql-adapter
[activerecord-jdbcsqlite3-adapter]: https://rubygems.org/gems/activerecord-jdbcsqlite3-adapter
[activerecord-jdbcpostgresql-adapter]: https://rubygems.org/gems/activerecord-jdbcpostgresql-adapter
[pg_jruby]: https://rubygems.org/gems/pg_jruby
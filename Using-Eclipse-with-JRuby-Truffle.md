Eclipse is one of the best supported IDE for editing JRuby+Truffle.
It provides excellent feedback from the Truffle DSL annotation processor
and actually keeps the project built at all times by rebuilding it incrementally.

### Get Mx

If you do not already have mx, clone it and add it to `$PATH`:
```bash
$ git clone https://github.com/graalvm/mx.git ../mx
$ export PATH="../mx:$PATH"
```

### Build the project

First, make sure the project is already built from the command line:
```bash
$ mx update
# Consider truffle API as a binary dependency
$ echo MX_BINARY_SUITES=truffle >> mx.jruby/env
$ mx build
```

### Generate the project files

```bash
$ mx eclipseinit
```

### Import the projects

Create a new workspace in Eclipse (>= Luna).

We can now import the projects:
* From the main menu bar, select `File` > `Import...`
* Select `General` > `Existing Projects into Workspace`
* Select `jruby` directory as root directory
* **Unselect** `jruby-jars` and `test`
* Click `Finish`

You shall be set!
There should be now 4 projects in your workspace:
`jruby-truffle`, `jruby-truffle-test`, `RUBY.dist`, `RUBY-TEST.dist`.

### Running from the Eclipse files directly

The [jt workflow tool](https://github.com/jruby/jruby/tree/master/truffle#workflow-tool)
automatically picks up the version compiled by mx and Eclipse oven Maven-compiled files.

```bash
$ tool/jt.rb ruby -e 'p RUBY_ENGINE'
"jruby+truffle"
```
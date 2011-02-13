Writing a Good Bug Report (GBR) is one of the keys to getting the core team to assign a high priority to a bug you may have found.  It is key to making a convincing 'argument' that you have, in fact, found a bug.  This section is intended to help you help the core team help you.

GBRs have three fundamental characteristics.  In short, they are

* **Reproducible**:  A GBR contains a "script" that reproduces the 'bad' behavior every time it's executed.  The script should contain initial state information, to the extent that reproducing the problem requires it.  Problems that can't be consistently reproduced are, in some circles, assumed to result from operator error. ;-)
* **Minimal**:  A GBR does not contain include anything that's not needed to reproduce the bug. To the extent that many JRuby 'bugs' will be discovered as a consequence of developing Rails applications, a small, focused Rails sandbox app can be a pretty good start.  Look closely at whether or not the app really has a DB requirement to reproduce the problem.  Try hard to narrow things down to a specific file or files, debugging that file(s) to figure out when things start to go wrong, and creating a reduced test case that demonstrates it. There's practically no bug that can't be reduced to a single script without external requirements, so that's the goal to aim for.
* **Executable**: Time is the core team's most precious resource.  In general, anything they can run is better than nothing, even if it's large. Of course there's a limit to what they can do; submissions that require specific host platforms, databases, or complicated combinations of libraries, applications, or servers means they're probably not going to "jump on it."  Additionally, all of the bug fixes committed to JRuby are required to be accompanied by a unit test, to ensure they don't get broken again. In practice, we're not 100% on this, but it is an official rule. Having a small, executable test case makes it trivial for the core team to fulfill that rule.

Writing GBRs is often hard, sometimes frustrating work.  Please understand that the core team understands that, and that the contributors who produce GBRs are very much appreciated for helping make JRuby _better_, that much _faster_!
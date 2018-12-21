# Demo node-quickfix

Demonstration of node-quickfix, a node.js wrapper of the QuickFIX financial server library.

See https://github.com/Trumid/node-quickfix

See https://github.com/joelparkerhenderson/demo_quickfix


## Install QuickFIX

Download quickfix tar here: http://www.quickfixengine.org/

Warning: the current version of QuickFIX as of 2018-12-21 seems to be incompatible with node-quickfix. See https://github.com/Trumid/node-quickfix/issues/48. Users report that a downgrade to 1.14.3 works. This repo includes the quickfix 1.14.3 for your convenience.

Check required dependencies: http://www.quickfixengine.org/quickfix/doc/html/dependencies.html

Install via the following instructions: http://www.quickfixengine.org/quickfix/doc/html/building.html

If you need help with building, then see our demo https://github.com/joelparkerhenderson/demo_quickfix

QuickFIX should create files that you will include in the next step:

```sh
ls /usr/local/include/quickfix/
```


## Copy QuickFIX config

The QuickFIX software directory has a top-level file `config.h`.

Copy the file to your system's typical directory of `include` files, which is usually `/usr/local/include/quickfix`.

Example:

```sh
sudo copy config.h /usr/local/include/quickfix/
```


## Install node

Install node tooling:

```sh
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
. ~/.nvm/nvm.sh
nvm install node
npm install -g node-gyp
```


## Install node-quickfix

Clone:

```sh
git clone https://github.com/Trumid/node-quickfix.git
cd node-quickfix
```


## Verify gyp

Verify the steps for `node-gyp` are successful:

```sh
node-gyp clean
node-gyp configure
node-gyp build
```

If you get this error in the logs:

```
info lifecycle node-quickfix@2.1.1~install: Failed to exec install script
```

Then be sure you're trying to use quickfix version 1.14.3 or similar compatibile version. This is because currently, as of 2018-12-21, the current quickfix version 1.15.1 is not compatible with the currentnode-quickfix version.

If you get this error:

```
../src/FixConnection.h:23:29: fatal error: quickfix/config.h: No such file or directory
 #include "quickfix/config.h"
```

Then be sure you copied the quickfix file `config.h` to a suitable locaiton, such as your system's typical include directory, for example:

```sh
sudo cp ~/quickfix/config.h /usr/local/include/quickfix/
```

If you get this error:

```
/usr/bin/ld: cannot find -lxml2
collect2: error: ld returned 1 exit status
```

Then install libxml:

```sh
sudo yum install libxml2
sudo yum install libxml2-devel
```

Retry as needed.


## Build

Build:

```sh
npm install
```

If you get this error in the logs:

```
info lifecycle node-quickfix@2.1.1~install: Failed to exec install script
```

Then be sure the steps above worked for `node-gyp`.

If you get this error:

```
found 3 vulnerabilities (1 low, 1 high, 1 critical)
  run `npm audit fix` to fix them, or `npm audit` for details
```

Then audit:

```
npm audit
```

We saw these issues:

* Package `growl` has a command injection.

* Package `debug` has a regular expression denial of service.

* Package `minimatch` has a regular expression denial of service.

In our case, these issues all are for development, not production. The long-term solution is to upgrade the `node-quickfix` repo with updated dependencies.


## Test

Test:
```
npm test
```

If you get this error:

```
Error: libquickfix.so.16: cannot open shared object file: No such file or directory
```

Then find the file:

```
sudo yum install mlocate
sudo updatedb
locate libquickfix.so.16
```

And if your setup is typical, then you should see the files here:

```
/usr/local/lib/libquickfix.so.16
/usr/local/lib/libquickfix.so.16.0.1
```

Then run:

```sh
sudo ldconfig
```

Then verify that your library path is set, and also lists the directory that has your libquickfiles:

```sh
echo $LD_LIBRARY_PATH
```

The `$LD_LIBRARY_PATH` environment variable is consulted at time of execution, to provide a list of additional directories in which to search for dynamically linkable libraries.

If your library path is not set, i.e. blank, then set it:

```sh
export LD_LIBRARY_PATH=/usr/local/lib/
```

If your library path is set, but does not list the directory that has your libquickfix files:

```sh
export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/usr/local/lib/"
```

Retry as needed.

Success looks like this:

```
npm test

> node-quickfix@2.1.1 test /home/ec2-user/node-quickfix
> mocha



  initiator
    ✓ should throw if not supplied options
    ✓ should throw if supplied options with no settings or propertiesFile
    ✓ should create a new initiator instance with fix beginString, senderCompID, targetCompID on defined session (3093ms)
    ✓ should start and stop when called (3131ms)

  acceptor
    ✓ should throw if not supplied options
    ✓ should throw if supplied options with no settings or propertiesFile
    ✓ should create a new acceptor instance with fix beginString, senderCompID, targetCompID on defined session (3168ms)
    ✓ should start and stop when called (3146ms)


  8 passing (13s)
```

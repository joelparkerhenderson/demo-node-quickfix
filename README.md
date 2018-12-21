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


## Install node-quickfix

Clone:

```sh
git clone https://github.com/Trumid/node-quickfix.git
cd node-quickfix
```

Verify gyp works:

```sh
node-gyp configure
...
gyp info ok
```

Verify gyp build works:

```sh
node-gyp build
```

info lifecycle node-quickfix@2.1.1~install: Failed to exec install script


Build:

```sh
npm install
```

If you get this error in the logs:

```
info lifecycle node-quickfix@2.1.1~install: Failed to exec install script
```


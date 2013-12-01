
Just a quick fork to patch `node-sudo` so that now you can retrieve the password
(here shown in CoffeeScript):

```coffeescript
sudo = require 'coffeenode-sudo'

options =
  cachePassword:  no
  prompt:         "Password, yo? "
  spawnOptions:   {} # other options for spawn

# assuming the file is only readable to root:
# -rw-r-----   1 root        wheel   16  1 Dez 01:23 forbidden

child = sudo [ 'cat', '/tmp/forbidden', ], options, ( error, password ) ->
  console.log "the password is:", password

child.stdout.on 'data', ( data ) ->
  console.log data.toString()
```

The handler will be called when the process has finished.


node-sudo
=========

A `child_process.spawn` but with `sudo` in between. The `sudo` password dialog
is abstracted away so that the calling Node script can interact with the
program that is run under `sudo` without worrying about it.

Synopsis
--------

```javascript
sudo(args, options)
```

 - `args`: An array of arguments to `sudo`. Can be both options (such as `-v`
   or `-E`) and the program to run. Example: `['ls']`.

 - `options`: An optional object containing options. Recognized options are:

    - `cachePassword`: Boolean; whether to remember the password between
      invocations or not.

    - `prompt`: String; what to display to the user when the password is
      needed.

    - `spawnOptions`: Object; passed on directly to `spawn`. `stdio` or
      `customFds` will be overwritten.

Example
-------

```javascript
var sudo = require('sudo');
var options = {
    cachePassword: true,
    prompt: 'Password, yo? ',
    spawnOptions: { /* other options for spawn */ }
};
var child = sudo([ 'ls', '-l', '/tmp' ], options);
child.stdout.on('data', function (data) {
    console.log(data.toString());
});
```

License
-------

MIT

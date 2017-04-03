# api documentation for  [gulp-run (v1.7.1)](https://github.com/MrBoolean/gulp-run)  [![npm package](https://img.shields.io/npm/v/npmdoc-gulp-run.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-gulp-run) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-gulp-run.svg)](https://travis-ci.org/npmdoc/node-npmdoc-gulp-run)
#### Pipe to shell commands in gulp

[![NPM](https://nodei.co/npm/gulp-run.png?downloads=true)](https://www.npmjs.com/package/gulp-run)

[![apidoc](https://npmdoc.github.io/node-npmdoc-gulp-run/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-gulp-run_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-gulp-run/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-gulp-run/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-gulp-run/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Marc Binder",
        "email": "marcandrebinder@gmail.com"
    },
    "bugs": {
        "url": "https://github.com/MrBoolean/gulp-run/issues"
    },
    "dependencies": {
        "gulp-util": "^3.0.0",
        "lodash.defaults": "^4.0.1",
        "lodash.template": "^4.0.2",
        "vinyl": "^0.4.6"
    },
    "description": "Pipe to shell commands in gulp",
    "devDependencies": {
        "babel-eslint": "^4.1.8",
        "del": "^2.2.0",
        "eslint": "^1.10.3",
        "eslint-config-airbnb": "^4.0.0",
        "eslint-plugin-react": "^3.16.1",
        "gulp": "^3.6.2",
        "gulp-codacy": "^1.0.0",
        "gulp-eslint": "^1.1.1",
        "gulp-if": "^2.0.0",
        "gulp-istanbul": "^0.10.3",
        "gulp-mocha": "^2.2.0",
        "gulp-rename": "^1.2.0",
        "gulp-sequence": "^0.4.4",
        "mocha": "^2.1.0",
        "should": "^8.2.1"
    },
    "directories": {},
    "dist": {
        "shasum": "e17c0acb7c30b6e2aeee23c04442a96c0caceffa",
        "tarball": "https://registry.npmjs.org/gulp-run/-/gulp-run-1.7.1.tgz"
    },
    "gitHead": "f58b6dbeb2d9f1c21579a7654d28ce6764f3a541",
    "homepage": "https://github.com/MrBoolean/gulp-run",
    "keywords": [
        "gulpplugin",
        "gulp",
        "run",
        "shell",
        "command",
        "sh"
    ],
    "license": "ISC",
    "main": "index.js",
    "maintainers": [
        {
            "name": "cbarrick",
            "email": "cbarrick1@gmail.com"
        },
        {
            "name": "mrboolean",
            "email": "marcandrebinder@gmail.com"
        }
    ],
    "name": "gulp-run",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git://github.com/MrBoolean/gulp-run.git"
    },
    "scripts": {},
    "version": "1.7.1"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module gulp-run](#apidoc.module.gulp-run)
1.  [function <span class="apidocSignatureSpan">gulp-run.</span>Command (command, options)](#apidoc.element.gulp-run.Command)
1.  [function <span class="apidocSignatureSpan">gulp-run.</span>super_ (options)](#apidoc.element.gulp-run.super_)
1.  object <span class="apidocSignatureSpan">gulp-run.</span>Command.prototype

#### [module gulp-run.Command](#apidoc.module.gulp-run.Command)
1.  [function <span class="apidocSignatureSpan">gulp-run.</span>Command (command, options)](#apidoc.element.gulp-run.Command.Command)

#### [module gulp-run.Command.prototype](#apidoc.module.gulp-run.Command.prototype)
1.  [function <span class="apidocSignatureSpan">gulp-run.Command.prototype.</span>exec (stdin, callback)](#apidoc.element.gulp-run.Command.prototype.exec)
1.  [function <span class="apidocSignatureSpan">gulp-run.Command.prototype.</span>toString ()](#apidoc.element.gulp-run.Command.prototype.toString)



# <a name="apidoc.module.gulp-run"></a>[module gulp-run](#apidoc.module.gulp-run)

#### <a name="apidoc.element.gulp-run.Command"></a>[function <span class="apidocSignatureSpan">gulp-run.</span>Command (command, options)](#apidoc.element.gulp-run.Command)
- description and source-code
```javascript
function Command(command, options) {
  var previousPath;

  this.command = command;

  // We're on Windows if 'process.platform' starts with "win", i.e. "win32".
  this.isWindows = (process.platform.lastIndexOf('win') === 0);

  // the cwd and environment of the command are the same as the main node
  // process by default.
  this.options = defaults(options || {}, {
    cwd: process.cwd(),
    env: process.env,
    verbosity: (options && options.silent) ? 1 : 2,
    usePowerShell: false
  });

  // include node_modules/.bin on the path when we execute the command.
  previousPath = this.options.env.PATH;
  this.options.env.PATH = path.join(this.options.cwd, 'node_modules', '.bin');
  this.options.env.PATH += path.delimiter;
  this.options.env.PATH += previousPath;
}
```
- example usage
```shell
...
    .src('path/to/input/*')             // get input files.
    .pipe(run('awk "NR % 2 == 0"'))     // use awk to extract the even lines.
    .pipe(gulp.dest('path/to/output'))  // profit.
  ;
});

// use gulp-run without gulp
var cmd = new run.Command('cat');  // create a command object for 'cat'.
cmd.exec('hello world');           // call 'cat' with 'hello world' on stdin.
'''

## API
### 'run(template, [options])'
Creates a Vinyl (gulp) stream that transforms its input by piping it to a shell command.
...
```

#### <a name="apidoc.element.gulp-run.super_"></a>[function <span class="apidocSignatureSpan">gulp-run.</span>super_ (options)](#apidoc.element.gulp-run.super_)
- description and source-code
```javascript
function Transform(options) {
  if (!(this instanceof Transform))
    return new Transform(options);

  Duplex.call(this, options);

  this._transformState = new TransformState(this);

  var stream = this;

  // start out asking for a readable event once data is transformed.
  this._readableState.needReadable = true;

  // we have implemented the _read method, and done the other things
  // that Readable wants before the first _read call, so unset the
  // sync guard flag.
  this._readableState.sync = false;

  if (options) {
    if (typeof options.transform === 'function')
      this._transform = options.transform;

    if (typeof options.flush === 'function')
      this._flush = options.flush;
  }

  // When the writable side finishes, then flush out anything remaining.
  this.once('prefinish', function() {
    if (typeof this._flush === 'function')
      this._flush(function(er) {
        done(stream, er);
      });
    else
      done(stream);
  });
}
```
- example usage
```shell
n/a
```



# <a name="apidoc.module.gulp-run.Command"></a>[module gulp-run.Command](#apidoc.module.gulp-run.Command)

#### <a name="apidoc.element.gulp-run.Command.Command"></a>[function <span class="apidocSignatureSpan">gulp-run.</span>Command (command, options)](#apidoc.element.gulp-run.Command.Command)
- description and source-code
```javascript
function Command(command, options) {
  var previousPath;

  this.command = command;

  // We're on Windows if 'process.platform' starts with "win", i.e. "win32".
  this.isWindows = (process.platform.lastIndexOf('win') === 0);

  // the cwd and environment of the command are the same as the main node
  // process by default.
  this.options = defaults(options || {}, {
    cwd: process.cwd(),
    env: process.env,
    verbosity: (options && options.silent) ? 1 : 2,
    usePowerShell: false
  });

  // include node_modules/.bin on the path when we execute the command.
  previousPath = this.options.env.PATH;
  this.options.env.PATH = path.join(this.options.cwd, 'node_modules', '.bin');
  this.options.env.PATH += path.delimiter;
  this.options.env.PATH += previousPath;
}
```
- example usage
```shell
...
    .src('path/to/input/*')             // get input files.
    .pipe(run('awk "NR % 2 == 0"'))     // use awk to extract the even lines.
    .pipe(gulp.dest('path/to/output'))  // profit.
  ;
});

// use gulp-run without gulp
var cmd = new run.Command('cat');  // create a command object for 'cat'.
cmd.exec('hello world');           // call 'cat' with 'hello world' on stdin.
'''

## API
### 'run(template, [options])'
Creates a Vinyl (gulp) stream that transforms its input by piping it to a shell command.
...
```



# <a name="apidoc.module.gulp-run.Command.prototype"></a>[module gulp-run.Command.prototype](#apidoc.module.gulp-run.Command.prototype)

#### <a name="apidoc.element.gulp-run.Command.prototype.exec"></a>[function <span class="apidocSignatureSpan">gulp-run.Command.prototype.</span>exec (stdin, callback)](#apidoc.element.gulp-run.Command.prototype.exec)
- description and source-code
```javascript
function exec(stdin, callback) {
  var self = this;
  var command;
  var fileName;
  var directory;
  var subShell;
  var log;
  var err;
  var stdout;

  // parse the arguments, both are optional.
  // after parsing, stdin is a vinyl file to use as standard input to
  // the command (possibly empty), and callback is a function.
  if (typeof stdin === 'function') {
    callback = stdin;
    stdin = undefined;
  } else if (typeof callback !== 'function') {
    callback = function noop() {};
  }

  if (!(stdin instanceof Vinyl)) {
    fileName = this.command.split(' ')[0];
    directory = path.join(this.options.cwd, fileName);

    if (typeof stdin === 'string') {
      stdin = new Vinyl({
        path: directory,
        contents: new Buffer(stdin)
      });
    } else if (stdin instanceof Buffer || stdin instanceof stream.Readable) {
      stdin = new Vinyl({
        path: directory,
        contents: stdin
      });
    } else {
      stdin = new Vinyl(stdin);

      if (!stdin.path) {
        stdin.path = directory;
      }
    }
  }

  // execute the command.
  // we spawn the command in a subshell, so things like i/o redirection
  // just work. e.g. 'echo hello world >> ./hello.txt' works as expected.
  command = applyTemplate(this.command)({
    file: stdin
  });

  if (this.isWindows && this.options.usePowerShell) {
    // windows powershell
    subShell = cp.spawn('powershell.exe', ['-NonInteractive', '-NoLogo', '-Command', command], {
      env: this.options.env,
      cwd: this.options.cwd
    });
  } else if (this.isWindows) {
    // windows cmd.exe
    subShell = cp.spawn('cmd.exe', ['/c', command], {
      env: this.options.env,
      cwd: this.options.cwd
    });
  } else {
    // POSIX shell
    subShell = cp.spawn('sh', ['-c', command], {
      env: this.options.env,
      cwd: this.options.cwd
    });
  }

  // setup the output
  //
  // - if verbosity equals to 3, the command prints directly to the terminal.
  // - if verbosity equals to 2, the command's stdout and stderr are buffered
  //   and printed to the user's terminal after the command exits (this
  //   prevents overlaping output of multiple commands)
  // - if verbosity equals to 1, the command's stderr is buffered as in 2, but
  //   the stdout is silenced.
  log = new stream.PassThrough();

  function sendLog(context) {
    var title = util.format(
      '$ %s%s',
      gutil.colors.blue(command),
      (self.options.verbosity < 2) ? gutil.colors.grey(' # Silenced\n') : '\n'
    );

    context.write(title);
  }

  switch (this.options.verbosity) {
    case 3:
      sendLog(process.stdout);
      subShell.stdout.pipe(process.stdout);
      subShell.stderr.pipe(process.stderr);
      break;
    case 2:
      subShell.stdout.pipe(log);
      // fallthrough
    case 1:
      subShell.stderr.pipe(log);
      sendLog(log);
      break;
  }

  // setup the cleanup proceedure for when the command finishes.
  subShell.once('close', function handleSubShellClose() {
    // write the buffered output to stdout
    var content = log.read();

    if (content !== null) {
      process.stdout.write(content);
    }
  });

  subShell.once('exit', function handleSubShellExit(code) {
    // report an error if the command exited with a non-zero exit code.
    if (code !== 0) {
      err = new Error(util.format('Command '%s' exited with code %s', command, code));
      err.status = code;

      return callback(err);
    }

    callback(null);
  });

  // the file wrapping stdout is as the one wrapping stdin (same metadata)
  // with different contents.
  stdout = stdin.clone();
  stdout.contents = subShell.stdout.pipe(new stream.PassThrough());

  // finally, write the input to the process's stdin.
  stdin.pipe(subShell.stdin);

  return stdout;
}
```
- example usage
```shell
...
## Usage

'''javascript
var run = require('gulp-run');

// use gulp-run to start a pipeline
gulp.task('hello-world', function() {
  return run('echo Hello World').exec()    // prints "Hello World\n".
    .pipe(gulp.dest('output'))      // writes "Hello World\n" to output/echo.
  ;
})


// use gulp-run in the middle of a pipeline:
gulp.task('even-lines', function() {
...
```

#### <a name="apidoc.element.gulp-run.Command.prototype.toString"></a>[function <span class="apidocSignatureSpan">gulp-run.Command.prototype.</span>toString ()](#apidoc.element.gulp-run.Command.prototype.toString)
- description and source-code
```javascript
function toString() {
  return this.command;
}
```
- example usage
```shell
n/a
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)

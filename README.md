# rxfs

portable local file server

usable standalone or as a module

## setup

### standalone

save [rxfs.js](https://github.com/r1vn/rxfs/raw/master/rxfs.js) in the directory you want to serve and run `node rxfs`

or (most linux distros)

save [rxfs.js](https://github.com/r1vn/rxfs/raw/master/rxfs.js) as `~/bin/rxfs` and run `chmod +x ~/bin/rxfs`

or (any os)

`npm i -g r1vn/rxfs` as root or admin

### as a module

save [rxfs.js](https://github.com/r1vn/rxfs/raw/master/rxfs.js) in your project directory

## options

```
{
// absolute or relative path of the directory to serve
    root: '.',
// localhost port to serve at, e.g. http://localhost:1234
    port: 1234,
// the path to serve at
// this part of the URL is ignored when mapping the URL to the resource
    baseurl: '/',
// automatically serves the index.html file if the requested path is a directory that contains one
// mutually exclusive with 'servedir'
    autoindex: false,
// displays a file browser view upon requesting a directory
// mutually exclusive with 'autoindex'
    servedir: true,
// headers applied to file/status responses
    headers: { 'cache-control': `public, max-age=604800, immutable` },
// additional MIME type definitions
// key = file extension, value = mime type, e.g. { "txt": "text/plain" }
    mimes: {},
// if specified, logs requests/responses to the log file
// the path is absolute or relative
    logfile: '',
// toggles logging requests/responses to the terminal
    verbose: true,
// toggles additional debug output to the terminal
    debug: false
}
```

## use

### standalone

CLI

```
rxfs
    with default configuration, serves the current working directory at http://localhost:1234
rxfs .
rxfs /tmp/files
    specifies the directory explicitly
    the path may be relative to terminal's working directory or absolute
rxfs . 2345
    the second argument specifies the port to run the server at, e.g. http://localhost:2345
rxfs . 2345 -logfile=/tmp/rxfs.log -autoindex ---servedir
    specifies additional options
    - prefix without a value sets the option to true, --- to false
```

in standalone mode, default options are replaced from the following sources:

- options in the command line invocation (highest precedence)
- `.rxfs` JSON file in terminal's directory
- `~/.config/rxfs` JSON file

example `.rxfs` / `~/config.rxfs` file:

```
{
    "root": ".",
    "port": 1234,
    "baseurl": "/",
    "autoindex": false,
    "servedir": true,
    "headers": { "cache-control": "public, max-age=604800, immutable" },
    "mimes": {},
    "logfile": "rxfs.log",
    "verbose": true,
    "debug": false
}
```

`rxfs -init` creates `.rxfs` with the above contents in the current directory

### as a module

```
const { RXFS } = require('./path/to/rxfs.js')

const rxfs = new RXFS({
    root: 'dir/to/serve',
    port: 2345,
    ...
})
```
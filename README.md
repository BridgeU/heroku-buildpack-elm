# heroku-buildpack-elm
A simple script to download the Elm binary in Heroku

## Why? 

To build on M1 we want to avoid installing Elm via npm, because it doesn't have binaries for all architectures. Hence we need a way to install Elm on Heroku.
I didn't understand what the other buildpacks were doing with Docker and AWS (see [this buildpack](https://elements.heroku.com/buildpacks/srid/heroku-buildpack-elm) for an example), seemed really complicated for something rather simple. They were also unmaintained and not touched since 2017.

## What does this do?

A Heroku buildpack is really just a little bit of bash that runs during deployment. It has three scripts: detect, compile and release.

### Detect

Checks to see whether `elm` in the /app/bin/ folder returns a non zero status code. If it's there (because maybe you checked it into the repo) this buildpack won't do anything. Probably could be improved to parse the output of `elm --version` and compare that with the `ELM_VERSION` to see if it needs installing...

### Compile

Doesn't compile anything :) Just downloads Elm from GitHub, unzips it and puts it into `/app/bin/elm`. Can't write it into `/usr/local/bin` so we'll set the `PATH` in the next step...

### Release

Adds the `$BUILD_DIR/bin/` (e.g. /app/bin) to the $PATH so we find elm.

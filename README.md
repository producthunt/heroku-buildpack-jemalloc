# heroku-buildpack-jemalloc

I am a Heroku buildpack that installs [jemalloc](http://jemalloc.net/) into a
dyno slug.

## Install

[Heroku supports using multiple buildpacks for an app](https://devcenter.heroku.com/articles/using-multiple-buildpacks-for-an-app).

```console
heroku buildpacks:add --index 1 https://github.com/producthunt/heroku-buildpack-jemalloc.git
git push heroku master
```

## Usage

### Recommended

Set the JEMALLOC_ENABLED config option to true and jemalloc will be used for
all commands run inside of your dynos.

```console
heroku config:set JEMALLOC_ENABLED=true
```

### Per dyno

To control when jemalloc is configured on a per dyno basis use
`jemalloc.sh <cmd>` and ensure that JEMALLOC_ENABLED is unset.

Example Procfile:
```yaml
web: jemalloc.sh bundle exec puma -C config/puma.rb
```

## Configuration

### JEMALLOC_ENABLED

Set this to true to automatically enable jemalloc.

```console
heroku config:set JEMALLOC_ENABLED=true
```

To disable jemalloc set the option to false. This will cause the application to
restart disabling jemalloc.

```console
heroku config:set JEMALLOC_ENABLED=false
```

### JEMALLOC_VERSION

Set this to select or pin to a specific version of jemalloc. The default is to
use the latest stable version if this is not set. You will receive an error
mentioning tar if the version does not exist.

A full list of supported versions and stacks is available on the
[releases page.](https://github.com/mojodna/heroku-buildpack-jemalloc/releases)

**note:** This setting is only used during slug compilation. Changing it will
require a code change to be deployed in order to take affect.

```console
heroku config:set JEMALLOC_VERSION=5.0.1
```

## Building

This uses Docker to build against Heroku
[stack-image](https://github.com/heroku/stack-images)-like images.

```console
make VERSION=5.0.1
```

Artifacts will be dropped in `dist/` based on Heroku stack and jemalloc version.

### Deploying New Versions

- `make VERSION=X.Y.Z`
- `open dist`
- Go to [releases](https://github.com/mojodna/heroku-buildpack-jemalloc/releases)
- Edit the release corresponding to each heroku Stack
- Drag and drop the new build to attach

### Creating a New Stack
- Go to [releases](https://github.com/mojodna/heroku-buildpack-jemalloc/releases)
- Click "Draft a new release"
- Tag is the name of the Stack (e.g. `heroku-18`)
- Target is `release-master`
- Title is `Builds for the [stack] stack`

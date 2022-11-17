# Post-build Slug trash files remover buildpack

A simple buildpack to run after all other buildpacks have completed,
which removes a set of files defined in `.slug-post-clean`, so that they
are not included in the finished slug.

Based on the awesome work in this [repo](https://github.com/Lostmyname/heroku-buildpack-post-build-clean/compare/master...marketertechnologies:heroku-buildpack-post-build-clean:master)

## Rationale

While this may seem to duplicate functionality provided by Heroku's
`.slugignore`, there is a key difference: `.slugignore`'d files are
removed after the repo is cloned, but before any buildpack is run. They
can therefore not be involved in the build process itself.

However, it is not uncommon for there to exist files in the repo that
are necessary for the build, but are not required at runtime. There may
also be installable build dependencies that are not runtime
dependencies.

In our case, a complex front-end build involves significant CSS, JS and
image assets, along with a large installation of node modules, all of
which are used only for building the production assets, but then remain
part of the slug.

## Usage

The `.slug-post-clean` file supports wildcards in the `find` command format. 
Wildcards should contain current directory reference at the beginning:

```
./some_huge_file.psd # removes file in the current root
./public/*.js.map # all *.js.map files in the public directory
./tmp/cache # matches exact name of the directory
```

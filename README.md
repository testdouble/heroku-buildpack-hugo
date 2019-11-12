**[🍴 Fork Alert 🍴]**

This is a slightly-modified fork of [@roperzh's Hugo
buildpack](https://github.com/roperzh/heroku-buildpack-hugo). We have made a
few changes and additions to suit our static content needs at [Test
Double](https://testdouble.com).

* Any build failures will now cause the script to exit with a non-zero status,
  which in turn will cause Heroku to recognize that the build has failed and
  prevent a broken build's deployment. (See [this
  commit](https://github.com/testdouble/heroku-buildpack-hugo/commit/64a8b54d6206e41afa9d001eb9dce8cba4054141))
* So that you can toggle behavior of the Hugo build based on the environment
  (using the `[getenv](https://gohugo.io/functions/getenv/#readout)` function,
  for example), all of the environment variables set on the Heroku dyno are
  assigned (by default, [they're only available to the buildpack as readable
  files in a
  directory](https://devcenter.heroku.com/articles/buildpack-api#bin-compile))
* If you define executable files in `script/pre-hugo-build` and
  `script/post-hugo-build`, those will be run before and after the hugo build.
  This can be useful for fetching supporting assets prior to running the build
  and trimming the size of the Heroku slug prior to the `compile` phase's exit
* To make _absolutely certain_ that there are no out-of-date cached items or
  residual assets from a previous build, the `hugo` command is run with both
  `--cleanDestinationDir` and `--ignoreCache` flags

**[/🍴 Fork Alert 🍴]**



Heroku buildpack: Hugo
===

This is a [Heroku buildpack](https://devcenter.heroku.com/articles/buildpacks)
for sites powered by [Hugo](https://github.com/spf13/hugo).
It uses the latest stable version of Hugo.

Usage
===

Create a Heroku application using this buildpack:

```bash
$ heroku create --buildpack https://github.com/testdouble/heroku-buildpack-hugo.git
```

or configure your existent application:

```bash
$ heroku buildpacks:set BUILDPACK_URL="https://github.com/testdouble/heroku-buildpack-hugo.git"
```

Optionally, define a `HUGO_VERSION` Config Var to specify the Hugo version you wish to use:

```bash
$ heroku config:set HUGO_VERSION=0.25
```

Then simply git push to heroku and open your application!

```bash
$ git push heroku master
$ heroku open
```

Using themes
===

This buildpack provides a simple api to use custom themes, just add a `.hugotheme`
file in the root of your application with the url of your theme.

***Example***

To fetch the great [hyde](https://github.com/spf13/hyde.git) theme:

```
https://github.com/spf13/hyde.git
```

[Here](http://immense-hollows-6319.herokuapp.com/) is an example application,
and [here](https://github.com/testdouble/example-heroku-buildpack-hugo) is the code.

Alternative method
---

If you don't like the idea of a `.hugotheme` file, you can simply manage your
themes with [git submodules](http://git-scm.com/book/en/Git-Tools-Submodules).
Heroku will take care to fetch all the submodules in your project.

Important notes
===

Don't forget to configure your hugo `baseurl` with the url of your application, using **https://** not http://

Contributing
===

1- Fork it

2- Create your feature branch (git checkout -b my-new-feature)

3- Commit your changes (git commit -am 'Add some feature')

4- Push to the branch (git push origin my-new-feature)

5- Create new Pull Request

License
===

The MIT License (MIT)

Copyright (c) 2015 Roberto Dip

Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the "Software"), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of
the Software, and to permit persons to whom the Software is furnished to do so,
subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS
FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR
COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER
IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

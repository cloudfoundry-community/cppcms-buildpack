# cppcms-buildpack

A buildpack for deploying [CppCMS](http://cppcms.com/) applications.

See [this blog post](http://engineering.pivotal.io/post/making-a-useful-c++-buildpack/) for an explanation and full context on why this buildpack was created.


## conventions

Your application must have a config file in its root directory named
`cppcms.js`. This file is where the buildpack will inject environment
configuration, such as the listen port.

You must provide a `Makefile` whose default target will compile the
cppcms executable referenced by your start command.

Finally, the default start command will be `make run`, meaning that
the buildpack expects there to be a `run` target. If you prefer, you
may provide a custom start command, either through
[an application manifest](https://docs.cloudfoundry.org/devguide/deploy-apps/manifest.html#start-commands)
or on the
[`cf push` commandline](http://docs.cloudfoundry.org/cf-cli/getting-started.html#push).


## `Makefile`

The CppCMS headers and libraries will be available at the `cppcms/`
subdirectory, so a working command might look like:

```make
hello:
	c++ hello.cpp -o hello -Lcppcms/lib -Icppcms/include -lcppcms -lbooster -lz
```


## example `Makefile`

```make
default: hello

run:
	./hello -c cppcms.js

hello: hello.cpp
	c++ hello.cpp -o hello -Lcppcms/lib -Icppcms/include -lcppcms -lbooster -lz

clean:
	rm -f hello
```


## example app

There's a "hello world" app in `test/fixtures/hello-world` that you can deploy by using the command:

```
cf push appname -b https://github.com/cloudfoundry-community/cppcms-buildpack
```


## the libraries

The cppcms libraries were compiled by
[cloudfoundry/binary-builder](https://github.com/cloudfoundry/binary-builder),
specifically the code at
[this experimental branch](https://github.com/flavorjones/binary-builder/tree/flavorjones-cppcms).


## TODO

### static versus shared linking

currently the cppcms tarball contains both shared and static libraries. it shouldn't.

we may also want to figure out how to do static linking, but
[this page](http://cppcms.com/wikipp/en/page/ref_embedded#Static.Linking)
indicates it may not actually be easy or desirable.


### should config.js get handled in the default start command?

currently we're doing it in `.profile.d/cppcms.sh`


## License

Copyright 2016, Mike Dalessio

The buildpack is released under the Apache 2.0 license, see `LICENSE` for for the full text.

Compiled [CppCMS bits are distributed under the LGPLv3](http://commercial.cppcms.com/faq).

I am not a lawyer, and so have no idea if the previous two statements
are compatible with each other. Let me know if you have thoughts.

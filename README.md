# Boilerplate code for BrewBlox Service plugins

There is some boilerplate code involved when creating a Brewblox service plugin. This repository can be forked to avoid having to do the boring configuration.

Everything listed under **Required Changes** must be done before the package works as intended.

## How to use

* Fork this repository to your own Github account or project
* Follow all steps outlined under the various **Required Changes**
* Start coding your plugin =)


## Files

### [setup.py](./setup.py)
Used to create a distributable and installable Python package. See https://docs.python.org/3.6/distutils/setupscript.html for more information.

**Required Changes:** 
* Change the `project_name` variable to your project name. This is generally the same as the repository name. This name is used when installing the package through Pip.
* Change the `package_name` variable to the module name. This can be the same name as your project name, but can't include dashes (`-`). This name is used when importing your package in Python.
* Check whether all other fields are correct. Refer to the documentation for more info.
* Change the `url` parameter to the url of your repository.
* Change the `author` parameter to your name.
* Change the `author_email` parameter to your email.


### [tox.ini](./tox.ini)
This file kicks off automated testing and linting of your package. See http://tox.readthedocs.io/en/latest/config.html for more information.

**Required Changes:**
* Change `--cov=YOUR_PACKAGE` to refer to your module name.
* The `--cov-fail-under=100` makes the build fail if code coverage is less than 100%. It is optional, but recommended. Remove the `#` comment character to enable it.


### [Pipfile](./Pipfile)
Lists all dependencies. Everything under [packages] is needed for the package to run, while everything under [dev-packages] is needed to run the tests.

You can use `pipenv install <package name>` or `pipenv install --dev <package name>` to add dependencies.

**Note:** There is overlap between your requirements file, and the `install_requires=[]` line in `setup.py`. For most cases, the rule of thumb is that if you need an external package to run, you should add it as dependency to both files.

**Required Changes:**
* Install pipenv (run `sudo pip3 install pipenv`)
* Update the `Pipfile.lock` file (run `pipenv lock`)


### [MANIFEST.in](./MANIFEST.in)
This file lists all non-code files that should be part of the package. 
See https://docs.python.org/3.6/distutils/sourcedist.html#specifying-the-files-to-distribute for more info.

For a basic plugin, you do not need to change anything in this file.


### [.travis.yml](./.travis.yml)
Travis CI configuration. If you haven't enabled travis for your repository: don't worry, it won't do anything.


### [.coveragerc](./.coveragerc)
This file contains some configuration for `pytest-cov`.

For a basic plugin, you do not need to change anything in this file.


### [README.md](./README.md)
Your module readme (this file). It will be the package description on Pypi.org, and automatically be displayed in Github.

**Required Changes:**
* Add all important info about your package here. What does your package do? How do you use it? What is your favorite color?


### [YOUR_PACKAGE/](./YOUR_PACKAGE/)
Your module. This directory name should match the `package_name` variable in `setup.py`.

**Required Changes:**
* Rename to the desired module name. (Python import name. Can't include dashes (`-`)).


### [test/conftest.py](./test/conftest.py)
Project-level pytest fixtures. Some useful fixtures for testing any brewblox_service implementation are defined here. See tests in https://github.com/BrewBlox/brewblox-service/tree/develop/test for examples on how to use.

For a basic implementation, you do not need to change anything in this file.


### [test/test_hello.py](./test/test_hello.py)
An example on how to test aiohttp endpoints you added. Feel free to remove this once you no longer need it.


### [docker/amd/Dockerfile](./docker/amd/Dockerfile)
A docker file for running your package. To build, you need to copy the local version of your python package to `docker/dist/` first.

The Dockerfiles are set up so both the AMD (desktop) and ARM variants can use the same input files.

Example:
```bash
tox

rm docker/dist/*
cp .tox/dist/* docker/dist/

docker build \
    --tag your-package:your-version \
    --file docker/amd/Dockerfile \
    docker/

# run it
docker run your-package:your-version
```

**Required Changes:**
* Rename instances of `YOUR-PACKAGE` and `YOUR_PACKAGE` in the docker file to desired module and package names.


### [docker/arm/Dockerfile](./docker/arm/Dockerfile)
The same as for `docker/amd/Dockerfile`, but for Raspberry Pi targets.

In order to build for Raspberry, you must also first enable the ARM compiler.

Example:
```bash
tox

rm docker/dist/*
cp .tox/dist/* docker/dist/

# Enable ARM compiler
docker run --rm --privileged multiarch/qemu-user-static:register --reset

# Build the Raspberry Pi version
docker build \
    --tag your-package:rpi-your-version \
    --file docker/arm/Dockerfile \
    docker/

# Try to run Raspberry version
# On the desktop, this will fail with "standard_init_linux.go:190: exec user process caused "exec format error""
docker run --detach your-package:rpi-your-version
```


**Required Changes:**
* Rename instances of `YOUR-PACKAGE` and `YOUR_PACKAGE` in the docker file to desired module and package names.

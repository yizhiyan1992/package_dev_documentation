# package_dev_documentation
document how to develop a python package

Basic directory tree
------------------
pkg\_name  		<br>
|--readme.md		<br>
|--setup.py		<br>
|--setup.cfg		<br>
|--examples (folder)<br>
|--license.txt 	<br>
|--CONTRIBUTING.md <br>
|--CHANGELOG.md  <br>
|--MANIFEST.in	<br>
|--test (folder) <br>
|--environments (folder) <br>
|--requirements.txt<br>
|--pkg\_name	(folder)	<br>
|--codecov.yml		<br>

Document Page
------------------
**Q: whay should introduce at the document page?**

- what is this package for? The function and purposes (on the high level).
- How to install the package?
- The modules and explicit functions/features for each module.
- Examples
- license 

Naming Conventions
------------------
- 5 underscore patterns for naming conventions in Python:

|case|description|
|-----|----------|
|single leading underscore `_var` | indicating the name is for internal use. A hint for programmers and not enforced by programmers.|
|double leading underscore `__var` | trigger name mangling (aka name decorating) when used in class context. Enforced by python interpreter.|
|single trailing underscore `var_` | used by convention to avoid naming conflicts with python keyworks. (e.g. class_ ---> class)
| double leading and doule trailing `__var__` | built-in variables naming, should avoid naming those vars by ourself |
| underscore `_` | used as a name for temporary or insignificant variables. |

- constant variables should be all capital: CONSTANT
- name a package or module: lower case letters without using underscore (except `__init__.py`)
- name a class: use camel case style : `exampleClass`
- use plural nouns to name sequence data (`names=['Alice','Bob']`)
- For boolean vars, we can use `is_connect=True`

requirements.txt
-----------------
tbd

setup.py file
-----------------
tbd

setup.cfg
-----------------

__init__.py file
---
# chapter X. Distributing Python Module

1. An introduction to disutils/setuptools

An introduction to Disutils/setuptools
------------------
[website](https://docs.python.org/3/distutils/introduction.html#distutils-simple-example)

As a developer, your responsibilities are:

- write a setup script (setup.py by convention)
- (optional) write a setup configuration file
- create a source distribution
- (optional) create one or more built (binary) distributions

A simple example of setuptools:

```python
from setuptools import setup

# call the setup
setup(
	name="package_name",
	version="1.1.0",
	descriptipn="xxxx",
	author="xxx",
	packages=[pkg_name],
	python_requires=">=3.7",
	install_requires=[pkg_name,...],
	extras_requires={
		"nearest_neighbor":["scikit-learn","scipy"]},
)
```

A simple example of distutils:

```python
from distutils.core import setup
setup(
	name="foo",
	version="1.0",
	py_modules=['foo']
```
some obervations:

- the keywords arguments fall into two categories: package metadata (name, version) and information about what's in the package (a lits of python modules)
- Modules are specified by module name, not filename
- It is recommended to supply more metadata, in particular your name, email address, and a url for the project.

Run on the terminal: `python setup.py sdist`

sdist will create an archive file containing your setup script and your module. The archive file will be named `foo-1.0.tar.gz`, and will unpack into a directory `foo-1.0`.

If an end-user wishes to install your foo module, all they have to do is download the archive, unpack it, and from the `foo-1.-` directory run `python setup.py install`, which will ultimately copy `foo.py` to the appropriate directory for third-party modules in their Python installation.

Some general python terminology:

- module: the basic unit of code reusability in Python: a block of code imported by some other code. Three types of modules concer us here: 
	- pure python module: a module written in Python and contained in a single `.py` file (sometime associated with `.pyc` files)
	- extension module: a module written in the low-level language of the python implementation: C/C++ for python, Java for Jython.
	- package: a module that contains other modules; typically contained in a directory in the filesystem and distinguished from other directories by the presense of a file `__init__.py`.
- root package: the root of the hierarchy of the packages. (it does not have `__init__.py`.

Disutils-specific terminology

- module distribution: a collection of Python modules distrivuted together as a single downloadable resource and meant to be installed en masse.
- pure module distribution: only contain python files
- non-pure modile distribution: xxx

Writing the setup script
------------------------
The setup script is the center of all activity in building, distributing, and installing modules using the Disutils. The main purpose of the setup script is to describe your module distribution to the Distutils, so that various commands that operate on your modules do the right thing.

```python
#!/usr/bin/env python

from distutils.core import setup

setup(name='Distutils',
      version='1.0',
      description='Python Distribution Utilities',
      author='Greg Ward',
      author_email='gward@python.net',
      url='https://www.python.org/sigs/distutils-sig/',
      packages=['distutils', 'distutils.command'],
     )
```

### Listing whole packages
The `package` option tells the Distutils to process all pure Python modules found in each package mentioned in the `packages`. When you say `packages=['foo']` in the setup script, you are promising that the Distutils will find a file `foo/__init__.py`.

If the modules are in the different root module, use the following command to set up the root module: `package_dir={"":"lib"}`.(You are priomising that `lib/foo/__init__.py` exisit.)

You must explicitly list all packages in `packages`, the Distutils will not recursively scan your source tree looking for any directory with an `__init__.py` file.

### Listing individual modiules
For a small module distribution, you might prefer to list all modules rather than listing packages.`py_modules=['mod1','pkg.mod2']`.

### Describing extension modules
Need to create a Extension instance, and add into `ext_modules` arguments.

```python
from distutils.core import setup, Extension
setup(name='foo',
      version='1.0',
      ext_modules=[Extension('foo', ['foo.c'])],
      )
```

### Relationships between Distributions and Packages
A distribution may relate to packages in three specific ways:

1. It can require packages or modules
2. It can provide packages or modules
3. It can obsolete packages or modules

Dependencies on other Python modules and packages can be speficied by sypplying the `requires` keyword argument to `setup()`. For example `Shapely>=1.7, <2.0`.

Now that we can specify dependencies, we also need to be able to specify what we provide that other distributions can require. This is done using the `provides` keyword argument. Expression: `mypkg (1.1)`. If not specifying version, it will provide the distribution version.

A package can declare that it obsoletes other packages using the `obsolete` keyword argument.

### Installing scripts
Scripts are files containing Python source code, intended to be started from the command line.
```python
setup(...,
      scripts=['scripts/xmlproc_parse', 'scripts/xmlproc_val']
      )
``` 

### Installing Package Data

### Version of package
Encoding the version information is an art in itself. Python packages generally adhere to the version format `major.minor[.patch][sub]`

The major number is 0 for initial, experimental releases of software. It is increamented for releases that represent major milestones in a package.

The minor number is incremented when important new features aere added to the package.

The patch number increments when bug-fix releases are made.

Additional trailing version information is sometimes used to indicate sub-releases.
### Classifiers?

Writing a setup configuration file
---------------
Some people consider it bad style to put much logic in `setup.py`. To reflect that, `setup.cfg` (which is declarative by design) has become more popular for packaging.

`setup.cfg` is more about setting for any plug-ins or the type of distribution you wish to create. (flake8, pydocstring, etc.)

The basic syntax for `setup.cfg` is simple:

```python
[command]
option=value
```
where command is one of the Distutils commands, and option is one of the options that command supports. For example:

```python
[flake8]

max_line_length = 100
max_complexity = 15
exclude = ./build/*
```

Creating a source distribution
------------------------
`python setup.py sdist`

The default format is a gzio'ed tar file (.tar.gz) on unix, and ZIP file on Windows. You can specify as many formats as you like:
`python setup.py sdist --format=gztar, zip`

When building a source distribution for your package, bu default only a minimal set of files are included. You may find yourself wanting to include extra files in the source distribution. Adding, removing files to and from the source distribution is done by writing a `MANIFEST.in` file at the project root.

An example of MANIFEST.in:

```python
include *.txt
recursive-include examples *.txt *.py
prune examples/sample?/build
```

the `sdist` command builds the list of files to include in the Distutils source distribution:

- all Python source files implied by the py_modules and packages options
- all C source files mentioned in the ext_modules or libraries options
- scripts identified by the scripts option See Installing Scripts.
- anything that looks like a test script: test/test*.py (currently, the Distutils don’t do anything with test scripts except include them in source distributions, but in the future there will be a standard for testing Python module distributions)
- Any of the standard README files (README, README.txt, or README.rst), setup.py (or whatever you called your setup script), and setup.cfg.
- all files that matches the package_data metadata. See Installing Package Data.
- all files that matches the data_files metadata. See Installing Additional Files.

Creating a built distribution
------------------
A python distribution is a versiond compressed archive file that contains your python package. The distribution file is what an end-user (the client) willd download from the internet when they run `pip install`. There are two primary distribution types in the use today: `Built Distributions` and `Source Distribution`.

**Source Distribution**
`sdist` is very similar to source code - the code that you write. Therefore, `sdist` will not include platform-specific binaries. The result is an archive (.tar.gz) that contains the source code of your package and intructions on how to build it, and the target system of your client will perform the actual build to creat a bdist (wheel).

The advantage of source distribution is that creating an sdist is the same for all platforms (windows, linux, mac) and machines (32/64 bit). The disadvantage is that users have to build the package themselves once they download the sdist.

**Built Distribution**
Principally, `bdist` creates a distribution containing `.so`, `.dll`, `.dylib` for binary modules. The result is an archive that is specific to a platform and to a version of Python.

Installing a `bdist` in the client is immediate, as they don't need to build anything. The downside is that you have to build for multiple platforms and versions and upload all of the distributions for max campatibility.

Q: should I produce sdist or bdist for clients?

A: It is best practice to upload both, wheels and a source distribution.

The `bdist` command has a --format option, similar to the `sdist` command:
```python setup.py bdist --format=zip```

default on Unix: .tar.gz

default on Winodws: .zip

creating RPM packages:
`python setup.py bdist_rpm`

Distutils Examples
----------------------
1. **pure python distribution (by module)**

```python
<root>/
	setup.py
	foo.py
	bar.py
```
The setup script might be:

```python
from distutils.core inmport setup
setup(name="foobar",
	version='1.0',
	py_modules=['foo', 'bar']
```
2. **pure python distribution (package)**

```python
<root>/
        setup.py
        foobar/
                 __init__.py
                 foo.py
                 bar.py
                 subfoo/
                           __init__.py
                           blah.py
```

If you have sub-packages, they must be explicitly listed in packges.

```python
from distutils.core import setup
setup(name='foobar',
      version='1.0',
      packages=['foobar', 'foobar.subfoo'],
      )
```

3. **single extention module**
Create Extension instance

```python
from distutils.core import setup
from distutils.extension import Extension
setup(name='foobar',
      version='1.0',
      ext_modules=[Extension('foopkg.foo', ['foo.c'])],
      )
```

4. **checking a package**
the `check` command allows you to verify if your pakage meta-data meet the minimum requirements to build a distribution.

`$ python setup.py check`

### General steps for package development
1. choose your API (public and private methods, and add \_\_init\_\_.py for each folder)
2. Documentation (including docstrings and README.md)
3. Create a license
4. Rearrange for packaging (version and install requires)
5. Sign-up to PyPI
6. Build and Deploy

### Docstring
How to automatically generate docstrings for functions and classes?

Use `pyment` package.

```
pyment --output numpydoc test.py #generate a patch
patch -p1<test.py.patch #place the patch in the py file.
```

[GitHub](https://github.com/dadadel/pyment)

A brief guidline teaching how to use pyment [here](http://daouzli.com/blog/pyment.html)

Numpydoc style [here](https://numpydoc.readthedocs.io/en/latest/format.html)

### Sphinx with ReadTheDocs
- step 1: create the folder 

```
mkdir docs
cd docs
pip install sphinx
```
- step 2 : initalize the doc using `sphinx-quickstart` command

    configure the doc, Separate source and build directions [Y]
- step 3 : change the configuration file at source/conf.py

```python
import os
import sys # uncomment these two lines in the file

sys.path.insert(0,os.path.abspath('../../<pkg_name>')) # add the package directory into file

extensions = [
    'recommonmark', #for markdown
    'sphinx_markdown_tables', #for markdown
    'sphinx.ext.autodoc', # for auto document
    'sphinx.ext.napoleon', # for auto document
    'sphinx_autodoc_typehints', # for the style of auto document
    'numpydoc' # for the style of auto document
]

html_theme = 'sphinx_rtd_theme'

```
Note that if make html fails due to those extensions, use pip to install them. For instance `pip install sphinx_rtd_theme`.

- step 4 : build the ReadTheDocs

`make html` : build the html file.

`open build/html/index.html` : check the interface.

- **how to auto doc API functions?**
  1. create the <module_name>.rst
  2. In the rst file, inference the original module with following:
   
  ```
  GPS Tool API Refereces
  ==============================
  .. automodule:: gpstools
      :members:
  ```
  3. Add the rst file at the doctree in the index.rst file.


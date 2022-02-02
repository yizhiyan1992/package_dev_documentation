# package_dev_documentation
document how to develop a python package

Basic directory tree
------------------
pkg_name  		<br>
|--readme.md		<br>
|--setup.py		<br>
|--test_folder	<br>
|--example_folder<br>
|--pkg_name		<br>
|		<br>

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
# package_dev_documentation
document how to develop a python package

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
.. _sec_scopes:

Scopes
######

Each identifier (variables, functions, ...) defined in a C++ program has a *scope* (delimited by a pair of curly brackets ``{..}``), which is a region of the program where it can be used. In particular for variables, a scope defines its lifetime.

.. note:: You can also define global variables, outside of any pair of curly brackets, but this is usually considered bad practice.

One of the main feature of C++, one of the concept that makes C++ more that just C with classes is how you can handle resources (for example memory). In C, you usually need to create an object, then you can use it, and finally you need to delete the object to free its memory. In C++, resource 

.. RAII and scope

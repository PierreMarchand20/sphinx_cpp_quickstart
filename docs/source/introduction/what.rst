What is C++?
############

.. _sec_short_history:

Short History
-------------

C++ is programming language introduced by `Bjarne Stroustrup <https://en.wikipedia.org/wiki/Bjarne_Stroustrup>`__ in 1985. It is initially derived from the `C programming language <https://en.wikipedia.org/wiki/C_(programming_language)>`__, adding multiple new features.

.. warning:: It is possible to write C-like code in a C++ program, but it is generally not considered as a good practice. The common 

Since it has been introduced, C++ became standardized by the International Organization for Standardization (ISO), meaning that each new version of the language is defined by a technical document published by ISO and written by an international committee of experts.

The current versions are C++03, C++11, C++14, C++17, C++20 (the current version) and C++23 (the forthcoming version). In this document, we will mainly use features from C++14.

.. _sec_properties:

Properties of C++
-----------------

- `Compiled language <https://en.wikipedia.org/wiki/Compiled_language>`__: files containing C++ source code are designed to be compiled, meaning that: to be understood by a specific computer, they need to be translated to machine language for this specific computer. 
  
  Compilers are tools that execute this translation, but they can go even further and try to optimize the machine code it produces from the generic C++ code you wrote. Or they can also keep more information in the machine code to ease debugging, and give warnings when compiling.

  .. note:: Other compiled languages are fortran and C, while Python or matlab use a `different strategy <https://en.wikipedia.org/wiki/Interpreter_(computing)>`__

- Statically-type language, each variable in a program has a type known at compile type.

  .. note:: On the contrary, Python is dynamically-type language, each variable has a type, but it does not know the type before the program is executed.

- It has two main components: the core language and the `C++ Standard Library <https://en.wikipedia.org/wiki/C%2B%2B_Standard_Library>`__. The latter is a collection of containers and algorithms built on the former, and both are standardized by ISO.

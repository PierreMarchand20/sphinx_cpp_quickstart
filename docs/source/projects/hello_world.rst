Hello World project
####################

Before relying on CMake for every other project in this document, compile a C++ program by hand once, so you understand what CMake automates for you.

Single file
~~~~~~~~~~~

.. important:: Required reading: :ref:`first_cpp_program/first_example:hello world`, :ref:`first_cpp_program/basic_syntax:basic syntax`.

Write the following program in a file called ``main.cpp``:

.. code-block:: cpp
    :caption: main.cpp
    :name: project_hello_world_main

    #include <iostream>
    int main(){
        std::cout << "Hello world!\n";
        return 0;
    }

Compile and run it directly with ``g++``, with no CMake involved:

.. code-block:: bash

    g++ main.cpp -o hello_world
    ./hello_world

Two files
~~~~~~~~~

.. important:: Required reading: :ref:`first_cpp_program/compilation:c++ source files`, :ref:`first_cpp_program/compilation:separate compilation`.

Split the program into a declaration and a definition, as in :ref:`first_cpp_program/compilation:c++ source files`:

.. code-block:: cpp
    :caption: hello_world.hpp
    :name: project_hello_world_header

    #ifndef HELLO_WORLD_HPP
    #define HELLO_WORLD_HPP

    #include <iostream>
    void print();

    #endif

.. code-block:: cpp
    :caption: hello_world.cpp
    :name: project_hello_world_source

    #include "hello_world.hpp"
    void print(){
        std::cout << "Hello world!\n";
    }

.. code-block:: cpp
    :caption: main.cpp
    :name: project_hello_world_main_split

    #include "hello_world.hpp"
    int main(){
        print();
        return 0;
    }

Compile each source file into an object file, then link them by hand:

.. code-block:: bash

    g++ -c hello_world.cpp -o hello_world.o
    g++ -c main.cpp -o main.o
    g++ main.o hello_world.o -o hello_world
    ./hello_world

.. note:: Every other project in this document uses CMake instead of these manual commands, but it is worth having done it by hand at least once.

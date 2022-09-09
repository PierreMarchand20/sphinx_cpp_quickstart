.. _first_program:

Hello world
###########

In the following example, we write our first C++ function that prints "Hello world!" to the standard output of the terminal:

.. code-block:: cpp
    :caption: main.cpp
    :name: main_single_file

    #include <iostream>
    int main(){
        std::cout << "Hello world!\n";
        return 0;
    }

The function's name is **main**, it takes no argument, the output type is an integer, called **int** in C++, and it returns 0. Its content is delimited by the brackets, each statement is ended by ``;``, and to print the string ``"Hello world!\n"`` to the terminal, it uses ``std::cout <<``.

.. important:: One strength of C++ is its standard library, called `Standard Template Library <https://en.wikipedia.org/wiki/Standard_Template_Library>`_ (STL) which provides common data structures and functions. We use here the ``include`` statement to include parts of the STL, and in this example, the part related to inputs and outputs, which contained in particular the function ``cout``.

.. note:: The prefix ``std::`` (called namespace) is the common prefix for the STL, it is here to disambiguate the case where another included library would define functions with the same name (another ``cout`` function for example).

The function **main** is a special function in C++. It is the entry point of the C++ program, in other words, it is the function called when we execute the compiled code.

This is the source code of a simple *hello world* program that you can copy and paste in a file called ``main.cpp``. In the next section, you will see how to use it to produce a C++ program.

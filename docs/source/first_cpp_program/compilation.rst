Compilation
###########

C++ is a `compiled language <https://en.wikipedia.org/wiki/Compiled_language>`_, files containing C++ source code are designed to be compiled, meaning that: to be understood by a specific computer, they need to be translated to machine language for this specific computer.

Compilers are tools that execute this translation, and thus they are necessary to have on your workstation. Examples of such tools are `g++ <https://gcc.gnu.org>`_ and `clang <https://clang.llvm.org>`_.

Compilers can try to optimize the machine code it produces from the generic C++ code you wrote. Or they can lower their optimization to keep more information in the machine code to ease debugging, and to give warnings when compiling.


.. note:: Other compiled languages are fortran and C, while Python or matlab use a `different strategy <https://en.wikipedia.org/wiki/Interpreter_(computing)>`_

Manual
~~~~~~

Single file compilation
=======================


To compile the file introduced in :ref:`main_single_file`, you need to use the command associated to the compiler (``g++`` or ``clang`` for example). It will produce an executable called ``a.out`` by default. Then, you can try to execute it to print "Hello world!" in your terminal.

.. code-block:: bash

    g++ main.cpp
    ./a.out
    

They are actually two steps in producing an executable:

- The compiler produce object files, which contain machine code. They are usually ``.o`` files
- Then, it links the object files to produce an executable.

.. note:: 
    - To change the name of the output file, you can use the ``-o``  flag.

    .. code-block:: bash
        
        g++ main.cpp -o My_Awesome_Executable

    - The two steps of the compilation can be done separately. First produce the object file using ``-c`` flag.

    .. code-block:: bash
        
        g++ -c main.cpp

    It will produce the object file ``main.o``. And then you can do the linking from the object file to make the executable (``a.out``).

    .. code-block:: bash
        
        g++ main.o 


Multiple files compilation
==========================

Having a single large file have several disadvantages:

- each change requires recompiling everything,
- you cannot easily reuse part of your code in other program,
- it is difficult to read.

That is why it is good practice to divide your code into several files, containing related declarations. In the case of our simple program, we can encapsulate the line that prints "Hello world" to the standard output into a function called ``print``.

- You can put the declaration of the function into a *header file*, see :ref:`header_multiple_file`.
- You can put the definition of the function into a different file from ``main.cpp``, see :ref:`cpp_multiple_file`.
- And finally, you can just add an include statement in ``main.cpp`` to 

.. .. note:: test


.. code-block:: cpp
    :caption: Main file with multiple files: ``main.cpp``
    :name: main_multiple_file

    #include "hello_world.hpp"
    int main(){
        print();
        return 0;
    }


.. code-block:: cpp
    :name: header_multiple_file
    :caption: Header file with multiple files: ``hello_world.hpp``

    #ifndef HELLO_WORLD_HPP
    #define HELLO_WORLD_HPP

    #include <iostream>
    void print();

    #endif

.. code-block:: cpp
    :caption: Header file with multiple files: ``hello_world.cpp``
    :name: cpp_multiple_file

    #include "hello_world.hpp"
    void print(){
        std::cout << "Hello world!\n";
    }

Once you have done that, you compile your code as follows 

.. code-block:: bash

    g++ -c hello_world.cpp -o hello_world.o
    g++ -c main.cpp -o main.o
    g++ main.o functions.o -o main


- You can recompile separately ``main.cpp`` and ``hello_world.cpp``. So that you just need to recompile the files you modified, and redo the linking to produce the executable.
- You can potentially include ``main.cpp`` in some other program, making your code more easily available.
- And finally, the files are smaller making them easier to read.



Make
~~~~


.. code-block:: bash

    CC      = g++
    CFLAGS  = -g -Wall
    LDFLAGS =
    LIBRARY =
    INCLUDE =

    EXEC = main
    SRCS = main.cpp functions.cpp
    OBJS = main.o functions.o



    all : $(EXEC)

    $(EXEC) : $(OBJS)
    $(CC) $(LDFLAGS) -o $@ $^

    %.o : %.cpp
    $(CC) $(CFLAGS) -c -o $@ $<

    .PHONY: clean, mrproper

    clean :
    rm -rf *.o

    mrproper: clean
    rm -rf $(EXEC)


CMake
~~~~~

The standard structure for a C++ project is to put *header files* in a folder called ``include``, and source files in another folder called ``src``. Applying this structure to our simple example, you can then put the following ``CMakeLists.txt`` at the root of your projet


.. code-block:: cmake

    project(HelloWorld)
    cmake_minimum_required(VERSION 3.0)
    set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

    add_executable(main src/main.cpp src/hello_world.cpp)
    target_include_directories(main PRIVATE include)


    

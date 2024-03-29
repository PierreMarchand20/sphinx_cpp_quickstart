.. _sec_compilation:

Compilation
###########


C++ is a :ref:`compiled language <sec_properties>`, thus we need :ref:`compilers <sec_follow_along_compilers>` to translate C++ code to machine language and produce an executable. In this section, we will see how to build a C++ program using directly compilers, and then using more advanced tools such as :ref:`sec_make` and :ref:`sec_cmake`. But first, we need to better organize our files, and learn more about the process of compilation.


Files and compilation
~~~~~~~~~~~~~~~~~~~~~

.. _sec_source_files:

C++ Source files
================


Having a single file, like in our first :ref:`example <main_single_file>`, have several disadvantages:

- each change requires recompiling everything,
- you cannot easily reuse part of your code in other program,
- it can be difficult to read for large codebase.

That is why it is good practice to divide your code into several files. It is also good to separate *definition* and *declaration* of functions and classes. It makes it easier to read the interface of your functions and classes (names, type of inputs and outputs, etc.) given by the declarations, without having all the definitions at the same place.

.. important:: Declarations are usually written in *header files*, by convention ``.hpp``, and definitions are contained in ``.cpp`` files.

In the case of our simple program, we can encapsulate the line that prints "Hello world" to the standard output into a function called ``print``.

- You can put the declaration of the function into a *header file*, see :ref:`header_multiple_file`.
- You can put the definition of the function into a different file from ``main.cpp``, see :ref:`cpp_multiple_file`.
- And finally, you can just add an include statement in ``main.cpp`` to :ref:`main_multiple_file` 

In this new example, the ``include`` statements are used copy/paste the :ref:`header file <header_multiple_file>` so that both ``.cpp`` files have the declaration of the function ``print``.


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
    :linenos:

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

.. warning:: 
    In :ref:`header_multiple_file`, you can see the lines 1,2 and 7 are special, they are called *header guards*. They are here to ensure that the header file is copied/pasted only once in ``.cpp`` files. One common error that can happen without these, is to have a second header file including the first header, while having a ``.cpp`` file including both. In that case the first header would be copied/pasted twice without the header guards.


.. _sec_separation_compilation:

Separate compilation
====================

They are mainly two steps in producing an executable from source code files:

- The compiler produces object files for each C++ source code. They are usually ``.o`` files and contain machine code for every variables, functions and classes defined in their associated ``.o`` file. They also refer to functions and classes declared, but not defined in the headers.
- Then, it links the object files to produce an executable. One goal of this steps is for the object files to obtain the correct adresses to all the functions and classes compiled in other object files.

.. note:: More exactly, there is another step involving the preprocessor, but I suggest we focus on these two steps. 


.. _fig:

.. figure:: ../_static/svg/compilation.drawio.svg

   Compilation process

Compilation for the example from :ref:`sec_source_files` is illustrated in :ref:`fig`. One advantage of the compilation process is that a modification of the definition of the function ``print`` in ``hello_world.cpp`` will not require a recompilation of ``main.cpp`` for example.


Manual compilation
~~~~~~~~~~~~~~~~~~

Single file compilation
=======================


To compile the file introduced in :ref:`main_single_file`, you need to use the command associated to the compiler (``g++`` or ``clang`` for example). It will produce an executable called ``a.out`` by default. Then, you can try to execute it to print "Hello world!" in your terminal.

.. code-block:: bash

    g++ main.cpp
    ./a.out
    


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

.. important:: Here are a few important flags when using compilers:

    - ``-g`` generates debug information that can be used with debuggers, such as `GDB <https://www.sourceware.org/gdb/>`__.
    - ``-Wall`` enables almost all compiler warnings. It will help you understand errors in your code.

    In the context of this document, we recommend using both flags.


Multiple files compilation
==========================


Once you have done that, you compile your code as follows 

.. code-block:: bash

    g++ -c hello_world.cpp -o hello_world.o
    g++ -c main.cpp -o main.o
    g++ main.o hello_world.o -o main


- You can recompile separately ``main.cpp`` and ``hello_world.cpp``. So that you just need to recompile the files you modified, and redo the linking to produce the executable.
- And the files are smaller making them easier to read.


.. _sec_make:

Make
~~~~

If you have many files, multiple files compilation can be quickly cumbersome. `Make <https://www.gnu.org/software/make/>`__ is a tool made to leverage this issue. It will figure out automatically which file needs to be recompiled, and recompile just them.

Make knows which files to track, and how to recompile them using a file, called ``Makefile``, which lists the different target to build, and how to build them. Using the same example as previously, you can copy and paste the code from :ref:`makefile` into a file called `Makefile` alongside ``main.cpp``, ``hello_world.cpp`` and ``hello_world.hpp``.

.. code-block:: make
    :name: makefile
    :caption: Simple example of makefile

    CC      = g++
    CFLAGS  = -g -Wall
    LDFLAGS =
    LIBRARY =
    INCLUDE =

    EXEC = main
    SRCS = main.cpp hello_world.cpp
    OBJS = main.o hello_world.o

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

To use Make in your terminal, you can call 

- ``make`` to (re)compile your code, the executable will be named ``main``
- ``make clean`` to delete all the object files,
- ``make mrproper`` to delete all the object files and the executable.

You can modify ``hello_world.cpp``, and recall ``make`` to check that it will only recompile ``hello_world.cpp``.

.. _sec_cmake:

CMake
~~~~~

At least for me, it is tricky to write makefiles, and they are usually bound to one system. To make a project more platform-independent, but also to easily integrate a C++ project with your favourite source editor (see :ref:`compilation_vscode` for example), another tool is often used in C++ projects: `CMake <https://cmake.org>`__, which will generate a ``Makefile`` for your system.

But before setting up CMake, let's organize better our C++ project. The standard structure for a C++ project is to put *header files* in a folder called ``include``, and source files in another folder called ``src``. Applying this structure to our simple example, you can then put a ``CMakeLists.txt`` file containing :ref:`simple_cmake` at the root of your project. You can find an illustration of this structure in :ref:`cpp_structure`, where ``cpp_example`` is the name of the folder containing our project.

.. code-block:: bash
    :name: cpp_structure
    :caption: Simple structure for C++ project

    cpp_example
    ├── CMakeLists.txt
    ├── include
    │   └── hello_world.hpp
    └── src
        ├── hello_world.cpp
        └── main.cpp



.. code-block:: cmake
    :name: simple_cmake
    :caption: Simple example of ``CMakeLists.txt``
    :linenos:

    cmake_minimum_required(VERSION 3.5)
    project(HelloWorld)
    add_executable(main src/main.cpp src/hello_world.cpp)
    target_include_directories(main PRIVATE include)
    target_compile_features(main PRIVATE cxx_std_17)
    target_compile_options(main PRIVATE -Wall -fsanitize=address)
    target_link_options(main PRIVATE -fsanitize=address)


The content of :ref:`simple_cmake` is relatively self-explanatory:

1. ``cmake_minimum_required(VERSION 3.5)`` is used to require a modern-enough version of CMake.
2. ``project(HelloWorld)`` defines a CMake project.
3. ``add_executable(main src/main.cpp src/hello_world.cpp)`` defines an executable ``main`` whose source files are ``src/main.cpp`` and ``src/hello_world.cpp`` (paths are given relatively to ``CMakeLists.txt``).
4. ``target_include_directories(main PRIVATE include)`` specifies that to build ``main``, headers can also be found in ``include``.
5. ``target_compile_features(main PRIVATE cxx_std_17)`` set the C++ standard used.
6. ``target_compile_options(main PRIVATE -Wall -fsanitize=address)`` adds to the compilation, ``-Wall`` flag, which enables almost all warnings, ``-fsanitize=address``, which is a tool to catch errors at runtime (out-of-bounds accesses in an array for example).
7. ``target_link_options(main PRIVATE -fsanitize=address)`` adds to the linking, ``-fsanitize=address``.

Once you have structured your C++ project and prepared ``CMakeLists.txt``, you can use :ref:`generate_makefile_cmake` to generate all the necessary files and a Makefile into a folder ``build`` in ``cpp_example``.

.. code-block:: bash
    :name: generate_makefile_cmake
    :caption: Generate Makefile

    mkdir build
    cd build
    cmake ../

.. note:: CMake projets usually use an *out-of-source* build strategy, meaning everything build-related will be in a separate folder from the sources (here `include` and `src`). It is considered good practice:

    - It separates source files, whose content is likely to be independent of the operating system and compilers, from files generated for your particular computer, compiler and compilation options. When using a version control system like `Git <https://pmarchand.pages.math.cnrs.fr/computertools/basic_tools/git.html>`__, you can ask it to ignore "build" folders, and to keep track of the source files
    - You can generate several "build" folders (using different compilers, or different compilation options)

Now that the CMake project is generated, you can call ``make`` in ``cpp_example/build`` to generate the executable.

.. warning:: You may want to remove the flag ``-fsanitize=address`` from compilation and linking if you want better performance from the resulting executable. But in all other cases, it's best to keep it.


.. _compilation_vscode:

Integration with IDEs
~~~~~~~~~~~~~~~~~~~~~

By default, CMake will generate a *Unix Makefile* which can then be used to build our C++ program. But CMake can be used to generate many types of "projects" that can be used in an Integrated Development Environment (IDE): Visual Studio, XCode, CodeBlocks, etc, (see `documentation <https://cmake.org/cmake/help/latest/manual/cmake-generators.7.html>`__).

Compilation in VS Code
======================

If you use VS Code, you can also easily integrate your C++ program using CMake. We refer to VS Code `documentation <https://code.visualstudio.com/docs/cpp/cmake-linux>`__ for using C++ and CMake. But to summarise:

- You need to install the extension `CMake Tools <https://marketplace.visualstudio.com/items?itemName=ms-vscode.cmake-tools>`__.
- Open the folder containing ``CMakeLists.txt`` with VS Code (``cpp_example`` in our example).
- Use the Command Palette (``maj+ctrl/cmd+P``) and run **CMake: Select Kit** to select a compiler.
- Use the Command Palette (``maj+ctrl/cmd+P``) and run **CMake: Select Variant** to select a build type (mainly *Release*, asking the compiler to include optimizations, or *Debug*, which is needed for debugging).
- Use the Command Palette (``maj+ctrl/cmd+P``) and run **CMake: Configure** to create a ``build`` folder and generate the makefile, equivalent to :ref:`generate_makefile_cmake`.
- Use the Command Palette (``maj+ctrl/cmd+P``) and run **CMake: Build** to generate the executable.

Debugging in VS Code
====================

If you want to run your executable with a debugger, `GDB <https://www.sourceware.org/gdb/>`__ for example, and you already built your code using *Debug* mode before configuration,

- Use the Command Palette (``maj+ctrl/cmd+P``) and run **CMake: Debug** to run and debug the executable.

To help you debug your C++ program, you can use *breakpoints*:

- Click on the left of the line number, it will appear as a red dot.

When running in debug, the C++ program will stop at the breakpoints, you will then see all the current variables, and you will be able to go to the next statement or the next breakpoint. See `Debugging documentation <https://code.visualstudio.com/docs/editor/debugging#_breakpoints>`__ and the `Debugging C++ documentation <https://code.visualstudio.com/docs/cpp/cpp-debug>`__ for more information.

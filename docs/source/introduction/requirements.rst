To follow along
###############

It is assumed that the reader has a unix-like operating system (Linux, macOS,...). In other words, we assume the reader do not have Windows, but see `About Windows <https://pmarchand.pages.math.cnrs.fr/computertools/introduction/setup.html#about-windows>`__ from `Computer Tools`_ if you are in this case.

I also assume that the reader has basic knowledge about tools such as bash. If this is not the case, we refer to `Computer Tools`_, and in particular its section on `basic tools <https://pmarchand.pages.math.cnrs.fr/computertools/basic_tools/index.html>`__.

.. _sec_follow_along_compilers:

Compilers
---------

C++ is a :ref:`compiled language <sec_properties>`, and thus we need tools to translated C++ code to machine language. Compilers are tools that execute this translation, and thus they are necessary to have on your workstation. Examples of such tools are `g++ <https://gcc.gnu.org>`__ and `clang++ <https://clang.llvm.org>`__. See their documentation for installation instructions. 

.. note:: They are very common tools, and thus, they are usually already available, or they can be easily installed, via a package manager for example.

.. important:: In the following examples, we will use ``g++``, you can just replace this command name by ``clang++`` if that is the available compiler on your machine.

Other tools
-----------

As for most languages, several tools can be used to improve your development environment:

- Syntax colouring,
- Code Formatting,
- Static-analyser,
- Debugger,
- Code completion
- Code navigation,

Visual Studio Code
------------------

Most tools are available or can be integrated with `VS Code <https://code.visualstudio.com>`__ using the `C/C++ extension <https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools>`__. See also its `documentation <https://code.visualstudio.com/docs/cpp/introvideos-cpp>`__ on using C++ in VS Code.

If you are not familiar to VS Code, you can also check out `this section <https://pmarchand.pages.math.cnrs.fr/computertools/introduction/setup.html#visual-studio-code>`__ from `Computer Tools`_.

Basic syntax
############

Code examples should be put in a function *main* and compiled as we have seen in :ref:`first_cpp_program/compilation:compilation`.

Variables and types
~~~~~~~~~~~~~~~~~~~

C++ requires every variable to be declared with its type. And the type of the variable cannot be changed during its lifetime. See :ref:`code_type`.

.. code-block:: cpp
    :name: code_type
    :caption: Example for declaring and initializing variables

    // Declarations
    int a;
    float b;
    double c;

    // Initializations
    int d{1};
    float e{2.0};
    double f{3.0};


It tells the compiler the size it needs to reserve in memory for every variable, and it also allows the compiler to check that every operation makes sense! If you try to compile :ref:`code_wrong_type`, you will get an error in your terminal:

.. code-block:: cpp
    :name: code_wrong_type
    :caption: Example of error at compilation

    int a{1};
    std::string str{"a string"};
    std::cout << a + str;

    /* Error from compiler:
    error: invalid operands to binary expression ('int' and 'std::string' (aka 'basic_string<char, char_traits<char>, allocator<char>>'))
    std::cout << a + str;
                 ~ ^ ~~~
    */

.. warning:: ``std::string`` is not part of the core language, but it is defined in the :ref:`stl/index:c++ standard library`. You need to add ``#include <string>``.

Statements and flow control
~~~~~~~~~~~~~~~~~~~~~~~~~~~

All the usual loops are available:

- If statement: ``if (condition) {statement;}`` where ``condition`` is an expression returning a boolean. You can use the usual `condition operators <https://en.cppreference.com/w/cpp/language/operator_comparison>`__. See :ref:`code_if_statement`
- For loop: ``for (initialization; condition; increase) {statement;}``. See :ref:`code_for_loop`
- While loop: ``while (expression) {statement;}``. See :ref:`code_while_loop`

.. code-block:: cpp
    :name: code_if_statement
    :caption: Example of if statement

    int x{10};
    if (x>1){
        std::cout << "x is greater than 1";
    }
    else if (x==1){
        std::cout << "x is 1";
    }
    else {
        std::cout << "x is lower than 1";
    }

.. code-block:: cpp
    :name: code_for_loop
    :caption: Example of for loop

    for (int n=0; n<10; n++) {
        std::cout << n << ", ";
    }

.. code-block:: cpp
    :name: code_while_loop
    :caption: Example of while loop
    
    int n{0};
    while (n<10){
        std::cout << n << ", ";
        n+=1;
    }


Functions
~~~~~~~~~

A function's declaration is composed by its name, its return type, its parameters' type, this set is also called the function's *prototype*. As you can see in :ref:`code_functions`, a function can also have no parameter or a return type ``void``, meaning that it returns nothing.


.. code-block:: cpp
    :name: code_functions
    :caption: Examples of functions

    void print(){
        std::cout << "Hello world!" <<"\n";
    }

    void print(int a){
        std::cout << a <<"\n";
    }

    int add(int a, int b){
        return a+b;
    }


.. important:: Functions can be overloaded. It means that you can define functions with the same name, but different parameters. See the two print functions in :ref:`code_functions`. But you cannot overload functions with only their return type. In our examples, you cannot define ``int print()``.


References
~~~~~~~~~~

A reference in C++ can be seen as an alias for a variable, it is just a new name for a variable. For a variable of type ``T``, the type for references to variables of such type is ``T&``. See :ref:`code_reference` where we used a ``string`` variable, and a reference to this variable, whose type is ``string &``.


.. code-block:: cpp
    :name: code_reference
    :caption: Examples of reference

    std::string a = "Not modified";
    std::string& b=a;
    b="Modified";
    std::cout << a << "\n";


The primary use of references is related to function parameters. In :ref:`code_functions`, we passed input parameters *by value*, meaning that the variables given as argument to a calling function are copied to other variables which are used in the body of the function. You can observe it in :ref:`code_functions_by_value` where we modify the argument of the function in the function body, but this does not modify the argument given in the function call. In this example, ``a`` is copied to ``b`` which is used in the function body.

.. code-block:: cpp
    :name: code_functions_by_value
    :caption: Example of passing parameters by value

    #include <string>
    #include <iostream>

    void example_by_value(std::string b){
        b = "Modified";
        std::cout << b << "\n";
    }

    int main(){
        std::string a = "Not modified";
        example_by_value(a);
        std::cout << a << "\n"; // prints Not Modified
    }

To summarize, when passing by value:

- The variables used as arguments in the function call cannot be modified.
- A copy is done between the variables used as arguments in the function call, and the variables used in the function body. This copy can be costly if the variable has a large size (with a large array for example).


An alternative is to pass parameters *by reference*. In :ref:`code_functions_by_reference`, ``b`` is an alias for ``a``, but the content is the same. Modifying ``b`` does modify ``a``.

.. code-block:: cpp
    :name: code_functions_by_reference
    :caption: Example of passing parameters by reference

    #include <string>
    #include <iostream>

    void example_by_reference(std::string& b){
        b = "Modified";
        std::cout << b << "\n";
    }

    int main(){
        std::string a = "Not modified";
        example_by_reference(a);
        std::cout << a << "\n"; // prints Modified
    }

To summarize, when passing by reference:

- The variables used as arguments in the function call can be modified.
- There is no copy!
  
But, what if we want to avoid copying an argument of the function (because it is expensive), but we also want to prohibit modifying it? In this case, we can tell the compiled that the variable is a *constant reference*, meaning that this is an alias, but it cannot modify the content of the variable. A reference for a type ``T`` is ``T&``, a constant reference for a type ``T`` is ``const T&``. :ref:`code_functions_by_constreference` does not compile because we try to modify a ``const int&``.

.. code-block:: cpp
    :name: code_functions_by_constreference
    :caption: Example of passing parameters by ``const`` reference

    // This example cannot be compiled! (thanks the compiler)
    #include <string>
    #include <iostream>

    void example_by_const_reference(const std::string& b){
        b = "Modified";
        std::cout << b << "\n";
    }

    int main(){
        std::string a = "Not modified";
        example_by_const_reference(a);
        std::cout << a << "\n";
    }

    /* Error from compiler:
    error: no viable overloaded '='
    b = "Modified";
    ~ ^ ~~~~~~~~~~
    ******string:905:19: note: candidate function not viable: 'this' argument has type 'const std::string' (aka 'const basic_string<char, char_traits<char>, allocator<char>>'), but method is not marked const
    basic_string& operator=(const basic_string& __str);
    */

.. note:: If you use an IDE with a static analysis tool, you do not even need to compile to see that there is an issue in :ref:`code_functions_by_constreference`. The IDE should tell you that ``b= "Modified"`` is not possible. But if you try to compile, the compiler will show an error.


Scope and RAII
~~~~~~~~~~~~~~

Each identifier (variables, functions, ...) defined in a C++ program has a *scope* (delimited by a pair of curly brackets ``{..}``), which is a region of the program where it can be used. In particular for variables, a scope defines its lifetime. Objects are destructed in reverse order of initialization when exiting their scope.

.. note:: You can also define global variables, outside any pair of curly brackets, but this is usually considered bad practice.

One of the main feature of C++, one of the concept that makes C++ more that just C with classes is how you can handle resources (for example memory, files). In C, you need to allocate a resource, so you can use it, and then you need to delete the object to free its memory. In C++, resources can be handled automatically by binding their life cycle to the lifetime of an object.

.. important:: This feature is called `RAII <https://en.cppreference.com/w/cpp/language/raii>`__ for *Resource Acquisition Is Initialization*, but one important point is that the resource is released automatically when the associated object is destructed.

Examples will be given in :ref:`stl/memory:pointers` for managing automatically memory allocation. 

Function templates
##################

Definition
----------

To parametrize the types used in a function, we define *template types* as in line 1 from :ref:`code_generic_function_template_definition`. Function template can have both template parameters and non-template parameters like ``multiply_by_N``.

.. code-block:: cpp
    :caption: Function template definition
    :name: code_generic_function_template_definition
    :linenos:
    :emphasize-lines: 1,6

    template<typename T>
    T multiply_by_ten(T a){
        return a * 10;
    }

    template <typename T>
    T multiply_by_N(T a, int N){
        return a * N;
    }

.. important:: A function template cannot be compiled directly as is. Since it is defined for generic/abstract types, it does not represent an actual definition of a function that can be compiled into language machine. Thus, definition **and** declaration should be in header files. 

.. note:: Function templates are really "templates" for defining actual functions at compilation time. Thus, they allow generating code, and that is why templates in C++ are referred as a tool for metaprogramming.



Use
---

To use a function generated from a function template, we can do as we saw with ``std::vector`` in :ref:`stl/container:arrays`, specifying the actual type. Or, we can rely on the compiler *template argument deduction*, where it uses the types of the arguments to deduce the template argument. Both are illustrated in :ref:`code_generic_function_template_use`.

.. code-block:: cpp
    :caption: Function template use
    :name: code_generic_function_template_use
    :linenos:
    :emphasize-lines: 7-10

    template<typename T>
    T multiply_by_ten(T a){
        return a * 10;
    }

    int main(){
        std::cout << multiply_by_ten<int>(2)<<"\n"; // 20
        std::cout << multiply_by_ten(2)<<"\n"; // 20
        std::cout << multiply_by_N(2,2)<<"\n"; // 4
        // std::cout << multiply_by_ten("2")<<"\n"; // does not compile
    }

.. note:: Function template may not compile if the substitution of the template type does not lead to meaningful operations. In :ref:`code_generic_function_template_use`, ``multiply_by_ten("2")`` cannot compile because ``operator*`` is not defined in ``"2"*10``.

Specialization
--------------

The definition of a template function can be modified for a particular value of template type. This is called *template specialization*.

.. code-block:: cpp
    :caption: Function template specialization
    :name: code_generic_function_template_specialization
    :linenos:
    :emphasize-lines: 8-11

    #include <iostream>

    template <typename T>
    void print(T a){
        std::cout << a << "\n";
    }

    template <>
    void print(std::pair<int, int> a){
        std::cout << a.first << " " << a.second << "\n";
    }

    int main(){
        print(2); 
        print(std::pair<int,int>(2,3)); // would not compile without specialization.
    }

.. note:: If a function template has several template types, specialization will work only if all template types are specialized.

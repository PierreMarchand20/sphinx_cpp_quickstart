Containers
##########

Arrays
~~~~~~

One of the most common containers is an array, meaning values of a given type contiguously stored in memory. In C++, there are two types of arrays:

- Static arrays, whose size is known at compile type, which implies that they have a fixed size.
- Dynamic arrays, whose size can vary during execution.

Both are available in the C++ standard library, with respectively ``std::array`` and ``std::vector``. To interact with objects of these types, you can use *methods*, functions that belong to the definition of these types. To call a method called ``call_method``, which takes an integer as argument, on an object called ``my_object``, you need to write ``my_object.call_method(3)``. In :ref:`code_arrays`, you can see how to use vectors and arrays from STL, and some of their methods. See the section "Member functions" in the documentation of `std::vector <https://en.cppreference.com/w/cpp/container/vector>`__ and `std::array <https://en.cppreference.com/w/cpp/container/array>`_


.. code-block:: cpp
    :caption: Example of arrays
    :name: code_arrays


    #include <iostream>
    #include <vector>
    #include <array>

    int main()
    {   
        std::vector<int> zero_array_of_size_10(10,0);
        std::vector<int> dynamic_array{1, 2, 3, 4};
        std::array<int, 3> static_array{1, 2, 3};

        // Sizes
        std::cout << dynamic_array.size() << " " << static_array.size() << "\n";

        // Access element
        std::cout << dynamic_array[2] << " " << static_array[2] << "\n";

        // Modify vector and accessing last element
        dynamic_array.push_back(5);
        std::cout << dynamic_array.size() << " " << dynamic_array.back() << "\n";

        return 0;
    }

.. important:: As you can see in :ref:`code_arrays`, ``std::vector`` and ``std::array`` are followed respectively by ``<int>`` and ``<int,3>``. They are *template arguments*, i.e., arguments parameters that indicate at *compile time* information about the actual type used. In the case of ``std::vector``, it means that it is a contiguous array of ``int``, while for ``std::array``, it means that it is a static array of ``int`` whose size is 3.


To loop over the elements of a vector or an array, you could use the for loop we saw in :ref:`first_cpp_program/basic_syntax:statements and flow control` accessing elements and using the ``size`` method, but a convenient alternative is to use *range-based for loops*. The advantage of this approach is that you are guaranteed to only loop on the elements of the vector, which is not the case of the traditional for loop where segmentation fault errors can occur if the loop goes too far. 

.. code-block:: cpp
    :caption: Example of range-based loops
    :name: code_loop_arrays

    #include <iostream>
    #include <vector>

    int main(){
        std::vector<int> dynamic_array{1, 2, 3, 4};
        for (int i : dynamic_array) // access by value
        {
            std::cout << i << " ";
            i = i + 1;
        }
        std::cout << "\n";

        for (int &i : dynamic_array) // access by reference
        {
            std::cout << i << " ";
            i = i + 1;
        }
        std::cout << "\n";

        for (const int &i : dynamic_array) // access by const reference
        {
            std::cout << i << " ";
        }
        std::cout << "\n";

        return 0;
    }


Other types of containers
~~~~~~~~~~~~~~~~~~~~~~~~~

As you may know, there are many types of data containers, which may have different operations available, with different complexities. Here are a few examples (see also `documentation <https://en.cppreference.com/w/cpp/container>`_): 

- Sequential containers: `std::array <https://en.cppreference.com/w/cpp/container/array>`__, `std::vector <https://en.cppreference.com/w/cpp/container/vector>`__, linked list as `std::list <https://en.cppreference.com/w/cpp/container/list>`__, ...
- Associative containers: `std::set <https://en.cppreference.com/w/cpp/container/set>`__, dictionary or the are called in C++ `std::map <https://en.cppreference.com/w/cpp/container/map>`__, ...
- Container adaptors: `std::stack <https://en.cppreference.com/w/cpp/container/stack>`__, `std::queue <https://en.cppreference.com/w/cpp/container/queue>`_, ...

Choosing the right type of container depends on the type of operations you need, and their cost. In the documentation, the complexity is reminded. For example, `access to an element <https://en.cppreference.com/w/cpp/container/vector/operator_at>`__ of a `std::vector <https://en.cppreference.com/w/cpp/container/vector>`__ via an index is constant, while access to an element of a `std::map <https://en.cppreference.com/w/cpp/container/map>`__ via its key is logarithmic in the size of the container.

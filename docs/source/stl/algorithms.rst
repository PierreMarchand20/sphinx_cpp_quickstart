Algorithms
##########

Many standard algorithms are already implemented in the STL, and they can be easily used with standard :ref:`stl/container:arrays`, and even :ref:`stl/container:other types of containers` if relevant. You can find more about them in the documentation of the `algorithm library <https://en.cppreference.com/w/cpp/algorithm>`__.

Searching, sorting and swapping
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: cpp
    :caption: Example of STL algorithms
    :name: code_stl_algorithm

    #include <algorithm>
    #include <iostream>
    #include <vector>

    int main()
    {
        std::vector<int> dynamic_array{1, 4, 3, 2, 6, 5, 2};
        std::cout << std::count(dynamic_array.cbegin(), dynamic_array.cend(), 2) << "\n"; // count the number of element 2
        std::sort(dynamic_array.begin() + 2, dynamic_array.end());                        // sort starting from the third element
        std::swap(dynamic_array.front(), dynamic_array.back());                           // swap the first element with the last element

        for (const int &i : dynamic_array) // access by const reference
        {
            std::cout << i << " ";
        }
        std::cout << "\n";

        return 0;
    }

Lambdas
~~~~~~~

Some algorithms, such as `std::count_if <https://en.cppreference.com/w/cpp/algorithm/count>`__, `std::find_if <https://en.cppreference.com/w/cpp/algorithm/find>`__ or `std::sort <https://en.cppreference.com/w/cpp/algorithm/sort>`__ with a custom comparison, need a *callable* (something that can be called like a function) as an argument, to define, respectively, a condition, or how to compare two elements. Defining a named function for this can be cumbersome for something only used once, so C++ allows defining an unnamed function directly where it is needed, called a *lambda expression*.

A lambda has the form ``[capture](parameters){body}``, where ``parameters`` and ``body`` are just like for a regular function. ``capture`` specifies which variables from the surrounding scope can be used inside the lambda's body, and how: ``[&]`` captures every used variable by reference, ``[=]`` captures every used variable by value, and you can also list variables explicitly, such as ``[x, &y]``, to capture ``x`` by value and ``y`` by reference.

.. code-block:: cpp
    :caption: Example of algorithms using lambdas
    :name: code_stl_algorithm_lambda

    #include <algorithm>
    #include <iostream>
    #include <vector>

    int main()
    {
        std::vector<int> dynamic_array{1, 4, 3, 2, 6, 5, 2};

        // count the number of elements greater than 3
        int threshold = 3;
        int count = std::count_if(dynamic_array.cbegin(), dynamic_array.cend(),
                                   [threshold](int i) { return i > threshold; });
        std::cout << count << "\n";

        // sort in decreasing order, instead of the default increasing order
        std::sort(dynamic_array.begin(), dynamic_array.end(), [](int a, int b) { return a > b; });

        // apply an operation to each element
        std::for_each(dynamic_array.begin(), dynamic_array.end(), [](int &i) { i *= 2; });

        for (const int &i : dynamic_array) // access by const reference
        {
            std::cout << i << " ";
        }
        std::cout << "\n";

        return 0;
    }

.. note:: The compiler generates, for each lambda expression, its own unique unnamed type. This is why a variable storing a lambda is usually declared with ``auto``, rather than trying to write its type explicitly.

Reductions
~~~~~~~~~~

Some algorithms compute a single result from a whole range, instead of searching, counting or reordering elements. `std::min_element <https://en.cppreference.com/w/cpp/algorithm/min_element>`__ and `std::max_element <https://en.cppreference.com/w/cpp/algorithm/max_element>`__ (in ``<algorithm>``) find the smallest and largest elements, and `std::accumulate <https://en.cppreference.com/w/cpp/algorithm/accumulate>`__ (in ``<numeric>``) combines every element into one value, starting from a given initial value and, by default, using ``+``.

.. code-block:: cpp
    :caption: Example of reduction algorithms
    :name: code_stl_algorithm_reduction

    #include <algorithm>
    #include <iostream>
    #include <numeric>
    #include <vector>

    int main()
    {
        std::vector<int> dynamic_array{1, 4, 3, 2, 6, 5, 2};

        auto min_it = std::min_element(dynamic_array.cbegin(), dynamic_array.cend());
        auto max_it = std::max_element(dynamic_array.cbegin(), dynamic_array.cend());
        std::cout << "min: " << *min_it << ", max: " << *max_it << "\n";

        int sum = std::accumulate(dynamic_array.cbegin(), dynamic_array.cend(), 0);
        std::cout << "sum: " << sum << "\n";

        // a fourth argument replaces + with a custom combining operation, here a product
        int product = std::accumulate(dynamic_array.cbegin(), dynamic_array.cend(), 1, [](int acc, int i) { return acc * i; });
        std::cout << "product: " << product << "\n";

        return 0;
    }

.. important:: ``std::min_element`` and ``std::max_element`` return an *iterator* to the found element, not the element itself, so it needs to be dereferenced with ``*`` to get its value, just like a :ref:`pointer <stl/memory:pointers>`. This also lets you know *where* the element is in the range, not just its value.

    This iterator's type is verbose and rarely useful to spell out, which is why :ref:`first_cpp_program/basic_syntax:type deduction with auto` is used to declare ``min_it`` and ``max_it`` above.

.. warning:: ``std::accumulate`` always needs an explicit initial value. This value also fixes the type of the accumulated result: using ``0`` above produces an ``int``, but if the elements were, e.g., ``double``, an initial value of ``0`` would silently truncate the result to an integer; ``0.0`` should be used instead.

Transform
~~~~~~~~~

`std::transform <https://en.cppreference.com/w/cpp/algorithm/transform>`__ applies a function to every element of a range, and writes each result into a destination range: it does not modify its input, unlike ``std::for_each`` in :ref:`stl/algorithms:lambdas`. It needs a third iterator, pointing at the beginning of where the results should be written.

If the destination already has the right size, its ``begin()`` can be used directly, as in :ref:`code_stl_algorithm_transform`. Otherwise, `std::back_inserter <https://en.cppreference.com/w/cpp/iterator/back_inserter>`__ (from ``<iterator>``) can be used on an initially empty container: instead of writing through the iterator, it calls ``push_back`` for every result.

.. code-block:: cpp
    :caption: Example of ``std::transform``
    :name: code_stl_algorithm_transform

    #include <algorithm>
    #include <iostream>
    #include <iterator>
    #include <vector>

    int main()
    {
        std::vector<int> dynamic_array{1, 4, 3, 2, 6, 5, 2};

        // destination already has the right size: write directly into it
        std::vector<int> squares(dynamic_array.size());
        std::transform(dynamic_array.cbegin(), dynamic_array.cend(), squares.begin(),
                        [](int i) { return i * i; });

        // destination starts empty: grow it with back_inserter
        std::vector<int> even_or_odd;
        std::transform(dynamic_array.cbegin(), dynamic_array.cend(), std::back_inserter(even_or_odd),
                        [](int i) { return i % 2 == 0; });

        for (const int &i : squares)
        {
            std::cout << i << " ";
        }
        std::cout << "\n";

        return 0;
    }

.. note:: The source and destination ranges can be the same container, which lets ``std::transform`` also be used to modify elements in place, like ``std::for_each``.

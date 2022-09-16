Algorithms
##########

Many standard algorithms are already implemented in the STL, and they can be easily used with standard :ref:`sec_arrays`, and even :ref:`sec_other_containers` if relevant.


.. code-block:: cpp
    :caption: Example of STL algorithms
    :name: code_stl_algorithm

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

.. _sec_scopes:

Memory management
#################


.. _sec_pointers:

Pointers
~~~~~~~~

A pointer is a variable that stores the address in memory of a variable. For example, in :ref:`code_pointer_variable`, ``pointer_to_a`` is a variable containing the address of ``a``, and to get the address from ``a`` we used ``&``. If you try to display it with ``std::cout``, it will print its address in memory. You can directly use methods from the pointed variable's type using ``->``, or you can access the underlying variable by "dereferencing" the pointer using ``*``.

.. code-block:: cpp
    :caption: Pointer variable
    :name: code_pointer_variable

    std::vector<int> a {0,1,2,3};
    std::vector<int>* pointer_to_a = &a;
    std::cout << pointer_to_a <<" "<< pointer_to_a->size() <<" "<< (*pointer_to_a)[0] <<"\n";

:ref:`sec_pointers` and :ref:`sec_references` are similar in the sense that they are both mechanisms to access an underlying variable. But there are key differences that we highlight here:

.. list-table:: References vs pointers
   :widths: 25 25
   :header-rows: 1

   * - Pointers
     - References
   * - Can be assigned to ``nullptr``
     - Cannot be null
   * - Can be re-assigned
     - Cannot be re-assigned


.. _sec_smart_pointer:

Smart pointers
~~~~~~~~~~~~~~

Common issues with pointers appear when a pointer "owns" the variable it points to, in other words, when it manages the lifetime of the underlying variable. With "raw" pointers as in the :ref:`previous <sec_pointers>` section, you need to allocate manually, give its address to the pointer, and free the memory manually before the end of the program. Typical errors are 

- forgetting to free the memory, *memory leaks*,
- freeing the memory too early, so that the pointer points to nothing, *dangling pointers*.

To avoid these issues, the idea is to use :ref:`RAII <sec_scope>`. We tie the lifetime of the underlying variable to the lifetime of a type representing a pointer which owns its data. The latter is implemented in the C++ standard library as ``std::unique_ptr``. 

To create a ``std::unique_ptr<T>`` to an object of type ``T``, one has to use ``make_unique<T>`` whose input parameters are the ones used to construct directly an object ``T``.

.. code-block:: cpp
    :caption: Example with ``std::unique_ptr``
    :name: code_unique_ptr

    // you need to use #include <memory>
    std::unique_ptr<std::vector<int>> owning_ptr_to_a_vector;
    owning_ptr_to_a_vector = std::make_unique<std::vector<int>>(10);
    std::cout << owning_ptr_to_a_vector->size() << "\n"; // its size is 10

.. note:: In C, one uses ``malloc`` and ``free`` to allocate and free memory. In C++, it is also possible with ``new`` and ``delete``, but it is discouraged since ``std::unique_ptr`` does it for you automatically. But it is not rare to see manual memory management in legacy C++ code, or specialized library.

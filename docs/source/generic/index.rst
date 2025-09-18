Templates
#########

Templates are a C++ mechanism that allows writing more generic code by parametrizing types. We already encountered this when using ``std::vector`` in :ref:`stl/container:arrays`. Indeed, we saw that ``std::vector`` was followed by ``<int>``, specifying the actual type of the stored elements. Types and functions from the :ref:`stl/index:c++ standard library` are usually defined for generic types. For example, one could use ``std::vector<double>``, ``std::vector<std::string>`` or even user-defined :ref:`object_oriented/classes_objects:classes and objects` such as ``std::vector<Point>``.


.. toctree::
    :maxdepth: 2

    function
    class

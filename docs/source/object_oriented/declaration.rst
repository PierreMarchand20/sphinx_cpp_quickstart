Class declaration and definition
################################

To declare a class in C++, one can use either the keyword ``class`` or ``struct`` as 


.. code-block:: cpp
    :caption: First example of custom objects
    :name: code_first_custom_objects
    :linenos:


    #include <iostream>
    #include <vector>
    #include <array>

    class Point {
        private: 
            double m_x;
            double m_y;
        public:
            Point(double x, double y): m_x(x), m_y(y) {}

    };

Data members
~~~~~~~~~~~~

Data members are the encapsulated data in the class. In :ref:`code_first_custom_objects`, the custom class ``Point`` encapsulates two ``double`` in lines 7 and 8, its *x* and *y* coordinates. They are in the ``private`` section of the class, meaning we cannot directly access them. If ones had defined a class ``PublicPoint`` such as :ref:`code_custom_objects_with_public_data_member`, then they would be directly accessible using ``.``.

.. code-block:: cpp
    :caption: Custom object with public data member
    :name: code_custom_objects_with_public_data_member
    :linenos:


    #include <iostream>

    class PublicPoint {
        public:
            double m_x;
            double m_y;
            PublicPoint(double x, double y): m_x(x), m_y(y) {}

    };

    int main(){
        PublicPoint my_public_point(1,2);
        std::cout << my_public_point.x << my_public_point.y <<"\n";
    }


.. note:: Several conventions exist on the naming of data members, we use here the prefix ``m_`` for member.

Constructors
~~~~~~~~~~~~

The class can have constructors initializing the encapsulated data, as in line 10 of :ref:`code_custom_objects_with_public_data_member`. It can actually have several constructors, we could have added another constructor taking only one ``double`` which initializes both ``m_x`` and ``m_y`` for example.

Another important point is the difference between using the constructor 

.. list-table:: Constructor vs copy for initializing data members
   :widths: 25 25

   * - .. code-block:: cpp
          :name: code_data_initialization

          PublicPoint(double x, double y): m_x(x), m_y(y) {
          }
     - .. code-block:: cpp
          :name: code_data_initialization

          PublicPoint(double x, double y) {
            m_x=x;m_y=y;
          }

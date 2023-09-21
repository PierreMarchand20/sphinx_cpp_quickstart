Classes and objets
##################

We saw in :ref:`sec_variable_types` that the language itself provides common types like numerical types (``int``, ``float``, etc.), and in :ref:`sec_cpp_standard_library`, we gave a few examples of predefined types such as ``std::vector``. One can also define its own custom types for its particular needs. In C++, a type is defined with the keywords ``class`` or ``struct``, and we call *objects* particular instances of a type.

Data members
~~~~~~~~~~~~

Data members are the encapsulated data in the class. In :ref:`code_custom_object_with_data_members`, the custom class ``Point`` encapsulates two ``double`` in lines 3 and 4, its *x* and *y* coordinates. They are in the ``private`` section of the class, meaning we cannot directly access them. If instead we had put the data member in a ``public`` section, then they would be directly accessible using ``.`` on the object once created.

.. code-block:: cpp
    :caption: Custom object with data members
    :name: code_custom_object_with_data_members
    :linenos:

    class Point {
        private: 
            double m_x;
            double m_y;

    };


.. note:: Several conventions exist on the naming of data members, we use here the prefix ``m_`` for member.

.. note:: The difference between ``class`` and ``struct`` is that by default, what is declared inside is private for the former, and public for the latter.

Constructors
~~~~~~~~~~~~

The class can have constructors initializing the encapsulated data, as in line 10 of :ref:`code_custom_object_with_constructor`. It can actually have several constructors, we could have added another constructor taking only one ``double`` which initializes both ``m_x`` and ``m_y`` for example.

.. code-block:: cpp
    :caption: First example of custom class
    :name: code_custom_object_with_constructor
    :linenos:

    class Point {
        private: 
            double m_x;
            double m_y;
        public:
            Point(double x, double y): m_x(x), m_y(y) {}

    };

Another important point is the difference between using the constructor of the data members vs using their assignment operator. In :ref:`table_data_initialization`, both approaches are possible, but the code on the left uses directly the constructor of ``double`` to initialize the values of ``m_x`` and ``m_y``, while in code on the right, the two values are initialized without a given value, and then we assign them their values. It is the same difference as in ``double a(3);`` vs ``double a; a = 3;``, which can matter if instead of ``double`` we have an object whose constructor is costly, or which does not have an assignment operator ``operator=``.



.. list-table:: Constructor vs copy for initializing data members
   :widths: 25 25
   :name: table_data_initialization

   * - .. code-block:: cpp
          :name: code_data_initialization

          Point(double x, double y): m_x(x), m_y(y) {
          }
     - .. code-block:: cpp
          :name: code_data_assignement

          Point(double x, double y) {
            m_x=x;m_y=y;
          }


Member functions
~~~~~~~~~~~~~~~~

Once we have constructed our object, we want to interact with it. Thus, we can add *member functions*, also called *methods*, that interact with the encapsulated data. In :ref:`code_custom_object_with_member_function`, we add three functions declared in the class ``Point``. Note that they can access to the encapsulated data, and they have the ``const`` keyword, which tells the compiler that these functions do not modify the encapsulated data.


.. code-block:: cpp
    :caption: Custom class with member functions
    :name: code_custom_object_with_member_function
    :linenos:

    #include <iostream>

    class Point {
        private: 
            double m_x;
            double m_y;
        public:
            Point(double x, double y): m_x(x), m_y(y) {}
            double x() const {return m_x;}
            double y() const {return m_y;}
            double norm() const {return sqrt(m_x*m_x+m_y*m_y);}

    };
    int main(){
        Point my_point(1,2);
        std::cout << my_point.x() << my_point.y() << " " << my_point.norm() <<"\n";
    }


Operator overloading
~~~~~~~~~~~~~~~~~~~~


    
Operators are symbols that performs operations, such as ``+``, ``-``, etc. They can be defined for custom classes. We will give a few examples and we refer to the `documentation <https://en.cppreference.com/w/cpp/language/operators>`__ for an exhaustive list.

Depending on the particular symbol, operators can be member functions, typical example for ``Point`` is given in lines 15-19 of :ref:`code_custom_object_with_member_function_and_operators`. 

.. important:: Member functions are always applied to an object of the class, so member functions have always an implicit argument which is the particular object on which they are applied. This implicit argument can be accessed using the keyword ``this``, which is a pointer to the object. 

.. note:: ``*this`` would be similar to ``self`` in python.


Other symbols can be defined as functions, see lines 21-26 from :ref:`code_custom_object_with_member_function_and_operators`.


.. code-block:: cpp
    :caption: Custom objects with member functions and operators
    :name: code_custom_object_with_member_function_and_operators
    :linenos:

    #include <iostream>

    class Point
    {
    private:
        double m_x;
        double m_y;

    public:
        Point(double x, double y) : m_x(x), m_y(y) {}
        double x() const { return m_x; }
        double y() const { return m_y; }
        double squared_norm() const { return m_x * m_x + m_y * m_y; }

        Point &operator*=(double t){
            m_x *= t;
            m_y *= t;
            return *this;
        }
    };
    Point operator+(const Point &u, const Point &v){
        return Point(u.x() + v.x(), u.y() + v.y());
    }
    std::ostream &operator<<(std::ostream &out, const Point &v){
        return out << v.x() << ' ' << v.y();
    }

    int main(){
        Point my_point(1,2);
        std::cout << my_point + my_point << " " << my_point.squared_norm() <<"\n";
    }





.. Life cycle of the custom object
.. ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. Some member functions and operators are special because they are related to the lifetime of objects:

.. - ``Point()``

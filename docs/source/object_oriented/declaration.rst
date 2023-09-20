Class declaration and definition
################################

To declare a class in C++, one can use either the keyword ``class`` or ``struct`` as 

Data members
~~~~~~~~~~~~

Data members are the encapsulated data in the class. In :ref:`code_first_custom_objects`, the custom class ``Point`` encapsulates two ``double`` in lines 7 and 8, its *x* and *y* coordinates. They are in the ``private`` section of the class, meaning we cannot directly access them. If ones had defined a class ``PublicPoint`` such as :ref:`code_custom_objects_with_public_data_member`, then they would be directly accessible using ``.``.

.. code-block:: cpp
    :caption: First example of custom objects
    :name: code_first_custom_objects
    :linenos:

    class Point {
        private: 
            double m_x;
            double m_y;
        public:
            Point(double x, double y): m_x(x), m_y(y) {}

    };

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

Another important point is the difference between using the constructor of the data members vs using their assignment operator. In :ref:`table_data_initialization`, both approaches are possible, but the code on the left uses directly the constructor of ``double`` to initialize the values of ``m_x`` and ``m_y``, while in code on the right, the two values are initialized without a given value, and then we assign them their values. It is the same difference as in ``double a(3);`` vs ``double a; a = 3;``, which can matter if instead of ``double`` we have an object whose constructor is costly, or which does not have an assignment operator ``operator=``.

.. list-table:: Constructor vs copy for initializing data members
   :widths: 25 25
   :name: table_data_initialization

   * - .. code-block:: cpp
          :name: code_data_initialization

          Point(double x, double y): m_x(x), m_y(y) {
          }
     - .. code-block:: cpp
          :name: code_data_initialization

          Point(double x, double y) {
            m_x=x;m_y=y;
          }


Member functions
~~~~~~~~~~~~~~~~

Once we have constructed our object, we want to interact with it. Thus, we can add *member functions* that interact with the encapsulated data. In :ref:`code_custom_object_with_member_function`, we add three functions declared in the class ``Point``. Note that they can access to the encapsulated data, and they have the ``const`` keyword, which tells the compiler that these functions do not modify the encapsulated data.


.. code-block:: cpp
    :caption: Custom objects with member functions
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


    
Operators are symbols that performs an operations, such as ``+``, ``-``, etc. They can be defined for custom objects. I will give a few examples and we refer to the `documentation <https://en.cppreference.com/w/cpp/language/operators>`__ for an exhaustive list.

Depending on the particular symbol, operators can be member functions, typical example for ``Point`` is given in lines 15-19 of :ref:`code_custom_object_with_member_function_and_operators`. Note that it returns ``*this`` whose type is a reference to ``Point``, it needs some explanations. In member functions, ``this`` is a pointer to the current object, the one on which we call the member function.

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

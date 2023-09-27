Polymorphism
############

In programming languages, `polymorphism <https://en.wikipedia.org/wiki/Polymorphism_(computer_science)>`__ is a concept referring to entities (variable, function or object) having *multiple forms*, meaning multiple definitions. We already saw two examples previously:

- :ref:`function overloading <sec_functions>`: a function can have multiple definitions depending on its number of parameters and their types,
- :ref:`sec_operator_overloading`: the same operator (for example ``+``) can have multiple meaning for different types.

In these two examples, the specific form taken by the function or the operator is chosen at compile-time. In the following, we will present *runtime polymorphism*, which is related to inheritance and overriding function members.


Inheritance
~~~~~~~~~~~

Like in many OOP languages, it is possible to define a custom class that inherits data members and methods from another class. We call the latter *base class*, and the former *derived* class. Given a custom class ``Character`` as in lines 3-14 of :ref:`code_inheritance_with_function_overriding`, one can implement a derived class as in line 15 of :ref:`code_inheritance_with_function_overriding` adding ``: public Character`` to the name of the class.

.. note:: ``public`` inheritance means that the derived class cannot access private member of the base class, but it can access public members. There is also a new section called ``protected`` in the base class, members in these sections are only accessible by derived class. In particular, ``Farmer`` can access ``m_name`` and ``m_life_points``.

.. code-block:: cpp
    :caption: Inheritance with function overriding
    :name: code_inheritance_with_function_overriding
    :linenos:

    #include <string>
    #include <iostream>
    class Character {
        protected: 
            std::string m_name;
            unsigned int m_life_points;
        public:
            Character(std::string name, unsigned int life_points): m_name(name), m_life_points(life_points) {}
            void who_am_i(){
                std::cout << "My name is "<< m_name;
                std::cout << " and I am a character with "<< m_life_points<< " life points.\n";
            }
    };
    class Farmer : public Character {
        private: 
            std::string m_sound;
        public:
            Farmer(std::string name, unsigned int life_points): Character(name,life_points) {}
            void who_am_i(){
                std::cout << "My name is "<< m_name;
                std::cout << " and I am a farmer with "<< m_life_points<< " life points.\n";
            }
    };

In this example, 

- we factorized the common parts of a character: its name and number of life points, and we specialized it for a specific case, a farmer. The advantage now is that we could add other derived classes, and benefit from the already implemented constructor of ``Character``,
- we can also override member function from the base class in a derived class, which is done for ``who_am_i``. In :ref:`code_inheritance_use_derived_class` line 3, we show how ``who_am_i`` for a ``Farmer`` instance uses the one defined in ``Farmer``.

.. code-block:: cpp
    :caption: Use derived class
    :name: code_inheritance_use_derived_class
    :linenos: 

    int main(){
        Farmer farmer("John", 10);
        farmer.who_am_i(); // My name is John and I am a farmer with 10 life points.
        Character &farmer_as_character = farmer;
        farmer_as_character.who_am_i(); // My name is John and I am a character with 10 life points.
    }

.. important:: Using a base class reference or pointer to an instance of a derived class is correct, but overridden member functions will use the base implementation. For example in :ref:`code_inheritance_use_derived_class` line 5, ``who_am_i`` use its definition from the ``Character`` class. To avoid this behavior, see :ref:`sec_virtual_member_functions`.

.. _sec_virtual_member_functions:

Virtual member functions
~~~~~~~~~~~~~~~~~~~~~~~~

We saw in :ref:`code_inheritance_use_derived_class` that the derived implementation of the overridden member function was not chosen during the execution of the program when using a reference. To make it possible, we need to define the member function as ``virtual``. 

.. code-block:: cpp
    :caption: Inheritance with virtual member function
    :name: code_inheritance_with_member_function
    :linenos:
    :emphasize-lines: 10,30

    // in character.hpp
    #include <string>
    #include <iostream>
    class Character {
        protected: 
            std::string m_name;
            unsigned int m_life_points;
        public:
            Character(std::string name, unsigned int life_points): m_name(name), m_life_points(life_points) {}
            virtual void who_am_i(){
                std::cout << "My name is "<< m_name;
                std::cout << " and I am a character with "<< m_life_points<< " life points.\n";
            }
    };
    class Farmer : public Character {
        private: 
            std::string m_sound;
        public:
            Farmer(std::string name, unsigned int life_points): Character(name,life_points) {}
            void who_am_i(){
                std::cout << "My name is "<< m_name;
                std::cout << " and I am a farmer with "<< m_life_points<< " life points.\n";
            }
    };

    int main(){
        Farmer farmer("John", 10);
        farmer.who_am_i(); // My name is John and I am a farmer with 10 life points.
        Character &farmer_as_character = farmer;
        farmer_as_character.who_am_i(); // My name is John and I am a farmer with 10 life points.
    }


Runtime polymorphism allows defining more generic functions. In our example, we could define a function taking a reference or a pointer to the base class, and it would accept any derived class from the base class (present and future!). Thus, we can write code using the interface defined by the base class.

.. code-block:: cpp
    :caption: Function taking base class as input
    :name: code_function_base_class_argument

    void who_is(Character& character){character.who_am_i();}


.. note:: It is call ``runtime polymorphism`` because, even if there is no information at compile-time, the correct derived implementation is invoked during the execution of the program.


Pure virtual member functions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

It may be better to actually not be able to construct an instance of a base class, which may not mean anything without a proper definition . In our example, it may not mean a lot to have an instance of ``Character``. To prevent this, one can use *pure virtual methods* adding ``=0`` as in line 10 of :ref:`code_inheritance_with_pure_member_function`. Then, the base class ``Character`` cannot be instantiated, the class is said to be *abstract*, and derived classes are required to implement to pure virtual member function to be instantiable.


.. code-block:: cpp
    :caption: Inheritance with pure virtual member function
    :name: code_inheritance_with_pure_member_function
    :linenos:
    :emphasize-lines: 10,30

    // in character.hpp
    #include <string>
    #include <iostream>
    class Character {
        protected: 
            std::string m_name;
            unsigned int m_life_points;
        public:
            Character(std::string name, unsigned int life_points): m_name(name), m_life_points(life_points) {}
            virtual void who_am_i() =0;
    };
    class Farmer : public Character {
        private: 
            std::string m_sound;
        public:
            Farmer(std::string name, unsigned int life_points): Character(name,life_points) {}
            void who_am_i() override{
                std::cout << "My name is "<< m_name;
                std::cout << " and I am a farmer with "<< m_life_points<< " life points.\n";
            }
    };

    int main(){
        Farmer farmer("John", 10);
        farmer.who_am_i(); // My name is John and I am a farmer with 10 life points.
        Character &farmer_as_character = farmer;
        farmer_as_character.who_am_i(); // My name is John and I am a farmer with 10 life points.
    }

.. note:: We added the keyword ``override`` in the definition of ``who_am_i`` in the derived class. It indicates to the compiler that the function is meant to override a virtual function from a base class, and compilation will fail if it does not. 
    
    It helps to detect common errors where the prototype of the function has a typo (missing argument, missing ``const`` keyword, etc.) and thus is not overriding the function from the base class but just adds a new function member using overload (using :ref:`sec_member_functions`).

.. note:: One usage of base classes with pure virtual member function is to define interfaces.

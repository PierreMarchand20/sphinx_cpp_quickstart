Class and templates
###################

Class template definition
-------------------------

Very similarly to :ref:`generic/function:function templates`, class template can be defined as :ref:`code_generic_class_template_definition`.

.. code-block:: cpp
    :caption: Class template definition
    :name: code_generic_class_template_definition
    :linenos:
    :emphasize-lines: 2

    #include <array>
    template <typename T>
    class vec3
    {
    private:
        std::array<T, 3> m_data;

    public:
        vec3(T x, T y, T z) : m_data{x, y, z} {}
        T norm() { return std::sqrt(m_data[0] * m_data[0] + m_data[1] * m_data[1] + m_data[2] * m_data[2]); }
    };

Use of class template
---------------------

A class template can always be used by explicitly specifying the type. But unlike :ref:`function templates <generic/function:use>`, *class template argument deduction* was only available starting from C++17.

.. code-block:: cpp
    :caption: Class template use
    :name: code_generic_class_template_use
    :linenos:
    :emphasize-lines: 14-15

    #include <array>
    template <typename T>
    class vec3
    {
    private:
        std::array<T, 3> m_data;

    public:
        vec3(T x, T y, T z) : m_data{x, y, z} {}
        T norm() { return std::sqrt(m_data[0] * m_data[0] + m_data[1] * m_data[1] + m_data[2] * m_data[2]); }
    };

    int main(){
        vec3<double> my_vec3(1.5,2.5,3.5);
        vec3 my_vec3(1.5,2.5,3.5); // only work for C++17 and newer
    }


Member templates
----------------

A class (template or not) can also contain template functions.

.. code-block:: cpp
    :caption: Member templates
    :name: code_generic_class_member_templates
    :linenos:
    :emphasize-lines: 12,13,22-24

    #include <iostream>
    #include <fstream>

    template <typename P>
    struct Printer
    {
    private:
        P &m_output;

    public:
        Printer(P &output) : m_output(output) {}
        template <typename T>
        void operator()(const T &a) { m_output << a << "\n"; }
    };

    int main()
    {

        Printer terminal_printer{std::cout};
        std::ofstream file("outputfile");
        Printer file_printer{file};
        terminal_printer(3);
        terminal_printer("hello");
        file_printer("test");
    }


Specialization
--------------

Class templates and member templates can also be specialized like :ref:`function templates <generic/function:specialization>`.

.. code-block:: cpp
    :caption: Member templates
    :name: code_generic_class_template_specialization
    :linenos:
    :emphasize-lines: 14,15

    #include <iostream>
    #include <fstream>

    template <typename P>
    struct Printer
    {
    private:
        P &m_output;

    public:
        Printer(P &output) : m_output(output) {}
        template <typename T>
        void operator()(const T &a) { m_output << a << "\n"; }
        template <>
        void operator()(const double &a) { m_output << std::scientific << a << "\n"; }
    };

    int main()
    {

        Printer terminal_printer{std::cout};
        std::ofstream file("outputfile");
        Printer file_printer{file};
        terminal_printer(3);
        terminal_printer("hello");
        file_printer("test");
    }
